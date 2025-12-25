<details>
<summary>ENG (English Version)</summary>

# Servlet Extended API Usage

### 1. Response Forwarding Mechanisms
- **redirect:** Sends HTTP 302 response; browser makes new GET request to new URL (URL visible).
- **refresh:** Sends HTTP header with delay; browser automatically redirects after N seconds.
- **location.href:** JavaScript client-side redirect; browser location bar changes.
- **forward (dispatch):** Server-side forwarding; same request/response passed to next servlet (URL unchanged).

### 2. redirect() Implementation
- **Method:** response.sendRedirect("second?name=lee") sends 302 status to browser.
- **Browser Action:** Makes new GET request to new URL.
- **Data Passing:** Parameters appended to URL visible in address bar.
- **Use Case:** Redirect to different domain, login page, or clean URL separation.

### 3. refresh() with Delay
- **Method:** response.addHeader("Refresh", "1;url=second") with 1-second delay.
- **Effect:** Browser waits 1 second, then silently redirects to new URL.
- **Advantage:** User sees original page briefly before transition.

### 4. Location.href (JavaScript)
- **Method:** JavaScript location.href = "second" executes in browser.
- **Effect:** URL bar changes, same page lifecycle behavior as redirect.

### 5. RequestDispatcher forward() / dispatch Pattern
- **Method:** RequestDispatcher dis = request.getRequestDispatcher("second"); dis.forward(request, response).
- **Behavior:** Server-side forwarding; URL unchanged in browser.
- **Request/Response:** Same request object passed; request attributes preserved.
- **Advantage:** Internal page routing without exposing internal URLs to client.

### 6. Forward vs Redirect Comparison
- **forward():** Server-side, URL unchanged, request attributes persist, faster.
- **redirect():** Client-side, URL visible, new request created, data passed via URL parameters.

### 7. Attribute Storage in Requests
- **HttpServletRequest:** request.setAttribute("key", value); request.getAttribute("key").
- **Scope:** Only within single request (forward compatible); lost after request completes.
- **redirect Issue:** Attributes lost after sendRedirect() (new request created).
- **Workaround:** Forward to JSP/servlet for immediate rendering.

### 8. ServletContext for Application Scope
- **Application-Wide:** context.setAttribute("key", value) accessible across all servlets.
- **Lookup:** context.getAttribute("key") retrieves stored object.
- **Use Case:** Shared application data, menus, configuration accessible to all requests.

### 9. ServletContext API Methods
- **getInitParameter(name):** Retrieve context-param from web.xml.
- **getInitParameterNames():** Enumerate all context-params.
- **getResourceAsStream(path):** Load file from WEB-INF or application root.
- **getRealPath(path):** Get absolute filesystem path.
- **setAttribute/getAttribute/removeAttribute:** Manage application-wide attributes.

### 10. web.xml Context Parameters
- **Definition:** `<context-param>` with `<param-name>` and `<param-value>`.
- **Access:** ServletContext context = getServletContext(); String val = context.getInitParameter("menuMember");
- **Use Case:** Database URLs, API keys, menu labels centralized in configuration.

### 11. File Loading from WEB-INF
- **getResourceAsStream("WEB-INF/bin/init.txt"):** Load file as InputStream.
- **BufferedReader:** Wrap InputStream to read line-by-line.
- **StringTokenizer:** Parse delimited data (comma, semicolon-separated).

### 12. ServletConfig (Servlet-Specific Parameters)
- **Definition:** Per-servlet initialization parameters via `<init-param>` in web.xml.
- **Access:** ServletConfig config = getServletConfig(); String val = config.getInitParameter("email");
- **Annotation:** @WebInitParam(name="email", value="admin@web.com") for declarative config.
- **Difference from ServletContext:** ServletConfig is servlet-specific; ServletContext is application-wide.

### 13. @WebServlet Annotation with Initialization
- **urlPatterns:** Multiple URL patterns ["/sInit", "/sInit2"].
- **initParams:** @WebInitParam array for servlet-specific parameters.
- **loadOnStartup:** Integer value (1 = load on server startup, init() called automatically).

### 14. load-on-startup Pattern
- **Purpose:** Initialize servlet on application startup (e.g., preload configuration, cache data).
- **init() Method:** Override to load context parameters into ServletContext attributes.
- **Timing:** Runs once when Tomcat starts (not per request).
- **Use Case:** Load global configuration, database connection pool, cache initialization.

### 15. Practical Example: Member Display
- **MemberServlet:** Retrieves list from database, calls request.setAttribute("membersList", list).
- **Forward:** dispatch.forward(request, response) passes control to ViewServlet.
- **ViewServlet:** Retrieves list via request.getAttribute("membersList"), renders HTML table.
- **Advantage:** Clean separation; MemberServlet handles logic, ViewServlet handles presentation.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 서블릿 확장 API 사용하기

### 1. 응답 전달 메커니즘
- **redirect:** HTTP 302 응답 전송; 브라우저가 새로운 URL로 GET 요청(URL 노출).
- **refresh:** HTTP 헤더로 지연 전송; 브라우저가 N초 후 자동 리다이렉트.
- **location.href:** JavaScript 클라이언트 리다이렉트; 주소창 변경.
- **forward (dispatch):** 서버 측 전달; 같은 요청/응답을 다음 서블릿으로 전달(URL 변경 없음).

### 2. redirect() 구현
- **메서드:** response.sendRedirect("second?name=lee")로 302 상태 브라우저 전송.
- **브라우저 동작:** 새 URL로 GET 요청 생성.
- **데이터 전달:** URL에 파라미터 포함(주소창에 노출).
- **사용 사례:** 다른 도메인 리다이렉트, 로그인 페이지, URL 분리.

### 3. refresh()로 지연 리다이렉트
- **메서드:** response.addHeader("Refresh", "1;url=second")로 1초 지연.
- **효과:** 브라우저가 1초 기다렸다가 조용히 새 URL로 리다이렉트.
- **장점:** 사용자가 원래 페이지 잠깐 본 후 전환.

### 4. Location.href (JavaScript)
- **메서드:** JavaScript location.href = "second" 브라우저 실행.
- **효과:** URL 바 변경, redirect와 유사한 페이지 생명주기.

### 5. RequestDispatcher forward() / dispatch 패턴
- **메서드:** RequestDispatcher dis = request.getRequestDispatcher("second"); dis.forward(request, response).
- **동작:** 서버 측 전달; 브라우저 URL 변경 없음.
- **요청/응답:** 같은 요청 객체 전달; 요청 속성 유지.
- **장점:** 내부 URL 노출 없이 서버 측 라우팅.

### 6. forward() vs redirect() 비교
- **forward():** 서버 측, URL 변경 없음, 요청 속성 유지, 빠름.
- **redirect():** 클라이언트 측, URL 노출, 새 요청 생성, URL 파라미터로 데이터 전달.

### 7. 요청 속성 저장
- **HttpServletRequest:** request.setAttribute("key", value); request.getAttribute("key").
- **범위:** 단일 요청 내 유지(forward 호환); 요청 완료 후 소실.
- **redirect 문제:** sendRedirect() 후 속성 소실(새 요청 생성).
- **해결:** JSP/서블릿으로 forward로 즉시 렌더링.

### 8. 애플리케이션 범위의 ServletContext
- **전역 범위:** context.setAttribute("key", value) 모든 서블릿에서 접근 가능.
- **조회:** context.getAttribute("key")로 저장된 객체 검색.
- **사용 사례:** 공유 애플리케이션 데이터, 메뉴, 모든 요청에서 접근 가능한 설정.

### 9. ServletContext API 메서드
- **getInitParameter(name):** web.xml의 context-param 조회.
- **getInitParameterNames():** 모든 context-param 열거.
- **getResourceAsStream(path):** WEB-INF 또는 애플리케이션 루트에서 파일 로드.
- **getRealPath(path):** 절대 파일시스템 경로 추출.
- **setAttribute/getAttribute/removeAttribute:** 애플리케이션 범위 속성 관리.

### 10. web.xml 컨텍스트 파라미터
- **정의:** `<context-param>` 내 `<param-name>`과 `<param-value>`.
- **접근:** ServletContext context = getServletContext(); String val = context.getInitParameter("menuMember");
- **사용 사례:** 데이터베이스 URL, API 키, 메뉴 레이블을 설정에 중앙집중화.

### 11. WEB-INF에서 파일 로드
- **getResourceAsStream("WEB-INF/bin/init.txt"):** 파일을 InputStream으로 로드.
- **BufferedReader:** InputStream을 감싸서 줄 단위 읽기.
- **StringTokenizer:** 구분자로 파싱(쉼표, 세미콜론 구분).

### 12. ServletConfig (서블릿별 파라미터)
- **정의:** `<init-param>`으로 web.xml의 서블릿별 초기화 파라미터.
- **접근:** ServletConfig config = getServletConfig(); String val = config.getInitParameter("email");
- **어노테이션:** @WebInitParam(name="email", value="admin@web.com")로 선언적 설정.
- **ServletContext와 차이:** ServletConfig는 서블릿별; ServletContext는 애플리케이션 전체.

### 13. 초기화 파라미터를 가진 @WebServlet 어노테이션
- **urlPatterns:** 다중 URL 패턴 ["/sInit", "/sInit2"].
- **initParams:** 서블릿별 파라미터를 위한 @WebInitParam 배열.
- **loadOnStartup:** 정수 값(1 = 서버 시작 시 로드, init() 자동 호출).

### 14. load-on-startup 패턴
- **목적:** 애플리케이션 시작 시 서블릿 초기화(설정 미리로드, 캐시 초기화).
- **init() 메서드:** 컨텍스트 파라미터를 ServletContext 속성으로 로드하려면 오버라이드.
- **시점:** Tomcat 시작 시 한 번 실행(요청당 아님).
- **사용 사례:** 전역 설정 로드, 데이터베이스 커넥션 풀, 캐시 초기화.

### 15. 실전 예시: 회원 목록 표시
- **MemberServlet:** 데이터베이스에서 목록 조회, request.setAttribute("membersList", list) 호출.
- **forward:** dispatch.forward(request, response)로 ViewServlet에 제어 전달.
- **ViewServlet:** request.getAttribute("membersList")로 목록 조회, HTML 테이블 렌더링.
- **장점:** 명확한 분리; MemberServlet은 로직, ViewServlet은 프레젠테이션 처리.

</details>
