<details>
<summary>ENG (English Version)</summary>

# Servlet & JSP

## 1. What Is a Servlet?  
A Java-based **dynamic web component** implemented as a `.java` class.  
Runs inside a web application server (WAS) to handle HTTP requests and responses.  
Lifecycle:  
- `init()` → `service()` → `destroy()`

## 2. JSP (Jakarta Server Pages)  
A view technology that embeds Java code within HTML.  
At compile time, JSP → Servlet → `.class`, then executed by the servlet engine.

## 3. Servlet vs. JSP  

| Criterion       | Servlet                                  | JSP                                           |
|-----------------|------------------------------------------|-----------------------------------------------|
| Primary Role    | Controller / Model                       | View                                          |
| Coding Style    | Java code with embedded HTML             | HTML with embedded Java or custom tags (JSTL) |
| Strengths       | Performance, portability, easy MVC use   | Easy UI authoring, maintainability            |
| Weaknesses      | Cumbersome for page layout               | Unsuitable for heavy logic                    |

## 4. MVC (Model-View-Controller) Pattern  
- **Controller (Servlet)**: Routes requests and controls flow  
- **Model (DAO / Service / VO)**: Business logic and database access  
- **View (JSP / JSTL / EL)**: Renders the response  
Layered separation supports maintainability and team collaboration.

## 5. Servlet API Class Hierarchy  
```
Servlet (interface)
 └─ GenericServlet (abstract)
     └─ HttpServlet
```
Key methods in `HttpServlet`: `doGet()`, `doPost()`, `doPut()`, etc.

## 6. Key Features  
- **Servlet Mapping**: `@WebServlet("/*")` or `<servlet-mapping>` in `web.xml`  
- **Filters**: Pre/post processing for encoding, authentication, logging  
- **Listeners**: Detect lifecycle events for context, session, request  
- **Session & Cookies**: Maintain user state for login, carts, etc.  
- **JSTL & EL**: Removes scriptlets from JSP for conditionals, loops, i18n, formatting  

## 7. Model2 (Modern MVC) Board Example Flow  
```
Browser → Controller (/board/list.do)
        → Service → DAO (SQL) → DB
        ← View (JSP) ← Model (List)
```
Business operations (create, update, delete, replies, paging) are encapsulated at the service layer.

## 8. Servlet Lifecycle Overview  
```
Class load → Instance creation
      ↓
   init()     (once)
      ↓
   service()  (per request)
      ↓
   destroy()  (on unload)
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 서블릿 & JSP

## 1. 서블릿이란?  
자바 기반의 **동적 웹 컴포넌트**로 `.java` 클래스로 구현됨.  
웹 애플리케이션 서버(WAS) 내부에서 HTTP 요청과 응답을 처리.  
생명 주기:  
- `init()` → `service()` → `destroy()`

## 2. JSP (자카르타 서버 페이지)  
HTML 내부에 자바 코드를 삽입하는 뷰 기술.  
컴파일 시 JSP → 서블릿 → `.class` 변환 후 서블릿 엔진에서 실행됨.

## 3. 서블릿과 JSP 비교  

| 구분            | 서블릿                                    | JSP                                           |
|-----------------|------------------------------------------|-----------------------------------------------|
| 역할            | 컨트롤러 / 모델                         | 뷰                                          |
| 코딩 방식       | HTML 포함 자바 코드                      | 자바 또는 커스텀 태그(JSTL) 포함 HTML          |
| 장점            | 성능, 이식성, MVC 활용 용이              | UI 작성 용이, 유지보수성 우수                  |
| 단점            | 페이지 레이아웃 구현에 불편                | 무거운 로직 처리 부적합                         |

## 4. MVC (모델-뷰-컨트롤러) 패턴  
- **컨트롤러 (서블릿)**: 요청 라우팅 및 흐름 제어  
- **모델 (DAO / 서비스 / VO)**: 비즈니스 로직 및 DB 처리  
- **뷰 (JSP / JSTL / EL)**: 응답 결과 렌더링  
계층 분리로 유지보수성과 협업 용이.

## 5. 서블릿 API 클래스 구조  
```
Servlet (인터페이스)
 └─ GenericServlet (추상클래스)
     └─ HttpServlet
```
`HttpServlet` 주요 메서드: `doGet()`, `doPost()`, `doPut()` 등.

## 6. 주요 기능  
- **서블릿 매핑**: `@WebServlet("/*")` 어노테이션 또는 `web.xml`의 `<servlet-mapping>`  
- **필터(Filter)**: 인코딩, 인증, 로깅 전후 처리  
- **리스너(Listener)**: 컨텍스트, 세션, 요청 생명주기 감지  
- **세션 & 쿠키**: 로그인, 장바구니 등 사용자 상태 유지  
- **JSTL & EL**: JSP 스크립틀릿 제거, 조건문, 반복문, 다국어, 포맷팅 지원  

## 7. Model2 (모던 MVC) 게시판 예시 흐름  
```
브라우저 → 컨트롤러 (/board/list.do)
        → 서비스 → DAO (SQL) → DB
        ← 뷰 (JSP) ← 모델 (리스트)
```
생성, 수정, 삭제, 답글, 페이징 등 비즈니스 로직은 서비스 계층에서 관리.

## 8. 서블릿 생명주기 개요  
```
클래스 로드 → 인스턴스 생성
      ↓
   init()     (최초 1회)
      ↓
   service()  (요청마다)
      ↓
   destroy()  (언로드 시)
```

</details>
