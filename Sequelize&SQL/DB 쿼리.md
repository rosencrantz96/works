```SQL
select * from crosscheck.user;
select * from crosscheck.snsinfo;

insert into snsinfo 
(sns_id, sns_email, provider, sns_name, sns_mobile, created_at, updated_at) 
values 
('insert', 'insert@test.com', 'kakao', '테스트', '010-1111-1111', now(), now()); 

insert into user (created_at, updated_at) values (now(), now());

set sql_safe_updates=0;
update snsinfo set user_id = 1 where sns_email = 'insert@test.com'; 
update user set email = 'rosencrantz96@kakao.com' where id = 1;

select user.email, snsinfo.sns_name, snsinfo.sns_mobile, user.gender, user.nation 
from user 
join snsinfo
on user.id = snsinfo.user_id;

-- case: sns로 회원가입 시 user 테이블에 커럼 자동 추가
-- 첫 회원가입일 때만 사용해야 함 (안 그러면 snsInfo 로우 하나마다 user 로우 생김 (1:n 관계 어긋남))  
-- 특정　테이블의　내용을　해당　테이블에　추가  
-- 근데... 굳이? 인서트 따로 주면 되지 뭐... 어차피 외래키 이어줘야 함 
-- 참고한 사이트 
-- http://www.webmadang.net/database/database.do?action=read&boardid=4001&page=1&seq=25 
insert into user (email, name, phone, created_at, updated_at) select sns_email, sns_name, sns_mobile, created_at, updated_at from snsinfo;
```