# 사용자 일반정보(User)와 1:N 관계인 사용자 sns정보(SnsInfo)

> create table user (id, email, password, name, phone, gender, nation, salt, createdAt, updatedAt)
> create table snsInfo (id, snsId, provider, flag, createdAt, updatedAt, userId(FK))

# 카카오 연동 회원가입 DB 업데이트 절차 

1. 가입된 회원인지 확인 
- 무슨 조건으로 조회를 할 것인가? 우선은 이름값과 provider로 구분 
```javascript
const tempUser = await User.findOne({
    where: {
        name: result.kakao_account.name, 
        provider: 'kakao',
    }
})
``` 
## 가입된 회원이 아닐 경우 

2.카카오에서 받아온 정보를 통해 User create()
```javascript 
// 예시 (result는 카카오에서 전달받은 사용자 정보를 담고있는 객체)
const registUser = await User.create({
    email: result.kakao_account.email,
    name: result.kakao_account.name,
    phone: result.kakao_account.phone_number,
})
```
3. user의 id값을 외래키로 갖는 snsInfo create()
```javascript 
// 예시
const newUserSnsInfo = await SnsInfo.create({
    snsId: result.id,
    provider: 'kakao',
    // 외래키 연결... 이게 되나? 
    userId: newUser.id,
})
```
→ create()를 두 번해서 테이블 각각 insert 

