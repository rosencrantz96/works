# crypto 모듈 사용법 

## crypto 
Node.js에 내장되어 있는 내장 모듈 중 하나 <br>
→ 문자열을 암호화, 복호화, 해싱할 수 있도록 도와준다 

## `createHash(), update(), digest()`
- `createHash()`: 사용할 알고리즘
- `update()`: 암호화 할 비밀번호
- `digest()`: 인코딩 방식 

### 레인보우 테이블 방지 1. salt 기법
→ salt 값을 DB에 저장해야 한다 

```javascript 
// 비밀번호 암호화 예시 
    // 1. generateAuthCode()를 통해서 12자리의 숫자로만 이루어진 salt 값을 생성
    const salt: string = generateAuthCode(100000000000, 999999999999, true);
    // 2. 비밀번호 해싱 
    const hashPassword: string = crypto
        // 알고리즘: SHA-256 방식 사용 
        .createHash('sha256')
        // 암호화 할 비밀번호 값과 salt 값을 같이 해싱
        .update(password + salt)
        // 인코딩 방식: hex 
        .digest('hex');
  
    // 3. DB에 회원 정보 등록 → salt 값도 함께 create 해준다 
    const member: User = await User.create({
        email,
        password: hashPassword,
        salt,
        });
```
salt 값을 생성할 때 자체적으로 랜덤한 숫자를 만들어서 사용할 수도 있지만, crypto 내장 함수를 활용하는 방법도 있다.

> `crypto.randomBytes()`메소드를 통해서 salt를 반환하는 함수를 작성 
```javascript 
const salt = crypto.randomBytes(34).toString('base64'); 
```
$*$ 솔트의 길이는 32바이트 이상이어야 솔트와 다이제스트를 추측하기 어렵다. 

> salt 값을 DB에 같이 저장해야 하는 이유 → 로그인 과정에서 필요

```javascript 
// 로그인 예시 
    // 1. 유저의 DB에 저장된 salt 값을 찾는다 
    const salt: User = await User.findOne({
        attributes: ['salt'],
        raw: true,
        where: {
          email,
        },
      });

    // 2. 로그인 시 입력한 패스워드와 DB에서 찾은 salt 값으로 비밀번호를 해싱해준다 
    const hashPassword: string = crypto
        .createHash('sha256')
        .update(password + +salt['salt'])
        .digest('hex');

    // 3. 입력받은 패스워드로 해싱한 값과 DB에 저장된 패스워드 값을 비교해 로그인 성공 여부를 판단한다 
    if (user.password == hashPassword) {
        /* 로그인 성공 */
    } else {
        /* 로그인 실패*/
    }
```
### 2. 키 스트레칭 기법
솔트와 패스워드를 해시함수에 넣는 과정을 반복 <br> 
→ 출력 값을 아주 느리게 산출되도록 한다 

> 암호화 방법에는 `pbkdf, scrypt, bcrypt` 세 가지가 있음 

`scrypt` 방식이 더 안전하다고 하지만 서버에 엄청난 부하가 가해지는 역효과가 발생할 수 있다 

## `pdkdf()` 
`pdkdf()` 함수는 5개의 인자가 필요한데 아래와 같다. 
```javascript 
crypto.pbkdf2(password, buffer.toString('인코딩 방식'), 반복횟수, digest길이, '암호화 알고리즘')
```
```javascript 
// 예시
crypto.pbkdf2(password, salt, 9999, 64, 'sha512', function(err, hashed) {
        if(err) {
          console.log(err)
        } else {
          console.log(hashed.toString('base64'))
        }
      })
```
> 참고 사이트 <br>
https://velog.io/@kaitlin_k/%EC%95%94%ED%98%B8%ED%99%94-%EB%B0%A9%EC%8B%9D