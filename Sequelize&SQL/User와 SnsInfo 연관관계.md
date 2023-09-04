# User : SnsInfo → 1 : N
    - user는 많은 sns 계정을 가질 수 있다(hasMany)
    - sns는 한 user에 속할 수있다(belongTo) 

## foreignKey: 두 모델이 소통하는 키 
    - userId로소통 

## 테이블 관계 설정 시퀄라이즈 
 
1. user.ts(models)
> ``user.hasMany(db.SnsInfo, { foreignKey: 'userId', sourceKey: 'id' })``

2. snsInfo.ts(models)
> ``snsInfo.belongTo(db.User, { foreignKey: 'userId', targetKey: 'id' })``


 → 'userId'라는 foreignKey를 생성: snsInfo의 테이블을 보면 user_id라는 이름의 컬럼(foreignKey)이 생성되어 있다 <br>
 → user의 sourceKey가 곧 snsInfo의 targetKey가 된다 
