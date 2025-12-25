<details>
<summary>ENG (English Version)</summary>

# Expression Language (EL) and JSTL

### 1. EL Syntax & Operators
Expression Language uses `${expression}` syntax. JSP 2.0+ enabled by default (`isELIgnored="false"`). Supports arithmetic (`+`, `-`, `*`, `/` `div`, `%` `mod`), relational (`==` `eq`, `!=` `ne`, `<` `lt`, `>` `gt`, `<=` `le`, `>=` `ge`), logical (`&&` `and`, `||` `or`, `!` `not`), empty operator.

### 2. EL Implicit Objects
Access scopes via `pageScope`, `requestScope`, `sessionScope`, `applicationScope`. Parameters: `param` (single), `paramValues` (array). Headers: `header`, `headerValues`. Cookies: `Cookies`. Context path: `pageContext.request.contextPath`. Init params: `initParam`.

### 3. EL Collection Access
ArrayList: `${membersList[0].id}`, `${membersList[1].name}`. HashMap: `${membersMap.id}`, `${membersMap.membersList[0].email}`. Nested objects (has-a): `${member.addr.city}`, `${member.addr.zipcode}`.

### 4. JSTL Setup
Download taglibs-standard-*.jar files from tomcat.apache.org. Place in WEB-INF/lib. Core taglib: `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`. FMT: `prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"`. Functions: `prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"`.

### 5. Core Tags: Variable Management
`<c:set var="id" value="hong" scope="page"/>` sets attributes. `<c:remove var="age"/>` deletes attributes. Scopes: `page` (default), `request`, `session`, `application`.

### 6. Core Tags: Conditional Logic
`<c:if test="${age > 22}">Adult</c:if>` for if statements. `<c:choose><c:when test="${name == null}">No name</c:when><c:otherwise>${name}</c:otherwise></c:choose>` implements switch-like logic.

### 7. Core Tags: Iteration
`<c:forEach var="i" begin="1" end="10" step="2">${i}</c:forEach>` numeric loops. `<c:forEach var="mem" items="${membersList}">${mem.id}</c:forEach>` collection iteration. `<c:forTokens var="token" items="apple,banana,cherry" delims=",">${token}</c:forTokens>` token splitting.

### 8. Core Tags: URL & Navigation
`<c:url var="url1" value="member1.jsp"><c:param name="id" value="hong"/></c:url>` builds URLs with parameters. `<c:redirect url="member1.jsp"/>` performs redirects. `<c:out value="${param.id}" default="No ID"/>` safe output with default values.

### 9. FMT Tags: Internationalization
Properties files: `member.properties` (Korean), `memberen.properties` (English). `<fmt:setLocale value="ko_KR"/><fmt:bundle basename="resource.member"><fmt:message key="mem.name"/></fmt:bundle>` loads localized messages.

### 10. FMT Tags: Number & Date Formatting
`<fmt:formatNumber value="100000000" type="currency" currencySymbol="₩" groupingUsed="true"/>` → ₩100,000,000. `<fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"/>` formats dates. `pattern="yyyy-MM-dd HH:mm:ss"` custom patterns.

### 11. Function Tags (fn)
`<fn:length(title1)>` → 12. `<fn:toUpperCase(title1)>` → HELLO WORLD!. `<fn:substring(title1, 3, 6)>` → lo. `<fn:split(fruitList, ',')>` splits strings. `<fn:contains(title2, 'JSP')>` → true.

### 12. Practical Examples
**Login Validation:** `<c:if test="${empty param.userID}">Please enter ID</c:if>`. **Grade System:** `<c:choose><c:when test="${score >= 90}">A</c:when>...<c:otherwise>F</c:otherwise></c:choose>`. **GuGuDan:** `<c:forEach var="i" begin="1" end="9">${dan} x ${i} = ${dan*i}</c:forEach>`.

### 13. MVC Integration Pattern
Servlet: `request.setAttribute("membersList", dao.listMembers())`. JSP: `<c:forEach var="mem" items="${membersList}"><tr><td>${mem.id}</td></tr></c:forEach>`. Forward: `<jsp:forward page="memberList.jsp"/>`.

### 14. EL vs Scriptlet Comparison
**Scriptlet:** `<% String id = request.getParameter("id"); %> <%= id %>`. **EL+JSTL:** `${param.id}`, `<c:out value="${param.id}"/>`. Advantages: cleaner separation, reusable, type-safe, XSS protection via `<c:out>`.

### 15. Best Practices
Always use `<c:out>` for user input to prevent XSS. Set `scope` explicitly for `<c:set>`. Use `varStatus` in `<c:forEach>` for indices/counts. Download JSTL 1.2.5 jars for compatibility. Properties files with Properties Editor plugin for i18n.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 표현 언어(EL)와 JSTL

### 1. EL 문법 & 연산자
표현 언어는 `${expression}` 문법 사용. JSP 2.0+ 기본 활성화 (`isELIgnored="false"`). 산술(`+`, `-`, `*`, `/` `div`, `%` `mod`), 관계(`==` `eq`, `!=` `ne`, `<` `lt`, `>` `gt`, `<=` `le`, `>=` `ge`), 논리(`&&` `and`, `||` `or`, `!` `not`), empty 연산자 지원.

### 2. EL 암시적 객체
범위 접근: `pageScope`, `requestScope`, `sessionScope`, `applicationScope`. 파라미터: `param`(단일), `paramValues`(배열). 헤더: `header`, `headerValues`. 쿠키: `Cookies`. 컨텍스트 경로: `pageContext.request.contextPath`. 초기화 파라미터: `initParam`.

### 3. EL 컬렉션 접근
ArrayList: `${membersList[0].id}`, `${membersList[1].name}`. HashMap: `${membersMap.id}`, `${membersMap.membersList[0].email}`. 중첩 객체(has-a): `${member.addr.city}`, `${member.addr.zipcode}`.

### 4. JSTL 설정
tomcat.apache.org에서 taglibs-standard-*.jar 다운로드. WEB-INF/lib에 배치. Core 태그라이브러리: `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`. FMT: `prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"`. 함수: `prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"`.

### 5. Core 태그: 변수 관리
`<c:set var="id" value="hong" scope="page"/>` 속성 설정. `<c:remove var="age"/>` 속성 삭제. 범위: `page`(기본), `request`, `session`, `application`.

### 6. Core 태그: 조건문
`<c:if test="${age > 22}">성인</c:if>` if문. `<c:choose><c:when test="${name == null}">이름없음</c:when><c:otherwise>${name}</c:otherwise></c:choose>` switch 로직 구현.

### 7. Core 태그: 반복문
`<c:forEach var="i" begin="1" end="10" step="2">${i}</c:forEach>` 숫자 반복. `<c:forEach var="mem" items="${membersList}">${mem.id}</c:forEach>` 컬렉션 반복. `<c:forTokens var="token" items="apple,banana,cherry" delims=",">${token}</c:forTokens>` 토큰 분할.

### 8. Core 태그: URL & 네비게이션
`<c:url var="url1" value="member1.jsp"><c:param name="id" value="hong"/></c:url>` 파라미터 포함 URL 생성. `<c:redirect url="member1.jsp"/>` 리다이렉트. `<c:out value="${param.id}" default="ID없음"/>` 기본값 포함 안전 출력.

### 9. FMT 태그: 다국어 지원
속성파일: `member.properties`(한국어), `memberen.properties`(영어). `<fmt:setLocale value="ko_KR"/><fmt:bundle basename="resource.member"><fmt:message key="mem.name"/></fmt:bundle>` 지역화 메시지 로드.

### 10. FMT 태그: 숫자 & 날짜 포맷
`<fmt:formatNumber value="100000000" type="currency" currencySymbol="₩" groupingUsed="true"/>` → ₩100,000,000. `<fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"/>` 날짜 포맷. `pattern="yyyy-MM-dd HH:mm:ss"` 사용자 정의 패턴.

### 11. 함수 태그 (fn)
`<fn:length(title1)>` → 12. `<fn:toUpperCase(title1)>` → HELLO WORLD!. `<fn:substring(title1, 3, 6)>` → lo. `<fn:split(fruitList, ',')>` 문자열 분할. `<fn:contains(title2, 'JSP')>` → true.

### 12. 실전 예제
**로그인 검증:** `<c:if test="${empty param.userID}">ID 입력하세요</c:if>`. **성적 시스템:** `<c:choose><c:when test="${score >= 90}">A</c:when>...<c:otherwise>F</c:otherwise></c:choose>`. **구구단:** `<c:forEach var="i" begin="1" end="9">${dan} x ${i} = ${dan*i}</c:forEach>`.

### 13. MVC 통합 패턴
서블릿: `request.setAttribute("membersList", dao.listMembers())`. JSP: `<c:forEach var="mem" items="${membersList}"><tr><td>${mem.id}</td></tr></c:forEach>`. 전달: `<jsp:forward page="memberList.jsp"/>`.

### 14. EL vs 스크립틀릿 비교
**스크립틀릿:** `<% String id = request.getParameter("id"); %> <%= id %>`. **EL+JSTL:** `${param.id}`, `<c:out value="${param.id}"/>`. 장점: 명확한 분리, 재사용성, 타입 안전, `<c:out>` XSS 방지.

### 15. 모범 사례
사용자 입력은 항상 `<c:out>` 사용 (XSS 방지). `<c:set>`은 `scope` 명시. `<c:forEach>`에서 `varStatus`로 인덱스/카운트 사용. 호환성을 위해 JSTL 1.2.5 jar 다운로드. 다국어는 Properties Editor 플러그인 사용.

</details>
