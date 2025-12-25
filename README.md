<details>
<summary>ENG (English Version)</summary>

# Servlet Business Logic Processing

### 1. Separating Business Logic from Presentation
- **Problem:** Servlet handles both UI rendering and database queries (poor separation of concerns).
- **Solution:** Split into MemberVO (model), MemberDAO (database logic), MemberServlet (controller).
- **Pattern:** Servlet delegates DB operations to DAO, receives data, renders HTML response.

### 2. VO (Value Object) & DAO (Data Access Object) Pattern
- **MemberVO:** Encapsulates member data (id, pwd, name, email, joinDate) as POJO.
- **MemberDAO:** Handles all database operations (SELECT, INSERT, DELETE) with SQL queries.
- **Benefits:** Code reusability, testability, clear separation of concerns.

### 3. Basic JDBC Connection & Statement
- **Setup:** Add ojdbc6.jar (Oracle JDBC driver) to WEB-INF/lib.
- **Connection:** Class.forName() → DriverManager.getConnection(url, user, pwd).
- **Statement:** con.createStatement() → stmt.executeQuery(SQL).
- **ResultSet:** Loop rs.next() → extract columns with rs.getString/getInt/getDate.

### 4. PreparedStatement for Safety
- **Advantage:** Prevents SQL injection by parameterizing queries.
- **Syntax:** String query = "SELECT * FROM tmember WHERE id = ?" with pstmt.setString(1, id).
- **Performance:** Reusable for multiple executions; DBMS pre-compiles once.
- **Implementation:** con.prepareStatement() → pstmt.setString/setInt → executeQuery().

### 5. DataSource & Connection Pooling
- **Problem:** Opening/closing connections per request is expensive.
- **Solution:** DataSource maintains pool of reusable connections.
- **JNDI (Java Naming & Directory Interface):** Key-value registry for resource lookup.
- **Setup:** Tomcat context.xml defines DataSource; DAO looks up via InitialContext.

### 6. Configuring DataSource in Tomcat
- **File:** CATALINA_HOME/conf/context.xml with `<Resource>` element.
- **Key Attributes:** 
  - `name="jdbcoracle"` (JNDI lookup key)
  - `driverClassName` (Oracle driver class)
  - `url` (JDBC connection URL)
  - `username`, `password`, `maxActive`, `maxWait`

### 7. Using DataSource in DAO
- **Lookup:** Context ctx = new InitialContext(); DataSource ds = (DataSource)ctx.lookup("java:comp/env/jdbcoracle");
- **Connection:** con = ds.getConnection() (no credentials needed; pool handles it).
- **Advantage:** Automatic connection recycling; improved performance.

### 8. CRUD Operations
- **Create (INSERT):** PreparedStatement with pstmt.setString/setInt → executeUpdate().
- **Read (SELECT):** ResultSet iteration with rs.next() → extract fields.
- **Update (UPDATE):** PreparedStatement with WHERE clause → executeUpdate().
- **Delete (DELETE):** PreparedStatement with WHERE clause → executeUpdate().

### 9. Form-Based Member Management
- **memberForm.html:** Client-side validation (JavaScript) before form submission.
- **Hidden input:** command parameter (addMember/delMember) to route server logic.
- **Servlet routing:** if(command.equals("addMember")) → DAO.addMember(); if(command.equals("delMember")) → DAO.delMember().

### 10. Debugging in Eclipse
- **Breakpoint:** Double-click line number to set breakpoint in MemberServlet.doHandle().
- **Debug Mode:** Right-click project → Debug As → Debug on Server.
- **Step Controls:** F5 (Step Into), F6 (Step Over), F8 (Resume).
- **Variables View:** Inspect variable values at runtime (command, id, pwd, etc.).

### 11. Database Setup
- **SQL Creator:** Create table tmember with columns (id, pwd, name, email, joinDate).
- **Sample Data:** Insert test records (hong, lee, kim) with SQL Developer.
- **Verification:** SELECT * FROM tmember to confirm data insertion.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 서블릿 비즈니스 로직 처리

### 1. 프레젠테이션과 비즈니스 로직 분리
- **문제:** 서블릿이 UI 렌더링과 DB 조회를 동시 처리 (관심사 미분리).
- **해결:** MemberVO(데이터), MemberDAO(DB 로직), MemberServlet(제어)로 분리.
- **패턴:** 서블릿이 DAO에 DB 작업 위임 → 데이터 수신 → HTML 렌더링.

### 2. VO(Value Object) & DAO(Data Access Object) 패턴
- **MemberVO:** 회원 데이터(id, pwd, name, email, joinDate)를 POJO로 캡슐화.
- **MemberDAO:** 모든 DB 작업(SELECT, INSERT, DELETE)을 SQL로 처리.
- **장점:** 코드 재사용성, 테스트 용이성, 관심사 명확히 분리.

### 3. 기본 JDBC 연결 & Statement
- **설정:** ojdbc6.jar(Oracle JDBC 드라이버)를 WEB-INF/lib에 추가.
- **연결:** Class.forName() → DriverManager.getConnection(url, user, pwd).
- **Statement:** con.createStatement() → stmt.executeQuery(SQL).
- **ResultSet:** rs.next() 순환 → rs.getString/getInt/getDate로 컬럼 추출.

### 4. PreparedStatement로 보안 강화
- **장점:** SQL 인젝션 방지; 쿼리 파라미터화.
- **문법:** String query = "SELECT * FROM tmember WHERE id = ?" + pstmt.setString(1, id).
- **성능:** 재실행 가능; DBMS가 한 번만 프리컴파일.
- **구현:** con.prepareStatement() → pstmt.setString/setInt → executeQuery().

### 5. DataSource & 커넥션 풀링
- **문제:** 요청마다 연결 생성/해제는 비싸다.
- **해결:** DataSource가 재사용 가능한 풀을 유지.
- **JNDI(Java Naming & Directory Interface):** 리소스 조회용 키-값 레지스트리.
- **설정:** Tomcat context.xml에서 DataSource 정의 → DAO에서 InitialContext로 조회.

### 6. Tomcat에서 DataSource 설정
- **파일:** CATALINA_HOME/conf/context.xml의 `<Resource>` 요소.
- **주요 속성:**
  - `name="jdbcoracle"` (JNDI 조회 키)
  - `driverClassName` (Oracle 드라이버 클래스)
  - `url` (JDBC 연결 URL)
  - `username`, `password`, `maxActive`, `maxWait`

### 7. DAO에서 DataSource 사용
- **조회:** Context ctx = new InitialContext(); DataSource ds = (DataSource)ctx.lookup("java:comp/env/jdbcoracle");
- **연결:** con = ds.getConnection() (자격증명 불필요; 풀이 처리).
- **장점:** 자동 연결 재활용; 성능 향상.

### 8. CRUD 작업
- **생성(INSERT):** PreparedStatement + pstmt.setString/setInt → executeUpdate().
- **읽기(SELECT):** ResultSet 순환 + rs.next() → 필드 추출.
- **수정(UPDATE):** WHERE 절 포함 PreparedStatement → executeUpdate().
- **삭제(DELETE):** WHERE 절 포함 PreparedStatement → executeUpdate().

### 9. 폼 기반 회원 관리
- **memberForm.html:** JavaScript로 폼 제출 전 클라이언트 검증.
- **숨겨진 입력:** command 파라미터(addMember/delMember)로 서버 로직 라우팅.
- **서블릿 라우팅:** if(command.equals("addMember")) → DAO.addMember(); if(command.equals("delMember")) → DAO.delMember().

### 10. Eclipse에서 디버깅
- **중단점:** 줄 번호 더블클릭으로 MemberServlet.doHandle()에 중단점 설정.
- **디버그 모드:** 프로젝트 우측클릭 → Debug As → Debug on Server.
- **스텝 제어:** F5(Step Into), F6(Step Over), F8(Resume).
- **변수 뷰:** 런타임에 변수 값 확인(command, id, pwd 등).

### 11. 데이터베이스 설정
- **테이블 생성:** create table tmember(id, pwd, name, email, joinDate).
- **샘플 데이터:** SQL Developer로 테스트 레코드(hong, lee, kim) 삽입.
- **검증:** SELECT * FROM tmember로 데이터 삽입 확인.

</details>
