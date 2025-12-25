<details>
<summary>ENG (English Version)</summary>

# Web Programming Evolution

### 1. Static Web Programming
- **Definition:** Web server stores pre-made HTML/CSS/images/JS; serves identical files to all requests.
- **User Experience:** Users see "unchanging fixed pages" regardless of request context.
- **Use Case:** Ideal for screen design and client-side event handling.
- **Limitation:** Unsuitable for real-time data (exchange rates, stock prices).

### 2. Static Web Drawback
- **Real-time Simulation:** Admin must manually edit HTML and reupload periodically (inefficient).
- **Result:** Fixed information delivery model; cannot reflect live data changes.
- **Core Components:** Web server/client/HTTP/HTML/JavaScript/CSS (HTML·JS·CSS handle UI/events).

### 3. Dynamic Web Programming
- **Solution:** Web server delegates request to **Web Application Server (WAS)**.
- **Flow:** Client request → Web server → WAS queries DB → generates dynamic HTML → returns to client.
- **Benefit:** Real-time data delivery (exchange rates, stock prices, user-specific content).

### 4. CGI (Early Dynamic Implementation)
- **Mechanism:** Creates new process per request; process loads and executes function.
- **Problem:** As users/requests increase, memory overhead grows exponentially (inefficient).
- **Limitation:** Spawning processes for each request is resource-intensive.

### 5. Modern Approach (JSP/ASP/PHP)
- **Improvement:** Thread-based execution instead of process-per-request.
- **JSP Efficiency:** Same function loads into memory once on first request, then reused for subsequent requests.
- **Advantage:** Eliminates per-request load cost; handles multiple concurrent requests efficiently.

### 6. Summary
- **Static → Dynamic Evolution:** Fixed pages → real-time data via WAS integration.
- **CGI vs JSP:** Process-heavy (CGI) → thread-efficient (JSP) for concurrent request handling.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 웹 프로그래밍 발전

### 1. 정적 웹 프로그래밍
- **정의:** 웹서버(아파치 등)에 미리 만들어 둔 HTML/CSS/이미지/JS 파일을 저장해두고 요청 시 그대로 내려주는 방식.
- **사용자 경험:** "변하지 않는 고정 페이지"를 보게 됨.
- **적합 분야:** 화면 디자인 구성, 클라이언트 이벤트 처리에 최적.
- **부적합 분야:** 환율/주가 같은 실시간 정보 제공.

### 2. 정적 웹의 문제점
- **실시간 정보 흉내:** 관리자가 주기적으로 HTML을 직접 수정해 업로드(비효율적).
- **결과:** 고정 정보 제공 중심이 됨; 실시간 데이터 반영 불가.
- **기본 구성 요소:** 웹서버/클라이언트/HTTP/HTML/JavaScript/CSS (HTML·JS·CSS는 화면과 이벤트 처리 담당).

### 3. 동적 웹 프로그래밍
- **해결책:** 웹서버가 요청을 **웹 애플리케이션 서버(WAS)**로 전달.
- **흐름:** 클라이언트 요청 → 웹서버 → WAS가 DB에서 실시간 데이터 조회 → 동적 HTML 생성 → 클라이언트에 전달.
- **장점:** 실시간 정보 제공(환율, 주가, 사용자별 맞춤 콘텐츠).

### 4. CGI (초기 동적 방식)
- **동작 방식:** 요청마다 프로세스를 새로 생성·로드하여 기능 수행.
- **문제점:** 사용자/기능 증가 시 프로세스 생성으로 메모리 부하 증가.
- **한계:** 요청당 프로세스 생성은 리소스 낭비.

### 5. 개선된 방식 (JSP/ASP/PHP)
- **개선점:** 프로세스 방식 대신 스레드 방식으로 동작.
- **JSP 효율성:** 동일 기능이 최초 요청 시 메모리에 로드되고, 이후 요청에서는 기존 로드된 기능을 재사용.
- **장점:** 매번 로드하는 비용 제거; 다수 동시 요청 환경에 적합.

### 6. 발전 요약
- **정적 → 동적 진화:** 고정 페이지 → WAS를 통한 실시간 데이터 제공.
- **CGI vs JSP:** 프로세스 방식(비효율) → 스레드 방식(효율적) 동시 요청 처리.

</details>
