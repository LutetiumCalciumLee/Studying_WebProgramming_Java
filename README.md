<details>
<summary>ENG (English Version)</summary>

# Servlet Filters and Listeners

### 1. Attribute Scopes Overview
- **ServletContext (Application):** Application-wide scope; shared across all servlets/requests (lifetime = application lifespan).
- **HttpSession (Session):** Per-user scope; persists across multiple requests (lifetime = session timeout).
- **HttpServletRequest (Request):** Single request scope; lost after response sent (lifetime = one request).
- **API Methods:** setAttribute(name, value), getAttribute(name), removeAttribute(name) for all scopes.

### 2. Request Context Path & URI Information
- **getContextPath():** Application context (e.g., /pro10).
- **getRequestURL():** Full URL including scheme/host/port (e.g., http://localhost:8090/pro10/login).
- **getServletPath():** URL pattern mapped to servlet (e.g., /login).
- **getRequestURI():** Context path + servlet path (e.g., /pro10/login).
- **getRealPath():** Absolute filesystem path from URI (e.g., C:/tomcat/webapps/pro10/login).

### 3. Servlet URL Mapping Patterns
- **Exact Match:** `/login` maps only to http://localhost:8090/pro10/login.
- **Prefix Match:** `/first*` maps to `/first`, `/firstbase`, `/first/any/path`.
- **Extension Match:** `*.do` maps to `/login.do`, `/base.do`, `/secondbase.do`.
- **Default:** `/` handles unmapped requests (catch-all).

### 4. Filter Concept
- **Definition:** Intercepts request before servlet execution and response after servlet completes.
- **Chain Pattern:** Multiple filters can be chained; each calls chain.doFilter(request, response) to pass to next.
- **Use Cases:** Character encoding, authentication, logging, request/response modification.

### 5. Filter Lifecycle
- **init(FilterConfig):** Initializes filter; gets servlet context and filter config once.
- **doFilter(ServletRequest, ServletResponse, FilterChain):** Executes per-request; can pre-process request, invoke chain, post-process response.
- **destroy():** Cleanup when filter unloaded.

### 6. Filter Mapping Configuration
- **URL Pattern:** `<url-pattern>/*</url-pattern>` applies to all requests.
- **Specific Servlet:** `<servlet-name>LoginServlet</servlet-name>` applies only to that servlet.
- **Dispatchers:** REQUEST (default), FORWARD, INCLUDE, ERROR control when filter activates.

### 7. Encoder Filter Implementation
- **Purpose:** Enforce UTF-8 character encoding on all requests.
- **Pre-Processing:** request.setCharacterEncoding("utf-8") before chain.doFilter().
- **Logging:** System.out.println() logs context path, request URI, real path, execution time.
- **Post-Processing:** Measure execution time with System.currentTimeMillis() before/after chain.doFilter().

### 8. HttpSessionBindingListener
- **Purpose:** Detects when object bound/unbound to session.
- **Methods:** valueBound(HttpSessionBindingEvent), valueUnbound(HttpSessionBindingEvent).
- **Use Case:** Track login count (static totalUser++) when LoginImpl bound; decrement on unbind.
- **Application:** LoginServlet sets session.setAttribute("loginUser", new LoginImpl(userid, userpwd)).

### 9. HttpSessionListener
- **Purpose:** Listens to session lifecycle events.
- **Methods:** sessionCreated(HttpSessionEvent), sessionDestroyed(HttpSessionEvent).
- **Use Case:** Log session creation/destruction; maintain active user count.
- **Application:** Increment totalUser on creation; decrement on timeout/invalidation.

### 10. Login/Logout Tracking Pattern
- **Login:** LoginTest servlet sets session attribute with LoginImpl object; listener increments user count.
- **User List:** ServletContext stores ArrayList of logged-in user IDs; updates on each login/logout.
- **Logout:** LogoutTest servlet calls session.invalidate(); listener decrements count; ArrayList updated.
- **Display:** Page shows current user count and all active user IDs.

### 11. ServletContextListener
- **Methods:** contextInitialized(ServletContextEvent), contextDestroyed(ServletContextEvent).
- **Timing:** contextInitialized() runs on application startup; contextDestroyed() on shutdown.
- **Use Case:** Load global configuration, initialize database connection pool, set up caches.

### 12. Listener Registration
- **Annotation:** @WebListener on class implementing listener interface.
- **web.xml:** `<listener><listener-class>package.ClassName</listener-class></listener>`.

### 13. Event Objects in Listeners
- **HttpSessionEvent:** getSession() returns HttpSession.
- **ServletContextEvent:** getServletContext() returns ServletContext.
- **ServletRequestEvent:** getServletRequest() returns ServletRequest, getServletContext().

### 14. Filter Chain Execution Order
- **Multiple Filters:** Applied in order defined in web.xml; each calls chain.doFilter().
- **Pre-Processing:** Execute before servlet (request modification).
- **Post-Processing:** Execute after servlet (response modification/logging).
- **Example:** EncoderFilter → AuthenticationFilter → LoginServlet → response processing.

### 15. Practical Pattern: Request/Response Monitoring
- **EncoderFilter + HttpSessionListener:** Ensures UTF-8, logs requests, tracks active sessions.
- **LoginImpl (HttpSessionBindingListener):** Automatically counts logged-in users.
- **LoginTest/LogoutTest Servlets:** Manage login/logout with session binding.
- **Result:** Unified logging, automatic character encoding, user session tracking across application.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 서블릿 필터와 리스너 기능

### 1. 속성 범위 개요
- **ServletContext (애플리케이션):** 애플리케이션 전역 범위; 모든 서블릿/요청에서 공유(수명 = 애플리케이션 수명).
- **HttpSession (세션):** 사용자별 범위; 다중 요청에 걸쳐 유지(수명 = 세션 타임아웃).
- **HttpServletRequest (요청):** 단일 요청 범위; 응답 전송 후 소실(수명 = 한 요청).
- **API 메서드:** setAttribute(name, value), getAttribute(name), removeAttribute(name) (모든 범위).

### 2. 요청 컨텍스트 경로 & URI 정보
- **getContextPath():** 애플리케이션 컨텍스트 (예: /pro10).
- **getRequestURL():** 스킴/호스트/포트 포함 전체 URL (예: http://localhost:8090/pro10/login).
- **getServletPath():** 서블릿에 매핑된 URL 패턴 (예: /login).
- **getRequestURI():** 컨텍스트 경로 + 서블릿 경로 (예: /pro10/login).
- **getRealPath():** URI에서 절대 파일시스템 경로 (예: C:/tomcat/webapps/pro10/login).

### 3. 서블릿 URL 매핑 패턴
- **정확한 매칭:** `/login`은 http://localhost:8090/pro10/login만 매핑.
- **접두어 매칭:** `/first*`는 `/first`, `/firstbase`, `/first/any/path` 매핑.
- **확장자 매칭:** `*.do`는 `/login.do`, `/base.do`, `/secondbase.do` 매핑.
- **기본:** `/`은 매핑되지 않은 요청 처리(catch-all).

### 4. 필터 개념
- **정의:** 서블릿 실행 전 요청 및 완료 후 응답을 가로챔.
- **체인 패턴:** 다중 필터를 체인으로 연결; 각각 chain.doFilter(request, response)로 다음에 전달.
- **사용 사례:** 문자 인코딩, 인증, 로깅, 요청/응답 수정.

### 5. 필터 생명주기
- **init(FilterConfig):** 필터 초기화; 서블릿 컨텍스트/필터 설정 한 번 획득.
- **doFilter(ServletRequest, ServletResponse, FilterChain):** 요청당 실행; 요청 전처리, 체인 호출, 응답 후처리.
- **destroy():** 필터 언로드 시 정리.

### 6. 필터 매핑 설정
- **URL 패턴:** `<url-pattern>/*</url-pattern>`은 모든 요청에 적용.
- **특정 서블릿:** `<servlet-name>LoginServlet</servlet-name>`은 그 서블릿에만 적용.
- **Dispatcher:** REQUEST(기본), FORWARD, INCLUDE, ERROR는 필터 활성화 시점 제어.

### 7. 인코더 필터 구현
- **목적:** 모든 요청에 UTF-8 문자 인코딩 강제.
- **전처리:** chain.doFilter() 전에 request.setCharacterEncoding("utf-8").
- **로깅:** System.out.println()으로 컨텍스트 경로, 요청 URI, 실제 경로, 실행 시간 로깅.
- **후처리:** chain.doFilter() 전후 System.currentTimeMillis()로 실행 시간 측정.

### 8. HttpSessionBindingListener
- **목적:** 객체가 세션에 바인드/언바인드될 때 감지.
- **메서드:** valueBound(HttpSessionBindingEvent), valueUnbound(HttpSessionBindingEvent).
- **사용 사례:** LoginImpl 바인드 시 로그인 카운트 추적(static totalUser++); 언바인드 시 감소.
- **애플리케이션:** LoginServlet이 session.setAttribute("loginUser", new LoginImpl(userid, userpwd)) 설정.

### 9. HttpSessionListener
- **목적:** 세션 생명주기 이벤트 청취.
- **메서드:** sessionCreated(HttpSessionEvent), sessionDestroyed(HttpSessionEvent).
- **사용 사례:** 세션 생성/파괴 로깅; 활성 사용자 수 유지.
- **애플리케이션:** 생성 시 totalUser 증가; 타임아웃/무효화 시 감소.

### 10. 로그인/로그아웃 추적 패턴
- **로그인:** LoginTest 서블릿이 LoginImpl 객체로 세션 속성 설정; 리스너가 사용자 수 증가.
- **사용자 목록:** ServletContext가 로그인한 사용자 ID의 ArrayList 저장; 로그인/로그아웃 시 업데이트.
- **로그아웃:** LogoutTest 서블릿이 session.invalidate() 호출; 리스너가 수 감소; ArrayList 업데이트.
- **표시:** 페이지가 현재 사용자 수 및 활성 사용자 ID 모두 표시.

### 11. ServletContextListener
- **메서드:** contextInitialized(ServletContextEvent), contextDestroyed(ServletContextEvent).
- **시점:** contextInitialized()는 애플리케이션 시작 시 실행; contextDestroyed()는 종료 시.
- **사용 사례:** 전역 설정 로드, 데이터베이스 커넥션 풀 초기화, 캐시 설정.

### 12. 리스너 등록
- **어노테이션:** 리스너 인터페이스를 구현하는 클래스에 @WebListener.
- **web.xml:** `<listener><listener-class>package.ClassName</listener-class></listener>`.

### 13. 리스너의 이벤트 객체
- **HttpSessionEvent:** getSession()으로 HttpSession 반환.
- **ServletContextEvent:** getServletContext()로 ServletContext 반환.
- **ServletRequestEvent:** getServletRequest()로 ServletRequest, getServletContext() 반환.

### 14. 필터 체인 실행 순서
- **다중 필터:** web.xml에 정의된 순서로 적용; 각각 chain.doFilter() 호출.
- **전처리:** 서블릿 전에 실행(요청 수정).
- **후처리:** 서블릿 후에 실행(응답 수정/로깅).
- **예시:** EncoderFilter → AuthenticationFilter → LoginServlet → 응답 처리.

### 15. 실전 패턴: 요청/응답 모니터링
- **EncoderFilter + HttpSessionListener:** UTF-8 보장, 요청 로깅, 활성 세션 추적.
- **LoginImpl (HttpSessionBindingListener):** 로그인한 사용자를 자동으로 카운트.
- **LoginTest/LogoutTest 서블릿:** 세션 바인딩으로 로그인/로그아웃 관리.
- **결과:** 통합 로깅, 자동 문자 인코딩, 애플리케이션 전체의 사용자 세션 추적.

</details>
