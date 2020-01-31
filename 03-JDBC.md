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
 * GenericServlet 추상클래스 상속 받음
 * `service()` 메서드 구현 -> 내부에서 호출되는 메서드는 우리가 구현
    * `doGet()`, `doPost()`, `doHead()` 등

### 구현

</br>

# Refresh
 새로 고침 : 일정 시간이 지나고 나서 자동으로 서버에 요청을 보내는 것

 ~~그림 TO BE ADDED~~

### 응답 헤더를 이용한 구현
 ```
 response.setHeader("Refresh" , "1;url=list")
// 1초 후 현재 주소를 기준으로 list로 가라
```

### HTML의 meta 태그를 이용한 구현
```
<meta http-equiv='Refresh' content='1; url=list'>
// 반드시 head 태그 안에 선언
```

</br>

# Redirect
ex) 회원 등록 후 결과를 출력하지 않고 즉시 회원 목록 화면으로 이동하게 하는 것

 ~~그림 TO BE ADDED~~

### sendRedirect()


</br>

# 서블릿 초기화 매개변수


### DD파일에 설정

### 애노테이션으로 설정
