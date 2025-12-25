<details>
<summary>ENG (English Version)</summary>

# Servlet Understanding

### 1. Servlet & WAS Basics
- **Servlet Definition:** Java server-side programs executed by WAS (Tomcat, JEUS, WebLogic) to process HTTP requests and generate HTML responses.
- **JSP Relation:** JSP internally compiles to servlets; core logic handled by servlet classes.
- **WAS Role:** Receives client requests, invokes appropriate servlet, returns dynamic content.

### 2. Servlet API Hierarchy
- **Core Interfaces:** `Servlet` → `ServletConfig` → `GenericServlet` → `HttpServlet` (HTTP-specific).
- **HttpServlet Methods:** `doGet()`, `doPost()`, `doDelete()`, `doHead()` for HTTP method handling.
- **Service Flow:** `service()` routes to appropriate `doXXX()` based on HTTP method.

### 3. Servlet Lifecycle
- **Phases:** `init()` (initialization, once) → `service()`/`doXXX()` (per request) → `destroy()` (cleanup).
- **init() Timing:** Server startup or first request; single execution per servlet instance.
- **Thread Safety:** Multiple threads share one servlet instance; design stateless operations.

### 4. FirstServlet Implementation
- **Project Setup:** Eclipse Dynamic Web Project → add `servlet-api.jar` → create `sec01.ex01.FirstServlet`.
- **Core Methods:** Override `init()`, `doGet()`, `destroy()` with console logging.
- **Testing:** `http://localhost:8090/pro05/first` after Tomcat deployment.

### 5. web.xml Configuration
- **Servlet Mapping:** `<servlet>` (class registration) + `<servlet-mapping>` (URL pattern like `/first`).
- **Multiple Servlets:** Map `FirstServlet` (`/first`), `SecondServlet` (`/second`) in same web.xml.
- **Deployment:** Eclipse Servers view → Add/Remove → Tomcat → Publish changes.

### 6. Annotation-Based Mapping
- **@WebServlet:** `@WebServlet("/third")` eliminates web.xml; Eclipse wizard auto-generates.
- **Wizard Features:** Auto-creates init/destroy/doGet/doPost stubs with URL mapping.
- **Access:** `http://localhost:8090/pro05/third` directly tests annotation servlet.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 서블릿 이해하기

### 1. 서블릿 & WAS 기초
- **서블릿 정의:** WAS(Tomcat, JEUS, WebLogic)가 실행하는 자바 서버 프로그램; HTTP 요청 처리 후 HTML 응답.
- **JSP 관계:** JSP는 내부적으로 서블릿으로 컴파일; 핵심 로직은 서블릿 클래스 담당.
- **WAS 역할:** 클라이언트 요청 수신 → 적절한 서블릿 호출 → 동적 콘텐츠 반환.

### 2. Servlet API 계층
- **핵심 인터페이스:** `Servlet` → `ServletConfig` → `GenericServlet` → `HttpServlet`(HTTP 전용).
- **HttpServlet 메서드:** `doGet()`, `doPost()`, `doDelete()`, `doHead()`로 HTTP 메서드별 처리.
- **서비스 흐름:** `service()`가 HTTP 메서드에 따라 적절한 `doXXX()` 호출.

### 3. 서블릿 생명주기
- **단계:** `init()`(초기화, 1회) → `service()`/`doXXX()`(요청당) → `destroy()`(종료).
- **init() 시점:** 서버 시작/최초 요청 시; 서블릿 인스턴스당 단일 실행.
- **스레드 안전성:** 다중 스레드가 하나의 서블릿 인스턴스 공유; stateless 설계 필수.

### 4. FirstServlet 구현
- **프로젝트 설정:** Eclipse Dynamic Web Project → `servlet-api.jar` 추가 → `sec01.ex01.FirstServlet` 생성.
- **핵심 메서드:** `init()`, `doGet()`, `destroy()` 콘솔 로깅 오버라이드.
- **테스트:** Tomcat 배포 후 `http://localhost:8090/pro05/first` 접근.

### 5. web.xml 설정
- **서블릿 매핑:** `<servlet>`(클래스 등록) + `<servlet-mapping>`(URL 패턴 `/first` 등).
- **복수 서블릿:** 동일 web.xml에서 `FirstServlet`(`/first`), `SecondServlet`(`/second`) 매핑.
- **배포:** Eclipse Servers 뷰 → Add/Remove → Tomcat → 변경사항 Publish.

### 6. 애노테이션 기반 매핑
- **@WebServlet:** `@WebServlet("/third")`로 web.xml 불필요; Eclipse 마법사 자동 생성.
- **마법사 기능:** init/destroy/doGet/doPost 스텁 + URL 매핑 자동 생성.
- **접근:** `http://localhost:8090/pro05/third`로 애노테이션 서블릿 직접 테스트.

</details>
