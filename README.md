# hello-spring
### Section 1. 프로젝트 환경설정
------------
#####   (1) 프로젝트 생성
#####   (2) 라이브러리 살펴보기
#####   (3) View 환경설정
#####   (4) 빌드하고 실행하기
#####   
### Section 2. 스프링 웹 개발 기초
------------
#####   (1) 정적 컨텐츠
- 스프링 부트는 정적 컨텐츠 기능을 자동으로 제공
- 실행 순서
  1. 웹 브라우저에서 localhost:8080/hello-static.html 입력
  2. 내장 톰켓 서버가 이 요청을 받아 스프링 부트에게 넘김
  3. hello-static 관련 컨트롤러를 찾음
  4. 없으면 resources에서 static/hello-static.html 찾음 
  5. hello-static.html 찾아 반환
#####   (2) MVC와 템플릿 엔진
#####   (3) API