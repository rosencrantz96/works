코드 테스트 

- [x] sendCode 테스트 (이메일 인증 코드)
--- 
- [ ] regist 테스트  

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
    - [ ] 필수 값입력 
        - [ ] 비밀번호 입력하지 않을 경우 회원가입 불가 
            `success`: salt값이 자동으로 생성되어서 그런지 password가 자동으로 생성되어 버림... password 컬럼이 null 허용이라 그런듯 코드 수정 필요 
        - [x] 이메일을 입력하지 않을 경우 회원가입 불가
            `Fail to regist userinfo`