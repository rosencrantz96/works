```SQL
update snsinfo set sns_email = 'test@test.com' where id = 1;
update snsinfo set sns_email = 'duplicate@test.com' where id = 3;

-- JOIN을 이용한 다중, 멀티 UPDATE 
update user a join snsinfo b on a.id = b.user_id set a.email = b.sns_email; 

-- INSERT DUPLICATE = MERGE INTO 
insert into user (email, name, phone, created_at, updated_at) select sns_email, sns_name, sns_mobile, created_at, updated_at from snsinfo a 
where id in (2, 3) on duplicate key update email = a.sns_email, name = a.sns_name, phone = a.sns_mobile, created_at = a.created_at, updated_at = a.updated_at;
-- 결과: 1. WHERE 절에 ID (3)만 줬을 때는 USER에 ID 3 생성 
-- 2. WHERE 절에 ID (2, 3)을 주니... 갑자기 USER에 ID 4 생성 → SNSINFO의 ID=2 값이 USER의 ID=4 값으로 새로 생성됨 
-- TODO: WHERE 절에 들어갈 ID 숫자 가장 마지막 값에서 + 1 되는 값으로 불러와야 하는 걸까? 그래야 새로 생성되는... 으악! 어려워
-- TODO: 생각이 꼬이는 걸 보니... SNS 회원가입 플로우를 한 번 정리하고 DB에 없데이트를 어떤 식으로 할지 결정한 후 쿼리를 짜는 것이 나을 것 같다 (점심먹고 하자!) 

-- 데이터 날리고 다시 duplicate 테스트
-- 하 ㅅㅂ... 테이블 컬럼 수정한거 까먹고 있었다 이 빠가자식... 오전에 한 일 절반은 날렸쥬? 
-- 1. sns 가입한 정보 
insert into snsinfo (sns_id, provider, created_at, updated_at) values ('sns연동회원1', 'kakao', now(), now()); 
-- 2. duplicate 구문으로 user id 추가 
insert into user (created_at, updated_at) select created_at, updated_at from snsinfo a where id (1) on duplicate key update created_at = a.created_at, updated_at = a.updated_at;
-- 에러메시지: Error Code: 1305. FUNCTION crosscheck.id does not exist 
-- 하아아 ㅅㅂ...
```