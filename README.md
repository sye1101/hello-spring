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
- 동작 방식
  1. 웹 브라우저에서 localhost:8080/hello-static.html 입력
  2. 내장 톰켓 서버가 이 요청을 받아 스프링 부트에게 넘김
  3. hello-static 관련 컨트롤러를 찾음
  4. 없으면 resources에서 static/hello-static.html 찾음 
  5. hello-static.html 찾아 반환
#####   (2) MVC와 템플릿 엔진
- 동작 방식
  1. 웹브라우저에서 localhost:8080/hello-mvc를 넘김
  2. 내장 톰캣 서버를 거쳐 스프링에게 줌
  3. 스프링은 helloController에 매핑되어 있는 것을 확인 후 그 메소드를 호출
  4. 리턴할 때 이름은 hello-template으로, model에는 키는 name, 값은 spring으로 넣어 스프링에게 넘겨줌
  5. viewResolver(view 찾아주고 템플릿 엔진 연결시켜 주는 역할)가 templates/hello-template.html를 찾아 Thymeleaf 템플릿 엔진에게 처리해달라고 넘김
  6. Thymeleaf 템플릿 엔진이 변환한 html을 웹 브라우저에게 반환
- 정적 컨텐츠와는 달리 Thymeleaf 템플릿 엔진이 변환하는 과정을 거침!!
#####   (3) API
- @ResponseBody 사용 시, HTTP의 BODY에 문자 내용을 직접 반환
- viewResolver 대신에 HttpMessageConverter가 동작
- 동작 방식
  1. 웹 브라우저에서 localhost:8080/hello-api를 넘김
  2. 내장 톰캣 서버에서 스프링에게 넘김
  3. 스프링은 helloController에서 @ResponseBody가 있는 것을 확인함
  4. hello 객체를 넘겨 HttpmessageConverter이 동작을 해서 JsonConverter이 동작함 (문자일 경우는 StringConverter이 동작)
  5. 객체를 Json 스타일로 바꿔 웹 브라우저에게 응답
### Section 3. 회원 관리 예제 - 백엔드 개발
------------
#####   (1) 비즈니스 요구사항 정리
#####   (2) 회원 도메인과 리포지토리 만들기
#####   (3) 회원 리포지토리 테스트 케이스 작성
#####   (4) 회원 서비스 개발
#####   (5) 회원 서비스 테스트
### Section 4. 스프링 빈과 의존관계
------------
#####   (1) 컴포넌트 스캔과 자동 의존관계 설정
#####   (2) 자바 코드로 직접 스프링 빈 등록하기