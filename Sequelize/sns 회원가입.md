⟫ 사용자 일반정보(User)와 1:N 관계인 사용자 sns정보(SnsInfo)

create table user (id, email, password, name, phone, gender, nation, salt, createdAt, updatedAt)
create table snsInfo (id, snsId, snsEmail, provider, snsName, snsMobile, flag, createdAt, updatedAt, userId(FK))

1. 카카오 연동 회원가입  
    - 카카오에서 받아올 정보: 회원명, 이메일 주소, 연락처
        - 이메일 주소 → 카카오 이메일이 될 수도, 일반 이메일이 될 수도 있음 

    - 현재: 카카오 연동 회원가입 시 User 테이블에 멤버 추가 
        - 추가되는 정보: id(자동 생성), snsId, email, name, provider
    - 수정사항: 카카오 연동 회원가입 시 User 테이블과 SnsInfo 테이블 동시에 insert... 가 되나? 
        - SnsInfo[insert]: id(자동 생성), snsId, snsEmail, provider, snsName, snsMobile
        - 연관 테이블 User[insert]: id(자동 생성), email(=snsEmail), name(=snsName), phone(=snsMobile)
            - null → password, gender, nation, salt

    
* 자식 테이블이 참조할 부모 테이블의 데이터가 있어야 참조 가능
* 외래키는 null 허용
⟫ 카카오 연동 회원가입을 할 경우... insert를 동시에 하지 말고 따로따로 트랜잭션을...?
그리고 update로 fk 연결... 
