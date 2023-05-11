# 서블릿 (Servlet)
### 설명
* Java 언어를 이용해 **웹 애플리케이션을 만들 때 사용되는 표준 기술**
* 웹 서버에서 HTTP 요청을 받아들이고 처리한 후, HTTP 응답을 생성해 클라이언트로 전송한다.
* `javax.servlet.Servlet` 인터페이스를 구현하며, 보통 HttpServlet 클래스를 상속받아 사용한다.
  * HttpServlet은 HTTP 요청을 처리하기 위해 `doGet()`, `doPost()` 등의 메서드를 제공한다.
* 서블릿을 사용하여 웹 애플리케이션을 만들 떄는 서블릿 컨테이너를 사용한다.
  * 서블릿 컨테이너는 생명주기를 관리하고, HTTP 요청을 받아 서블릿에게 전달하는 역할을 한다.
  * 대표적인 서블릿 컨테이너 : `Apache Tomcat`, `Jetty`
### 주요 기능
* HTTP 요청 처리
* 동적인 콘텐츠 생성
* HTTP 응답 생성
### 생명주기
1. 서블릿 인스턴스 생성
2. 초기화 (`init()`)
3. 클라이언트의 요청 처리 (`doGet()`, `doPost()`, ...)
4. 소멸 (`destroy()`)
### 사용 예제
```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    
    public MyServlet() {
        super();
    }
    
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String name = request.getParameter("name");
        response.setContentType("text/html");
        response.getWriter().append("Hello, " + name + "!");
    }
    
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
}
```
* HttpServlet을 상속하여 `doGet()` 메서드를 오버라이드해 HTTP GET 요청을 처리한다.
* 클라이언트가 전송한 이름 데이터는 `request.getParameter("name")` 으로 받아 처리한다.


