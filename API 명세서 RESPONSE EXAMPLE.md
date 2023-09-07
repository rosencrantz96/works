> Response Success Example

```json
// 카카오 
// createToken
{
  token(string),
  "msg": {
    "Generate access token"
  },
  "status": true(boolean)
}
// registTo~
{
  registUser(object),
  newUserSnsInfo(object),
  "msg": {
    "Successfully registed userinfo & DB created"
  },
  "status": true(boolean)
}
// loginTo~
{
    userInfoToken(object),
    "msg": {
        "Login success & JWT token generated"
    },
}
// withdrawMembership
{
    "msg": {
        "Cancel the account"
    }, 
    "status": true(boolean),
}

// TODO 카카오 네이버 구글 응답 메시지, 변수명 통일시키기 오늘은 귀찮아잉... 
```


> Response Fail Example

```json
{
    status: 400 | 500,
    data: {
        "msg": {
            에러 발생 내용
        },
        "status": false(boolean),
    }
}
```
