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
- 데이터: 회원ID, 이
- 기능: 회원 등록, 조회
- 아직 DB가 선정되지 않음(가상의 시나리오)
- 일반적인 웹 애플리케이션 계층 구조
  - 컨트롤러: 웹 MVC의 컨트롤러 역할
  - 서비스: 핵심 비즈니스 로직 구현
  - 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
  - 도메인: 비즈니스 도메인 객체, 예) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨
  - 데이터베이스(DB)
- 클래스 의존 관계
  - 아직 DB가 선정되지 않아서, 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계
  - 개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 DB 사용
#####   (2) 회원 도메인과 리포지토리 만들기
- Member.java
- MemberRepository.java
- MemoryMemberRepository.java
#####   (3) 회원 리포지토리 테스트 케이스 작성
개발한 기능을 실행해서 테스트 할 때 자바의 main 메서드를 통해서 실행하거나,
웹 애플리케이션의 컨트롤러를 통해서 해당 기능을 실행한다.
이러한 방법은 준비하고 실행하는데 오래 걸리고,
반복 실행하기 어렵고 여러 테스트를 한번에 실행하기 어렵다는 단점이 있다.
자바는 JUnit이라는 프레임워크로 테스트를 실행해서 이러한 문제를 해결한다.
- MemoryMemberRepositoryTest.java
- @AfterEach : 한번에 여러 테스트를 실행하면 메모리 DB에 직전 테스트의 결과가 남을 수 있다. 이렇게
  되면 다음 이전 테스트 때문에 다음 테스트가 실패할 가능성이 있다. @AfterEach 를 사용하면 각 테스트가
  종료될 때 마다 이 기능을 실행한다. 여기서는 메모리 DB에 저장된 데이터를 삭제한다.
- 테스트는 각각 독립적으로 실행되어야 한다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아니다.
#####   (4) 회원 서비스 개발
- MemberService.java
#####   (5) 회원 서비스 테스트
- 기존에는 회원 서비스가 메모리 회원 리포지토리를 직접 생성하게 했다. 회원 리포지토리의 코드가
  회원 서비스 코드를 DI 가능하게 변경한다. (memberRepository를 직접 new해서 사용하는 것이 아니라 
  외부에서 넣어주도록 변경!)
- MemberServiceTest.java
### Section 4. 스프링 빈과 의존관계
------------
#####   (1) 컴포넌트 스캔과 자동 의존관계 설정
- 컴포넌트 스캔 원리
  - @Component 애노테이션이 있으면 스프링 빈으로 자동 등록된다.
  - @Controller 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문이다.
  - @Component 를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록된다.
    - @Controller
    - @Service
    - @Repository
- 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다(유일하게 하나만
  등록해서 공유한다) 따라서 같은 스프링 빈이면 모두 같은 인스턴스다. 설정으로 싱글톤이 아니게 설정할 수
  있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.
#####   (2) 자바 코드로 직접 스프링 빈 등록하기
- 향후 메모리 리포지토리를 다른 리포지토리로 변경할 예정이므로, 컴포넌트 스캔 방식 대신에
  자바 코드로 스프링 빈을 설정했다.
- DI(Dependency Injection)에는 필드 주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있다. 의존관계가 실행중에
  동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.
-  @Autowired 를 통한 DI는 helloController , memberService 등과 같이 스프링이 관리하는
   객체에서만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.
### Section 5. 회원 관리 예제 - 웹 MVC 개발
------------
#####   (1) 회원 웹 기능 - 홈 화면 추가
요청이 오면 관련 컨트롤러를 먼저 찾고 없으면 정적 파일을 찾기에 컨트롤러가 정적 파일보다 우선순위가 높다.
#####   (2) 회원 웹 기능 - 등록
/members/new는 GET 방식으로 들어가 members/createMemberForm으로 이동하여 createMemberForm.html이 실행되는데 이 파일은 POST 방식이다. 
등록을 누르면 url은 GET 방식을 사용했을 때와 같지만 POST 방식이기에 create 메소드가 호출된다. 
create 메소드가 실행되면 MemberForm을 통해 setName을 호출해 getName으로 member의 name을 꺼낸다.
#####   (3) 회원 웹 기능 - 조회
- 루프를 돌면서 객체를 꺼내 member에 담고, getId()와 getName()을 통해 값을 가져와 id와 name을 출력한다.
- 서버를 다시 시작하면 데이터가 사라짐 --> 파일이나 데이터베이스 저장 필요!!
### Section 6. 스프링 DB 접근 기술
------------
#####   (1) H2 데이터베이스 설치
- cmd에서 h2.bat 실행 필요
#####   (2) 순수 JDBC
- 개방-폐쇄 원칙(OCP, Open-Closed Principle) : 확장에는 열려있고, 수정, 변경에는 닫혀있다.
- 스프링의 DI (Dependencies Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현
  클래스를 변경할 수 있다.
#####   (3) 스프링 통합 테스트
스프링 컨테이너와 DB까지 연결한 통합 테스트를 진행
- @SpringBootTest : 스프링 컨테이너와 테스트를 함께 실행한다. 
- @Transactional : 테스트 케이스에 이 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고, 
테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.
#####   (4) 스프링 JdbcTemplate
- 순수 Jdbc와 동일한 환경설정을 하면 된다.
- 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야 한다.
- 생성자가 딱 하나만 있으면 스프링빈으로 등록될 때, @Autowired 생략 가능
#####   (5) JPA
- JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해준다.
- JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다.
- JPA를 사용하면 개발 생산성을 크게 높일 수 있다.
- spring-boot-starter-data-jpa 는 내부에 jdbc 관련 라이브러리를 포함한다. 따라서 jdbc는 제거해도
  된다. (build.gradle 파일)
- JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.
#####   (6) 스프링 데이터 JPA
- 스프링 데이터 JPA는 JPA를 편리하게 사용하도록 도와주는 기술이다.
- 스프링 데이터 JPA가 SpringDataJpaMemberRepository 를 스프링 빈으로 자동 등록해준다.
- 인터페이스를 통한 기본적인 CRUD (findByName() , findByEmail() 처럼 메서드 이름 만으로 조회 기능 제공, 페이징 기능 자동 제공)
### Section 7. AOP
------------
#####   (1) AOP가 필요한 상황
#####   (2) AOP 적용