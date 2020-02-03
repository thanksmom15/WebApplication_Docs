# JDBC

### 개념
 * Java DataBase Connectivity
 * Java 애플리케이션을 위한 DBMS 접속 인터페이스
 * [Java애플리케이션] <-> [JDBC드라이버] <-> [DB]

### 구현 (w/ GenericServlet)
 * 사용할 JDBC 드라이버 등록
 * 드라이버를 사용하여 DB에 연결
 * SQL 실행 객체 준비
 * 객체를 사용하여 DB에 SQL문 보내기
 * 가져온 데이터로 HTML을 만들어 웹 브라우저로 출력
 * 자원 해제

</br>

# javax.servlet.http.HttpServlet 추상클래스
`public class HelloWorld extends HttpServlet`

### 개념
 * `GenericServlet` 추상클래스 상속 받음
 * `service()` 메서드 구현 -> 내부에서 호출되는 메서드는 우리가 구현
    * `doGet()`, `doPost()`, `doHead()` 등

### 구현

</br>

# Refresh
 새로 고침 : 일정 시간이 지나고 나서 자동으로 서버에 요청을 보내는 것

 ~~그림 TO BE ADDED~~

### 응답 헤더를 이용한 구현
 ```
 // 1초 후 현재 주소를 기준으로 list로 가라
 response.setHeader("Refresh" , "1;url=list")
```

### HTML의 meta 태그를 이용한 구현
```
// 반드시 head 태그 안에 선언
<meta http-equiv='Refresh' content='1; url=list'>
```

</br>

# Redirect
ex) 회원 등록 후 결과를 출력하지 않고 즉시 회원 목록 화면으로 이동하게 하는 것

 ~~그림 TO BE ADDED~~

### sendRedirect()


</br>

# 서블릿 초기화 매개변수
 * 서블릿을 생성하고 초기화할 때, 즉 `init()`를 호출할 때 서블릿 컨테이너가 전달하는 데이터
 * 데이터베이스 연결 정보와 같은 정적인 데이터를 서블릿에 전달할 때 사용
 * 다른 서블릿은 참조X

### DD파일에 설정
 * web.xml만 편집하면 되기 때문에 유지보수가 쉬움

```
// Web.xml에서 설정
<init-param>
    <param-name>매개변수 이름</param-name>
    <param-value>매개변수 값</param-value>
</init-param>
```
```
// 서블릿 코드의 doGet(), doPost()에서 사용
Class.forName(this.getInitParameter("매개변수 이름"));
```

### 애노테이션으로 설정
 * 서블릿 소스 코드에 애노테이션으로 서블릿 초기화 매개변수 설정
 * 추천 X

 ```
 @WebServlet(
     name="매개변수 이름",  /* 필수 */
     value="매개변수 값",   /* 필수 */
     description="매개변수에 대한 설명" /* 선택 */
 )
 ```

</br>

# 컨텍스트 초기화 매개변수
 * 같은 웹 애플리케이션에 소속된 서블릿들이 공유하는 매개변수

### 선언
 * Web.xml파일의 시작 부분에 선언
```
<context-param>
    <param-name>매개변수 이름</param-name>
    <param-value>매개변수 값</param-value>
</context-param>
```

### 사용
 * 서블릿 코드의 `doGet()` `doPost()` 메서드에서 사용
 * `ServletContext` 객체 필요 (`HttpServlet` 상속)
 
 ```
 ServletContext sc = this.getServletContext();
 Class.forName(sc.getInintParameter("매개변수 이름"));
 ```
 
