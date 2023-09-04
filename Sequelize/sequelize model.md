# 시퀄라이즈 
- ORM (자바스크립트의 객체와 데이터베이스를 연결해주는 도구)
    - 관계형 데이터베이스, SQL은 테이블 사용, 자바스크립트에서 데이터는 객체 사용: 불일치!
    → ORM을 통해서 객체 간의 관계를 바탕으로 SQL을 자동으로 생성: 불일치 해결! 

## config: DB 연결 정보 저장하는 파일 

##  models: DB 각 테이블의 정보 및 필드타입 정의, 하나의 객체로 모음
- 워크벤치에서 테이블 정의하는 것과 같이 모델 만들어주기 
- 테이블은 MySQL 내에 미리 만들어져 있어야 한다 

# model 정의 

## super.init() 메소드 
- 테이블에 대한 설정 
- 첫 번째 인수: 테이블 컬럼에 대한 설정
- 두 번째 인수: 테이블 자체에 대한 옵션

## 테이블 옵션
```javascript
public static initModel(sequelize: Sequelize) {
    return user.init(
      {
        // 컬럼 설정    
      },
      {
        sequelize,
        timestamps: true,
        underscored: true,
        paranoid: false,
        modelName: 'User',
        tableName: 'users',
        charset: 'utf8mb4',
        collate: 'utf8mb4_general_ci',
      },
    );
  }
```
> 옵션 
- `sequelize`: static init() 메서드의 매개변수와 연결 
- `timestamps`: true일 경우 자동으로 createdAt과 updatedAt 컬럼 추가! (자동으로 날짜 컬럼이 추가되는 기능)
- `underscored`: 시퀄라이즈는 기본적으로 카멜케이스 사용 → 이를 스네이크 케이스로 변경
- `modelName`, `tableName`: 모델 이름, 데이터베이스 테이블 이름 (테이블네임은 기본적으로 모델이름의 소문자 및 복수형으로 해야 함)
- `paranoid`: true 설정 시 deletedAt 컬럼 생성(로우 복원해야 할 상황이 있을 경우 true 설정! 완전히 지워지지 않고 지운 시각 기록함)
- `charset: 'utf8mb4'`, `collate: 'utf8mb4_general_ci'`: 한글 입력받기 위한 설정 

## static associate() 메서드 
```javascript
// 예시
public static associate(db: any) {
    user.hasOne(db.WorkPlaceInfo, {
      foreignKey: 'userEmail',
      sourceKey: 'email',
    });
    user.hasMany(db.Board, { foreignKey: 'userEmail', sourceKey: 'email' });
}
```
> 다른 모델과의 관계
- 다른 모델들과 연결할 떄 사용
