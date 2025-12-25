<details>
<summary>ENG (English Version)</summary>

# Servlet Basics - Request & Response Handling

### 1. Servlet Request/Response Flow
- **HttpServletRequest API:** Methods to extract form data (getParameter, getParameterValues, getParameterNames).
- **HttpServletResponse API:** Methods to send response (setContentType, getWriter, setHeader, sendRedirect).
- **Lifecycle:** init() → doGet()/doPost() → destroy().

### 2. Form Handling with GET Method
- **HTML Form:** Create login.html with text input, password input fields.
- **getParameter():** Retrieves single parameter value by name.
- **URL Format:** `http://localhost:8090/pro06/login?userid=hong&userpw=1234`
- **LoginServlet:** Receives GET request, prints userid/userpw to console.

### 3. Multiple Values with getParameterValues()
- **Use Case:** Checkbox/multi-select with same name attribute (e.g., subject=java, subject=C, subject=JSP).
- **getParameterValues():** Returns String array for multiple selections.
- **InputServlet Example:** Loop through array with for-each to print selected subjects.

### 4. All Parameters with getParameterNames()
- **Enumeration:** Get all parameter names as Enumeration<String>.
- **while(hasMoreElements()):** Iterate through all parameters without knowing names.
- **InputServlet2 Example:** Print all name-value pairs dynamically.

### 5. Response Generation with HTML
- **setContentType():** Set MIME type (e.g., "text/html; charset=utf-8") before writing.
- **getWriter():** Get PrintWriter to send HTML content to browser.
- **LoginServlet2:** Generate HTML response with userid/userpw displayed.
- **CalcServlet:** Build HTML form and calculate currency conversion dynamically.

### 6. GET vs POST Methods
- **GET:** URL visible (max 255 chars), insecure for passwords, used for queries.
- **POST:** Data in request body, hidden from URL, suitable for sensitive data.
- **doGet() vs doPost():** Implement appropriate method based on form method attribute.
- **LoginServlet3:** Demonstrates POST method; GET request to POST servlet returns HTTP 405 error.

### 7. Unified Request Handling
- **LoginServlet4:** Implement both doGet() and doPost() with common doHandle() method.
- **Benefits:** Single code path for both GET/POST; easy maintenance and logic reuse.
- **Pattern:** Call doHandle() from both methods to reduce duplication.

### 8. Client-Side Validation
- **JavaScript:** Validate userid/userpw length before form submission.
- **Hidden Input:** Use hidden field (e.g., useraddress) to pass extra data to server.
- **LoginServlet5:** Receives and processes hidden input along with visible form data.

### 9. Practical Examples
- **LoginTest:** Basic ID/password validation with HTML response messages.
- **LoginTest2:** Check for specific ID (admin) and display role-based message.
- **GuguTest:** Dynamic multiplication table generation with HTML table/row loops.
- **GuguTest2/3:** Add alternating row colors, radio buttons, checkboxes for interactivity.

### 10. Summary
- **Form Submission:** HTML form (GET/POST) → servlet (doGet/doPost) → form data extraction.
- **Response Generation:** setContentType() → getWriter() → print HTML.
- **Data Handling:** getParameter() (single), getParameterValues() (array), getParameterNames() (all).
- **Best Practices:** Unified doHandle(), client validation, hidden fields for extra data.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 서블릿 기초 - 요청 & 응답 처리

### 1. 서블릿 요청/응답 흐름
- **HttpServletRequest API:** 폼 데이터 추출(getParameter, getParameterValues, getParameterNames).
- **HttpServletResponse API:** 응답 전송(setContentType, getWriter, setHeader, sendRedirect).
- **생명주기:** init() → doGet()/doPost() → destroy().

### 2. GET 메서드로 폼 처리
- **HTML 폼:** login.html에 텍스트/패스워드 입력 필드 작성.
- **getParameter():** 이름으로 단일 파라미터 값 추출.
- **URL 형식:** `http://localhost:8090/pro06/login?userid=hong&userpw=1234`
- **LoginServlet:** GET 요청 수신 후 userid/userpw 콘솔 출력.

### 3. getParameterValues()로 다중 값 처리
- **사용 사례:** 같은 이름 속성의 체크박스/멀티선택(subject=java, subject=C, subject=JSP).
- **getParameterValues():** 다중 선택에 대한 String 배열 반환.
- **InputServlet 예시:** 배열 순환으로 선택된 과목 출력.

### 4. getParameterNames()로 모든 파라미터 추출
- **Enumeration:** 모든 파라미터 이름을 Enumeration<String>으로 추출.
- **while(hasMoreElements()):** 이름을 모르는 상태에서 모든 파라미터 순환.
- **InputServlet2 예시:** 모든 이름-값 쌍을 동적으로 출력.

### 5. HTML로 응답 생성
- **setContentType():** MIME 타입 설정("text/html; charset=utf-8") 후 작성.
- **getWriter():** 브라우저로 HTML 콘텐츠 전송할 PrintWriter 추출.
- **LoginServlet2:** userid/userpw가 표시된 HTML 응답 생성.
- **CalcServlet:** 동적 환율 계산 폼 및 결과 생성.

### 6. GET vs POST 메서드
- **GET:** URL에 노출(255자 제한), 패스워드 보안 취약, 조회용.
- **POST:** 요청 본문에 데이터, URL 숨겨짐, 민감 데이터 적합.
- **doGet() vs doPost():** 폼 method 속성에 맞춰 구현.
- **LoginServlet3:** POST 메서드 시연; GET 요청 시 HTTP 405 오류.

### 7. 통합 요청 처리
- **LoginServlet4:** doGet()과 doPost() 모두 구현, 공통 doHandle() 호출.
- **장점:** 양쪽 메서드로 단일 코드 경로; 유지보수 용이.
- **패턴:** 중복 제거로 양쪽 메서드에서 doHandle() 호출.

### 8. 클라이언트 측 검증
- **JavaScript:** 폼 제출 전 userid/userpw 길이 검증.
- **숨겨진 입력:** 숨겨진 필드(useraddress 등)로 추가 데이터 전송.
- **LoginServlet5:** 숨겨진 입력과 폼 데이터를 함께 수신 처리.

### 9. 실전 예시
- **LoginTest:** 기본 ID/패스워드 검증 후 HTML 응답 메시지.
- **LoginTest2:** 특정 ID(admin) 확인 및 역할별 메시지 표시.
- **GuguTest:** 동적 구구단 생성(HTML 테이블/행 반복).
- **GuguTest2/3:** 행별 색 변경, 라디오 버튼, 체크박스 추가.

### 10. 요약
- **폼 제출:** HTML 폼(GET/POST) → 서블릿(doGet/doPost) → 폼 데이터 추출.
- **응답 생성:** setContentType() → getWriter() → HTML 출력.
- **데이터 처리:** getParameter()(단일), getParameterValues()(배열), getParameterNames()(전체).
- **모범 사례:** 통합 doHandle(), 클라이언트 검증, 숨겨진 필드로 추가 데이터 전송.

</details>
