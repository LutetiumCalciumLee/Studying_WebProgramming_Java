<details>
<summary>ENG (English Version)</summary>

# Servlet Concept

## 1. What is a Servlet?

- A **Servlet** is a **Java server-side class** generating **dynamic web content** in web applications.
- Originally the main dynamic web solution before JSP, and still the foundation for modern frameworks (e.g., Spring).
- **High platform independence, reusability, and security**.
- Runs only on the server side (not on the client).
- Typical container: **Tomcat** (Servlet/JSP environment).

***

## 2. Web Application Flow & Servlet Container

1. The **client (browser)** makes a request.
2. The **web server** passes this to the **Servlet container**.
3. The **Servlet class** processes the request (DB, business logic, etc.).
4. The result (HTML, JSON, etc.) is returned to the browser as a **response**.

- The Servlet container manages **creation, execution, destruction**, and **multithreading** for Servlets.

***

## 3. Servlet Class Hierarchy

Diagram:
```
Servlet (Interface)
  ↳ GenericServlet (Abstract class)
    ↳ HttpServlet (Class)
```
- **Servlet interface**: Defines lifecycle methods (`init()`, `service()`, `destroy()`).
- **GenericServlet**: Implements `Servlet`, generic for various protocols.
- **HttpServlet**: Specialized for HTTP and most commonly used.

***

## 4. Servlet Lifecycle Methods

- Called by the container in this order:
  1. **init()**: Once, when first loaded.
  2. **service()**: Per request; calls `doGet()`, `doPost()`, etc. as needed.
  3. **destroy()**: Once, at shutdown or reload.

- **doXXX methods**:  
  - `doGet()`, `doPost()`, `doPut()`, `doDelete()`, `doHead()`, `doOptions()`
  - Each matches a specific HTTP method.

***

## 5. Servlet Mapping

### web.xml-based mapping

- `<servlet>` and `<servlet-mapping>` tags register mapping:

```xml
<servlet>
    <servlet-name>FirstServlet</servlet-name>
    <servlet-class>com.example.FirstServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>FirstServlet</servlet-name>
    <url-pattern>/first</url-pattern>
</servlet-mapping>
```
- `servlet-name` and `url-pattern` must match.
- Typos/mismatches result in 404 errors.

### Annotation-based mapping

- Use `@WebServlet("/url")` above the class:

```java
@WebServlet("/third")
public class ThirdServlet extends HttpServlet { ... }
```
- Each URL pattern must be unique or the server won't start.
- Easier to configure and maintain.

***

## 6. Managing Multiple Servlets

- Real-world applications (e-commerce, forum, etc.) organize each core function in a **separate Servlet**.
- Each `<servlet>` or `@WebServlet` must be uniquely named and mapped.
- On code changes, restart Tomcat to apply.
- On first request, `init()` is called; later requests reuse the same instance, calling the appropriate method (`doGet()`, etc.).
- Servlets use **thread-based handling** for efficiency.

***

## 7. Hands-on Implementation Steps

1. Create a **Dynamic Web Project** in Eclipse.
2. Add `servlet-api.jar` (Tomcat library).
3. Create a package and Servlet class extending `HttpServlet`.
4. Override `init()`, `doGet()`/`doPost()`, `destroy()`, and add log outputs.
5. Configure mapping (xml or annotation).
6. Run on Tomcat, request URL, and observe your logs.

***

## 8. Using Sample Code

- Import sample source into Eclipse.
- Use a new workspace to avoid project name conflicts.
- Register the project with Tomcat and test.
- Compare your work with the sample to aid understanding.

</details>


<details>
<summary>KOR (한국어 버전)</summary>

# 서블릿 개념

## 1. 서블릿이란?

- **서블릿**은 **동적 웹 페이지** 생성을 위한 **자바 서버 측 클래스**.
- JSP 등장 전 동적 웹 구현의 대표 수단이었고, 현재도 JSP/Spring 등 근간 기술.
- **높은 이식성·재사용성·보안성**.
- 서버 측에서만 실행됨.
- 일반 컨테이너: **Tomcat** (Servlet/JSP 환경 제공).

***

## 2. 웹 애플리케이션 동작 & 서블릿 컨테이너

1. **클라이언트(브라우저)**가 요청 전송.
2. **웹 서버**가 요청을 **서블릿 컨테이너**로 전달.
3. **서블릿 클래스**가 실행되어 요청 처리(DB, 비즈니스 로직 등).
4. 결과(HTML, JSON 등)가 **응답**으로 브라우저에 반환.

- 컨테이너는 **생성·실행·소멸 및 멀티스레드 관리**.

***

## 3. 서블릿 클래스 구조

구조
```
Servlet (인터페이스)
  ↳ GenericServlet (추상 클래스)
    ↳ HttpServlet (클래스)
```
- **Servlet 인터페이스**: `init()`, `service()`, `destroy()` 등 생명주기 메서드 정의.
- **GenericServlet**: 다양한 프로토콜 지원하는 추상 구현체.
- **HttpServlet**: HTTP 전용, 실사용 비중 가장 높음.

***

## 4. 서블릿 생명주기 메서드

컨테이너 호출 흐름
1. **init()**: 최초 1회 로드 시 호출.
2. **service()**: 요청마다 호출, 내부적으로 `doGet()`, `doPost()` 등 라우팅.
3. **destroy()**: 언로드/서버 종료 시 1회.

- **doXXX 메서드**  
  - `doGet()`, `doPost()`, `doPut()`, `doDelete()`, `doHead()`, `doOptions()`
  - 각 메서드는 HTTP 방식과 1:1 대응.

***

## 5. 서블릿 매핑

### 1) web.xml 기반 매핑

- `<servlet>`, `<servlet-mapping>` 태그로 등록

```xml
<servlet>
    <servlet-name>FirstServlet</servlet-name>
    <servlet-class>com.example.FirstServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>FirstServlet</servlet-name>
    <url-pattern>/first</url-pattern>
</servlet-mapping>
```
- `servlet-name`, `url-pattern`은 반드시 일치.
- 오타/불일치 시 404 오류.

### 2) 어노테이션 기반 매핑

- 클래스 상단에 `@WebServlet("/url")` 추가

```java
@WebServlet("/third")
public class ThirdServlet extends HttpServlet { ... }
```
- URL 매핑 패턴은 반드시 고유해야 하며, 중복 시 서버 실행 불가.
- 관리와 유지보수 용이.

***

## 6. 복수 서블릿 관리

- 실제 서비스(쇼핑몰, 게시판 등)는 기능별로 **개별 서블릿** 분리.
- 각각의 `<servlet>`, `@WebServlet`은 고유 이름, 패턴으로 등록.
- 코드/설정 변경 후 톰캣 재시작 필요.
- 최초 요청 시 `init()` 호출, 이후 동일 인스턴스 반복 사용(`doGet()` 등), 효율적 **스레드 기반 처리**.

***

## 7. 실습 구현 절차

1. Eclipse에서 **Dynamic Web Project** 생성.
2. `servlet-api.jar`(톰캣 라이브러리) 추가.
3. 패키지와 `HttpServlet` 상속 클래스 생성.
4. `init()`, `doGet()`/`doPost()`, `destroy()` 메서드 구현 및 로그 출력 코드 추가.
5. web.xml 또는 어노테이션 매핑 설정.
6. Tomcat에서 실행 → URL 요청, 로그 결과 확인.

***

## 8. 샘플 코드 사용 방법

- 제공 예제 소스를 Eclipse에 임포트.
- 프로젝트명 중복 방지 위해 새 워크스페이스 권장.
- 톰캣 서버 등록, 정상 동작 확인.
- 직접 구현한 코드와 예제 비교하며 학습.

</details>
