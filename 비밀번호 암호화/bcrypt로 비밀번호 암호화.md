# bcrypt 모듈 사용법    

`bcrypt.hashpw(password, bcrypt.gensalt())`

Blowfish 암호를 기반으로 설계된 암호화 함수로, **가장 강력한 해시 메커니즘** 중 하나

### Bcrypt가 추천되는 이유 
> 좀 더 느리게 설게뙨 암호화 방식이기 때문에 암호화 해독이 조금 더 어렵다 (SHA family는 연산 속도가 매우 빠르다! 공격자의 속도를 늦출 수록 암호화 해독이 어려워진다) 

- ISO-27001 보안 규정을 준수해야 함: **PBKDF2** 사용
- 일반적으로 규정을 준수해야 할 상황이 아니라면: **Bcrypt** 사용 (구현이 쉽고 비교적 강력하다)
- 보안 시스템을 구현하는데 돈을 많이 쓸 수 있다: **Scrypt** 사용

## bycrpt
_동작 매커니즘: 키 스트레칭_ 

> 필요한 값: 입력한 암호, salt(bcrypt를 활용한 랜덤 생성)

## `genSaltSync(rounds, minor), hashSync(data, salt)`
2차 비밀번호 설정에는 bycrpt 방식을 사용해 암호화를 했다 

```javascript 
// 1. salt를 생성한다 
    const saltRounds = 10; // default가 10이다 
    const salt = bycrpt.genSaltSync(saltRounds);

// 2. 해싱한다 
    const hashedPassword = bcrypt.hashSync(secondaryPassword, salt); 

// 3. 암호화 한 2차 비밀번호를 DB의 user 테이블에 업데이트 해준다 
    userInfo.update({
        secondaryPassword: hashedPassword,
      }, {
        // 로그인 과정에서 식별한 회원 이메일
        where: { email: accessToken['userEmail'] }
      })
```

## `compareSync(data, encrypted)`

설정한 2차 비밀번호는 마이페이지에서 회원정보를 불러올 때 bycrpt의 `compareSync()` 함수를 통해 비교한다 

```javascript 
// 1. 유저 정보를 식별 
    const userInfo: User = await User.findOne({
        where: { email: accessToken['userEmail'] },
    });

// 2. 패스워드 확인 
    if (bcrypt.compareSync(secondaryPassword, userInfo.secondaryPassword)) {
        /* 성공 */
    } else {
        /* 실패 */
    }
```

## 성능 설정 
salt를 만들 때 사용하는 rounds의 값은 아래에서 선택 가능하다 <br>
디폴트는 10이고, 적절한 rounds 값을 선택해 공격에 대비할 수 있다
```javascript
    rounds=8 // ~40hashes/sec
    rounds=9 // ~20hashes/sec
    rounds=10 // ~10hashes/sec
    rounds=11 // ~5hashes/sec
    rounds=12 // 2~3hashes/sec
    rounds=13 // ~1 sec/hash
    rounds=14 // ~1.5 sec/hash
    rounds=15 // ~3 sec/hash
    rounds=25 // ~1 hour/hash
    rounds=31 // 2~3 days/hash
``` 