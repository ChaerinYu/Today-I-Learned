[HTTP Protocol](HTTP.md)은  
**1. connectionless**  
연결을 유지하지 않습니다. request를 보내고 response를 받으면 연결이 해제됩니다.  
**2. stateless**  
상태정보를 유지하지 않습니다. 기존 request에서 뭘 했는지 알 수 없습니다.  
👉 접속 상태가 유지되지 않기 때문에, 이전 작업 내용을 기억할 필요가 있습니다.  
우리는 보완책으로 **cookie와 session**을 사용합니다.  

> **session tracking**  
> 일정 기간 동안 동일한 사용자로부터 들어오는 여러 요청들을 하나의 상태로 처리할 수 있도록 지원하는 기술


## Cookie  

cookie는 client가 저장하고 있습니다.  
1. cookie  
시스템에 만료시간과 함께 파일로 저장됩니다. 
2. session cookie  
브라우저 실행동안 브라우저 실행 메모리에 저장됩니다.  

* 브라우저는 request 시, request header에 cookie를 넣어 자동으로 server에 전송합니다. 
* key, value 형태로 구성된 string 형태입니다.
* cookie는 도메인 별로 유지되며 관리됩니다. _네이버에서 사용된 쿠키를 다음에서 사용하지 않죠!_
* cookie에는 key, value, expires, domain, path .. 가 저장됩니다.

### 쿠키 사용 목적?
1. session 관리: 서버가 알아야 할 정보들을 저장합니다. (sessionID)
2. 개인화: 사용자별 적절한 페이지를 출력해주는 역할을 합니다.
3. 트랙킹: 사용자의 행동/패턴 분석이 가능합니다.

### 쿠키의 단점?
1. 사용자가 조회, 수정, 삭제할 수 있습니다. 보안에 취약!
2. 브라우저에서 쿠키를 차단할 경우, 쿠키를 사용할 수 없습니다. 😱


## Session  

session은 server가 저장하고 있습니다.  
웹 서버에 웹 컨테이너의 상태를 유지하기 위한 정보를 저장합니다.  
방문자가 웹 서버에 접속해 있는 상태를 하나의 단위로 봅니다.  

* server측 메모리에 object 형태로 저장됩니다. server memory 허용량까지 용량 제한이 없습니다.
* 브라우저를 닫거나 sever에서 session을 삭제할 때 까지 session은 유지됩니다.
* 사용자가 직접 수정, 삭제할 수 없어 쿠키보다 보안이 좋습니다.  
* 각 client별 sessionID를 부여하여 client별 서비스를 제공합니다.
* session은 내부적으로 cookie 기술을 사용합니다. _웹 서버에 저장되는 cookie라고 생각합시다._

### session timeout?
일정 시간 동안 client의 접속이 없으면 session이 종료된다.  

### session 무효화  
```
session.invalidate();
```
개발자가 임의적으로 세션을 종료하려고 할 때 호출하는 메소드입니다.  
로그인된 계정을 로그아웃할 때 주로 사용됩니다.  


---


### 요약

| |Cookie|Session|
|:----:|-----|-----|
|저장위치|client|server|
|저장형태|key, value 형태의 string|object|
|크기|용량이 제한적이다.|server 허용량까지 가능. cookie보다 크다.|
|보안|사용자가 직접 수정, 삭제 가능하여 보안이 나쁘다.|사용자가 직접 수정, 삭제할 수 없어 cookie보다 보안이 좋다.|

