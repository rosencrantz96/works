# 1. JWT?
> 사용자 인증을 위해 사용하는 암호화 된 토큰 (토큰 방식의 일종)

    - header: {토큰 타입, 해시암호와 알고리즘에 대한 정보}
    - payload: {로그인 된 사용자의 단편적인 정보}
    - signature: {header, payload 데이터와 salt를 해싱한 값}
    JWT = header.payload.signature

> 로그인 시 만들어진 JWT를 쿠키에 넣어준다! 

# 2. JWT 토큰 생성 방법 (sign)
> jwt.sign(payload, secretkey, option)

* secretkey = salt값! -.env 파일에 담아두는 것이 좋다 
 