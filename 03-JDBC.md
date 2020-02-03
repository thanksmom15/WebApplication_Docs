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
 
</br>

# 필터

### 개념
 * 서블릿 실행 전후에 어떤 작업을 하고자 할 때 사용하는 기술
~~ 이미지 TOBEADDED

### 구현
 * javax.servlet.Filter 인터페이스 구현
 * `init()` : 필터 객체가 생성되고 나서 준비 작업을 위해 딱 한 번 호출 / 매개변수는 `FilterConfig` 객체
 * `doFilter()` : 필터와 연결된 URL에 대해 요청이 들어올 때마다 호출
 ```
 public void doFilter(
     ServeltRequest request, ServletResponse response, FilterChain nextFilter) throws IOException,
     ServletException{
         /* 서블릿이 실행되기 전에 해야 할 작업 */

         // 다음 필터 호출. 더이상 필터가 없다면 서블릿의 service()가 호출
         nextFilter.doFilter(request, response);

         /* 서블릿을 실행한 후, 클라이언트에게 응답하기 전에 해야할 작업 */
     }
 )
 ```
 * `destroy()` : 서블릿 컨테이너응 웹 애플리케이션을 종료하기 전에 필터들에 대해 `destroy()`를 호출하여 마무리 작업을 할 수 있는 기회를 줌

### 생명주기
~~ 그림 TOBEADDED

### 적용 사례
 * `nextFilter.doFilter()`을 기준으로 사전, 사후로 분류하겠음
 * 사전 작업
    * 문자집합 설정
    * 압축해제
    * 암호화된 데이터 복원
    * 로그 작성
    * 사용자 검증
    * 사용권한 확인
 * 사후 작업
    * 응답 데이터 압축
    * 응답 데이터 암호화
    * 데이터 형식 변환

### 필터 배치
 * 선언
 ```
<filter>
    <filter-name>필터 별명</filter-name>
    <filter-class>필터 클래스 이름(패키지 이름 포함)</filter-class>
    <filter-param>
        <param-name>매개변수 이름</param-name>
        <param-value>값</param-value>
    </filter-param>
</filter>
 ```
 
 * 매핑
 ```
<filter-mapping>
    <filter-name>필터 별명</filter-name>
    <url-pattern>필터 적용할 URL</url-pattern>
</filter-mapping>
 ```
