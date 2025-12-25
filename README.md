<details>
<summary>ENG (English Version)</summary>

# Cookies and Sessions

### 1. HTTP Stateless Protocol Issue
- **Problem:** HTTP is stateless; each request is independent with no memory of previous requests.
- **Solutions:** Hidden form fields, URL rewriting, cookies, sessions.

### 2. Hidden Form Fields
- **Method:** Add `<input type="hidden">` fields in HTML form with extra data (address, email, phone).
- **Transmission:** Data sent via form submission (POST) along with visible form data.
- **Limitation:** Only useful for single form submission; data lost on next request.

### 3. URL Rewriting
- **Method:** Append query parameters to URL: `http://localhost:8090/second?userid=hong&userpw=1234&useraddress=seoul`.
- **Encoding:** Use URLEncoder.encode() for special characters.
- **Limitation:** URL length limit (255 chars); visible in browser history; sensitive data exposed.
- **Use Case:** Simple parameter passing between two pages.

### 4. Cookies: Client-Side Persistence
- **Definition:** Small text files stored on client PC; browser automatically sends to server on each request.
- **Size Limit:** ~4KB per cookie; domain-specific storage.
- **API:** Cookie class with getName(), getValue(), setMaxAge(), setDomain(), getPath().
- **Server Action:** response.addCookie(cookie) sends cookie to browser; request.getCookies() retrieves.

### 5. Cookie Lifecycle & Deletion
- **Persistence:** setMaxAge(seconds) determines cookie lifespan (24 * 60 * 60 = 1 day).
- **Deletion:** setMaxAge(-1) immediately expires cookie; browser removes it.
- **Use Case:** Remember login, user preferences, browsing history.

### 6. Cookie Encoding/Decoding
- **Encoding:** URLEncoder.encode(value, "utf-8") for special characters before setting.
- **Decoding:** URLDecoder.decode(value, "utf-8") when retrieving.
- **Browser Developer Tools:** F12 → Application → Cookies to inspect stored cookies.

### 7. Pop-up "Don't Show Again" with Cookies
- **JavaScript:** Check document.cookie for "notShowPopup" flag before opening pop-up.
- **Checkbox:** User checks "Don't show again"; expires cookie set for 1 month.
- **Behavior:** Next visit, pop-up doesn't show if cookie not expired.

### 8. Sessions: Server-Side State Management
- **Definition:** Server-side session object with unique session ID (JSESSIONID).
- **Lifetime:** Default 30 minutes inactivity; per-user session storage.
- **API:** HttpSession session = request.getSession(); session.setAttribute(key, value); session.getAttribute(key).
- **Storage:** Session data stored server-side; only session ID sent to client via cookie.

### 9. HttpSession Methods
- **getSession():** Returns existing session or creates new one if true (default).
- **getSession(false):** Returns existing session or null if none exists.
- **getId():** Unique session identifier (e.g., 7F8A786F5FE45997EB5474C583B03425).
- **getCreationTime()/getLastAccessedTime():** Timestamps in milliseconds since epoch.
- **getMaxInactiveInterval():** Timeout in seconds (default 1800 = 30 min).
- **isNew():** True if first access; false if previous requests exist.
- **invalidate():** Destroys session and forces new session ID.

### 10. Session Timeout Configuration
- **web.xml:** `<session-config><session-timeout>30</session-timeout></session-config>` sets timeout minutes.
- **Programmatic:** session.setMaxInactiveInterval(5) overrides default for specific session.
- **Reset on Activity:** Last access time updates on each request; inactivity counter resets.

### 11. Session Persistence (Tomcat Manager)
- **File Storage:** Sessions saved to disk in Manager pathname directory.
- **Tomcat Restart:** Sessions restored from saved state (unless disabled).
- **web.xml Setting:** `<Manager pathname=""/>` controls session file location.

### 12. URL Rewriting with Session IDs (Fallback)
- **Scenario:** Client disables cookies; session ID must travel in URL.
- **Method:** response.encodeURL(url) appends JSESSIONID to URL.
- **Format:** `http://localhost:8090/login;jsessionid=FF6228C4DC96B585DF7FF458B8DCEF1`.
- **Automatic:** Browser recognizes JSESSIONID; server extracts session from URL if no cookie.

### 13. Login Example with Session & Database
- **MemberDAO.isExisted():** Queries database with SQL DECODE to verify userid/password match.
- **LoginServlet:** On successful login, sets session attributes (isLogon=true, login.id, login.pwd).
- **ShowMember:** Checks session; if isLogon=true, displays member data; else redirects to login.
- **Logout:** session.invalidate() destroys session, forces re-login.

### 14. Session Security Considerations
- **HttpOnly Flag:** Prevents JavaScript access to session cookie (mitigates XSS attacks).
- **Secure Flag:** Cookie transmitted only over HTTPS.
- **Session Fixation:** Regenerate session ID after successful login to prevent hijacking.

### 15. Session vs Cookie Comparison
- **Session:** Server-side, secure, larger storage, per-user, timeout-based.
- **Cookie:** Client-side, visible, small storage (~4KB), domain-specific, persistent.
- **Hybrid:** Use cookies for non-sensitive data; sessions for authentication/sensitive state.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 쿠키와 세션 알아보기

### 1. HTTP 무상태 프로토콜 문제
- **문제:** HTTP는 무상태; 각 요청은 독립적으로 이전 요청 기억 없음.
- **해결책:** 숨겨진 폼 필드, URL 재작성, 쿠키, 세션.

### 2. 숨겨진 폼 필드
- **방식:** HTML 폼에 `<input type="hidden">` 필드 추가(주소, 이메일, 전화번호).
- **전송:** 폼 제출(POST) 시 보이는 데이터와 함께 전송.
- **한계:** 단일 폼 제출에만 유용; 다음 요청에서 데이터 소실.

### 3. URL 재작성
- **방식:** URL에 쿼리 파라미터 추가: `http://localhost:8090/second?userid=hong&userpw=1234&useraddress=seoul`.
- **인코딩:** URLEncoder.encode()로 특수문자 처리.
- **한계:** URL 길이 제한(255자); 브라우저 히스토리에 노출; 민감 데이터 노출.
- **사용 사례:** 두 페이지 간 단순 파라미터 전달.

### 4. 쿠키: 클라이언트 측 영속성
- **정의:** 클라이언트 PC에 저장된 작은 텍스트 파일; 브라우저가 매 요청마다 자동 전송.
- **크기 제한:** ~4KB/쿠키; 도메인별 저장.
- **API:** Cookie 클래스 (getName(), getValue(), setMaxAge(), setDomain(), getPath()).
- **서버 동작:** response.addCookie(cookie)로 브라우저에 전송; request.getCookies()로 조회.

### 5. 쿠키 생명주기 & 삭제
- **영속성:** setMaxAge(초)로 쿠키 수명 결정 (24 * 60 * 60 = 1일).
- **삭제:** setMaxAge(-1)로 즉시 만료; 브라우저가 삭제.
- **사용 사례:** 로그인 기억, 사용자 선호도, 방문 기록.

### 6. 쿠키 인코딩/디코딩
- **인코딩:** URLEncoder.encode(value, "utf-8") 특수문자 처리 후 설정.
- **디코딩:** URLDecoder.decode(value, "utf-8") 조회 시 해석.
- **브라우저 개발자 도구:** F12 → Application → Cookies로 저장된 쿠키 확인.

### 7. 쿠키로 "다시 보지 않기" 구현
- **JavaScript:** pop-up 표시 전 document.cookie에서 "notShowPopup" 플래그 확인.
- **체크박스:** 사용자가 "다시 보지 않기" 선택; 1개월 기간으로 쿠키 설정.
- **동작:** 다음 방문 시 쿠키 미만료이면 pop-up 표시 안 함.

### 8. 세션: 서버 측 상태 관리
- **정의:** 고유 세션 ID(JSESSIONID)를 가진 서버 측 세션 객체.
- **수명:** 기본 30분 무활동; 사용자별 세션 저장.
- **API:** HttpSession session = request.getSession(); session.setAttribute(key, value); session.getAttribute(key).
- **저장:** 세션 데이터는 서버에 저장; 세션 ID만 쿠키로 클라이언트에 전송.

### 9. HttpSession 메서드
- **getSession():** 기존 세션 반환 또는 새 세션 생성(true 기본값).
- **getSession(false):** 기존 세션 반환 또는 없으면 null 반환.
- **getId():** 고유 세션 식별자 (예: 7F8A786F5FE45997EB5474C583B03425).
- **getCreationTime()/getLastAccessedTime():** 에포크 이후 밀리초 타임스탬프.
- **getMaxInactiveInterval():** 타임아웃 초(기본 1800 = 30분).
- **isNew():** 첫 접근 시 true; 이전 요청 있으면 false.
- **invalidate():** 세션 파괴, 새 세션 ID 강제.

### 10. 세션 타임아웃 설정
- **web.xml:** `<session-config><session-timeout>30</session-timeout></session-config>`로 타임아웃 분 설정.
- **프로그래밍:** session.setMaxInactiveInterval(5)로 특정 세션 기본값 오버라이드.
- **활동 시 리셋:** 각 요청마다 마지막 접근 시간 업데이트; 무활동 카운터 리셋.

### 11. 세션 영속성 (Tomcat Manager)
- **파일 저장:** Manager pathname 디렉토리에 세션 디스크 저장.
- **Tomcat 재시작:** 저장된 상태에서 세션 복원(비활성화 안 함).
- **web.xml 설정:** `<Manager pathname=""/>`로 세션 파일 위치 제어.

### 12. 세션 ID를 URL에 포함 (폴백)
- **시나리오:** 클라이언트가 쿠키 비활성화; 세션 ID를 URL에 포함.
- **메서드:** response.encodeURL(url)로 JSESSIONID를 URL에 추가.
- **형식:** `http://localhost:8090/login;jsessionid=FF6228C4DC96B585DF7FF458B8DCEF1`.
- **자동 처리:** 브라우저가 JSESSIONID 인식; 서버가 쿠키 없으면 URL에서 세션 추출.

### 13. 세션 & 데이터베이스를 사용한 로그인 예시
- **MemberDAO.isExisted():** SQL DECODE로 userid/password 일치 여부 DB 조회.
- **LoginServlet:** 로그인 성공 시 세션 속성 설정 (isLogon=true, login.id, login.pwd).
- **ShowMember:** 세션 확인; isLogon=true이면 회원 데이터 표시; 아니면 로그인 페이지로 리다이렉트.
- **로그아웃:** session.invalidate()로 세션 파괴, 재로그인 강제.

### 14. 세션 보안 고려사항
- **HttpOnly 플래그:** JavaScript 세션 쿠키 접근 방지 (XSS 공격 완화).
- **Secure 플래그:** HTTPS를 통해서만 쿠키 전송.
- **세션 고정:** 로그인 성공 후 세션 ID 재생성으로 하이재킹 방지.

### 15. 세션 vs 쿠키 비교
- **세션:** 서버 측, 보안, 큰 저장소, 사용자별, 타임아웃 기반.
- **쿠키:** 클라이언트 측, 가시적, 작은 저장소(~4KB), 도메인별, 영속적.
- **하이브리드:** 비민감 데이터는 쿠키; 인증/민감 상태는 세션 사용.

</details>
