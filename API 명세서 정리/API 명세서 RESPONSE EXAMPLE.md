> Response Success Example

```json
// 카카오 
// (SNS) createToken
{
  token(string),
  "msg": "Generate access token",
  "status": true(boolean),
}
// registTo~
{
  // Email
  member(object),
  // Sns
  registUser(object),
  newUserSnsInfo(object),
  "msg": "Successfully registed userinfo & DB created",
  "status": true(boolean),
}
// (SNS) loginTo~
{
  userInfoToken(string),
  "msg": "Login success & JWT token generated",
  // google
  "msg": "Cookie for login request" | "Login success & Call the generated cookie",
  "status": true(boolean),
}
// loginToEmail
{
  token(string),
  "msg": "Login success & JWT token generated",
  "status": true(boolean),
}
// sendAuthCode
{
  "msg": "Generate authentication code", 
  "status": true(boolean),
}
// passwordResetRequest
{
  "msg": "Send reset code to email account",
  "status": true(boolean),
}
// receiveVerifyCode 
{
  "msg": "Reset code has verified", 
  "status": true(boolean),
}
// resetPassword
{
  "msg": "Password reset successfully", 
  "status": true(boolean),
}
// updatePassword
{
  token,
  userInfo,
  "msg": "Successfully changed password",
  "status": true(boolean),
}
// getMyPageInfo 
{
  userInfo,
  "msg": "Retreive userInfo",
  "status": true(boolean),
}
// updateMyPageInfo
{
  userInfo,
  "msg": "Successfully updated userinfo",
  "status": true(boolean),
}
// updateWorkInfo
{
  "msg": "Successfully created workInfo" | "Successfully updated workInfo",
  "status": true(boolean)
}
// withdrawMembership
{
  "msg": "Cancel the account", 
  "status": true(boolean),
}

// TODO 카카오 네이버 구글 응답 메시지, 변수명 통일시키기 오늘은 귀찮아잉... 
```


> Response Fail Example

```json
{
    status: 400 | 500,
    data: {
        "msg": "에러 발생 내용",
        "status": false(boolean),
    }
}
```
