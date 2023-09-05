```javascript
router.post('/regist', async (req: Request, res: Response) => {
  try {
    const { code, state, client_id, client_secret }: INaverAuthenticationCode =
      req.body;
      console.log(code, state, client_id, client_secret);
    const result = (
      await axios.get(
        `https://nid.naver.com/oauth2.0/token?grant_type=authorization_code&client_id=${client_id}&client_secret=${client_secret}&code=${code}&state=${state}`,
      )
    ).data;
    console.log(result)
    if (!result.access_token)
      res.send({ error: 'Naver Access Token create failed', status: false });
    else {
      const userInfo = (
        await axios.get('https://openapi.naver.com/v1/nid/me', {
          headers: {
            Authorization: `Bearer ${result.access_token}`,
          },
        })
      ).data;
      console.log('userInfo의 Email: '+ userInfo.response.email)
      const tempUser: User = await User.findOne({
        where: { 
          email: userInfo.response.email
        },
      });
      console.log('#tempUser: '+ tempUser)
      if (tempUser) res.send({ error: 'Exist User', status: false });
      else {
        const registUser: User = await User.create({
          email: userInfo.response.email,
          name: userInfo.response.nickname,
          phone: userInfo.response.mobile,
        });
        console.log('#registUser Regist: ' + JSON.stringify(registUser))
        const newUserSnsInfo: SnsInfo = await SnsInfo.create({
          snsId: userInfo.response.id,
          provider: 'naver',
          userEmail: registUser.email,
      })
      console.log('#newUserSnsInfo Regist: ' + JSON.stringify(newUserSnsInfo))
        res.send({ registUser, newUserSnsInfo, msg: 'regist & db created', status: true });
      }
    }
  } catch (error) {
    console.log(error);
    res.send({ error, status: false });
  }
});
```