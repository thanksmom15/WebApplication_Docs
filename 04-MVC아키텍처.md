# MVC 아키텍처

### 올인원 방식과 문제점
 * 앞장까지 다룬 구조는 클라이언트의 요청 처리를 서블릿 홀로 담당하는 올인원 방식
 * 서블릿이 `요청 데이터 처리`, `비즈니스 로직 및 데이터 처리`, `결과 화면 생성`까지 모두 다 함
 * 규모가 크거나 업무 변경이 잦은 경우 -> 요지 보수 어려움 / 운영 비용 증가

### MVC 아키텍처
 * UI 코드와 비즈니스 코드를 분리 -> 종속성 줄이고, 재사용성 높이고, 보다 쉬운 변경 가능

 * **Model 컴포넌트**
    * 데이터 저장소와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다루는 일

 * **View 컴포넌트**
    * Model의 정보를 사용자에게 표시
    * 하나의 Model을 다양한 View에서 사용

 * **Controller 컴포넌트**
    * 사용자의 요청을 받아 Model에 변경된 상태 반영 -> 다시 View에 넘기는 역할

### 구동원리
~~그림 TOBEADDED~~
 1) **서블릿(Controller) 실행**
    * 웹 브라우저가 웹 애플리케이션 실행 요청
    * 웹 서버가 그 요청을 받아 서블릿 컨테이너(ex: 톰캣 서버)에 넘김
    * 서블릿 컨테이너는 URL을 확인하여 그 요청을 처리할 서블릿을 찾아서 실행
 2) **서블릿 : 실제 업무를 처리하는 모델 자바 객체의 메서드 호출**
    * 웹 브라우저가 보낸 데이터 저장 or 변경해야 한다면 데이터를 가공하여 값 객체(VO; Value Object) 생성
    * 모델 객체의 메서드를 호출할 때 인자값으로 넘김
 3) **모델 객체 : 데이터 다룸**
    * JDBC를 사용하여 매개변수로 넘어온 값 객체를 데이터베이스에 저장
    * 데이터베이스로부터 질의 결과를 가져와 값 객체로 만들어 반환
    * 값 객체는 객체와 객체 사이에 데이터를 전달하는 용도로 사용 -> 데이터 전송 객체(DTO; Data Transfer Object)라고 부름
 4) **서블릿은 모델 객체로부터 반환받은 값을 JSP에 전달**
 5) **JSP(View) : 웹 브라우저가 출력할 결과 화면 생성**
    * 서블릿으로부터 전달받은 값 객체를 참조하여 웹 브라우저에 출력 -> 요청처리 완료
 6) **웹 브라우저는 서버로부터 받은 응답 내용을 화면에 출력**

</br>

# Spring MVC
~~그림 TOBEADDED~~

### DispatcherServlet
 * 스프링 프레임워크가 제공하는 서블렛 클래스
 * 웹 요청과 응답의 Life Cycle 주관
 * 사용자의 요청을 받아 `HandlerMapping`에 보냄

### HandlerMapping
 * Controller URL Mapping : 웹 요청 시 해당 URL을 처리할 Controller 결정
 * 요청 URL에 해당하는 Controller 정보를 저장하는 테이블 가짐
 * `@RequestMapping("/url")`을 명시하면 해당 URL에 대한 요청이 들어왔을 때 테이블에 저장된 정보에 따라 해당 클래스 또는 메서드에 Mapping

### Controller
 * 비즈니스 로직 수행(호출)
 * 결과 데이터를 ModelAndView에 반영
 * `@Controller`, `@RequestMapping`, `@Autowired`

### Service
 * Controller에 의해 호출되어 비즈니스 로직과 트랜잭션 처리
 * DB CRUD를 담당하는 `DAO`객체를 Spring으로부터 주입 받아서, `DA`O에 DB CRUD처리 위임
 * 처리 결과를 Controller에게 반환
 * `@Service`, `@Transactional`, `@Autowired`

### DAO
 * `Service`에 의해 호출되어 쿼리 수행 후 결과 반환
 * 쿼리를 담당하는 `SqlMapClientTemplate`객체를 Spring으로부터 주입 받아서, `SqlMapClientTemplate`객체에 쿼리 수행 위임
 * 처리 결과를 `Service`에게 반환

### SqlMapClientTemplate
 * 스프링 프레임워크가 제공하는 클래스
 * `DAO`에 의해 호출되어 `SqlMapConfig.xml`의 정보를 이용하여 실제 쿼리문을 읽어와 CRUD 수행 후 결과 반환

### ViewResolver
 * Controller의 실행 결과를 보여줄 View 검색
