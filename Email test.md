# `authController`
- [x] **sendAuthCode 테스트 (이메일 인증 코드)**
```json
{
    "email": "topgdvidsyb@gmail.com"
}
```
- [x] **passwordResetRequest 테스트 (비밀번호 초기화)**
```json
{
    "email": "topgdvidsyb@gmail.com"
}
```
--- 
# `registController `
- [x] **회원가입 테스트**  

```json
{
    "email": "topgdvidsyb@gmail.com",
    "password": "1234",
    "name": "권수경",
    "phone": "010-9908-0803",
    "gender": "여성",
    "nation": "한국"
}
// success
```
- [x] 중복 이메일 방지 
- [x] 필수 값입력 
    - [x] 비밀번호 입력하지 않을 경우 회원가입 불가 <br /> 
        - salt값이 자동으로 생성되어서 그런지 password가 자동으로 생성되어 버림... password 컬럼이 null 허용이라 그런듯 코드 수정 필요 
    - [x] 이메일을 입력하지 않을 경우 회원가입 불가 <br />`Fail to regist userinfo`
- [x] salt값 생성 시 util의 `generateAuthCode()` 메서드 사용 
    - string으로만 선언이 되는데 그 이유는 모르겠음... password가 string이라서 그런건가? 
    - 모델을 바꿔도 선언이 불가하다고 하여 그냥 `string`으로 선언 
---
# `loginController`
- [x] **로그인 테스트** 
```json
{
    "email": "topgdvidsyb@gmail.com",
    "password": "1234"
}
// success (JWT 토큰 생성 완료)
```
- [x] 비회원 로그인 테스트 
- [x] 잘못된 비밀번호 테스트    
--- 
# `myPageController`
- [ ] **2차 비밀번호 설정 테스트** 
```json
{
    "token": "",
    "secondaryPassword": "102938"
}
```
- [ ] 2차 비밀번호 설정하는 api가 없는 것 같아서 생성  
    - [x] user 테이블에 2차 비밀번호 데이터 업데이트 
    - [ ] 2차 비밀번호 암호화 (bcrypt 방식)

<br>

- [ ] **마이페이지 정보 불러오기** 

<br>

- [x] **마이페이지 update 테스트** 
```json
{
    "token": "",
    "name": "권수경",
    "phone": "010-1111-1111",
    "gender": "여자",
    "nation": "미국"
}
// success 
```
- [x] token 값이 없을 때 → json web token error / no login 
- [x] 모든 값을 바꾸지 않을 때 → 바꾸고 싶은 정보만 update 성공 

<br />
- [ ] 직장정보 업데이트 

```json
{
    "email": "topgdvidsyb@gmail.com",
    "name": "권수경",
    "category": "IT 회사사",
    "workName": "CROSSCHECK",
    "workAddress": "서울시 어쩌구",
    "purpose": "재태크",
    "sourceFunds": "근로소득"
}
// success 
```

- [x] 직장정보 인서트
- [x] 직장정보 업데이트 
    - [ ] 원하는 정보만 업데이트 (null 값이 새로 들어가지 않도록)
--- 
# `passwordController`

- [ ] **비밀번호 초기화 테스트: resetPassword**
```json
{
    "email": "topgdvidsyb@gmail.com",
    "code": "870465",
    "newPassword": "102938"
}
// success 
```
- [x] resetCode가 없을 때 비밀번호 변경 실패 확인 
- [x] 잘못된 코드를 입력했을 때 
- [x] resetCode가 있을 때 
    - [x] 비밀번호 변경 확인 (이전 비밀번호 && 바뀐 비밀번호 로그인 확인)
    - [x] passwordReset 테이블 destroy 확인 
    
<br />

- [ ] **비밀번호 재설정 테스트: updatePassword**
