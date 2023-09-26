## DELETE 요청 시 메시지를 담을 곳 

보안을 위해서 Request Header parameter에 담는 것을 추천한다 

https://humblego.tistory.com/18

## PUT 메소드와 PATCH 메소드 

PUT 메소드는 리소스를 대체(수정)할 때 사용 <br>
**해당 리소스가 없으면 생성(등록)**한다 = 리소스 덮어쓰기 <br>

PATCH 메소드는 **리소스를 일부만 변경**할 때 사용 <br>
→ 덮어씌워지는 것이 아닌 해당 리소스만 변경된다 
