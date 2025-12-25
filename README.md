<details>
<summary>ENG (English Version)</summary>

# Spring JDBC Functionality

### 1. Spring JDBC Architecture
Replaces manual JDBC boilerplate (Connection, PreparedStatement, ResultSet) with **JdbcTemplate**. Supports DI for DataSource, Service, Controller layers. Configuration split: `action-dataSource.xml` + `action-service.xml`.

### 2. Multi-File Configuration
**web.xml:** `ContextLoaderListener` loads `action-dataSource.xml`, `action-service.xml`.  
**jdbc.properties:** `driverClassName=oracle.jdbc.driver.OracleDriver`, `url=jdbc:oracle:thin:@localhost:1521:XE`.  
**SimpleDriverDataSource:** Injects DB credentials into JdbcTemplate.

### 3. Layered Dependency Injection
```
MemberController → MemberService → MemberDAO → JdbcTemplate → DataSource
```
All via `<property name="memberService" ref="memberService"/>` setter injection.

### 4. JdbcTemplate Core Methods
| Method | Purpose | Example |
|--------|---------|---------|
| `query(sql, RowMapper)` | SELECT → List | `selectAllMembers()` |
| `update(sql)` | INSERT/UPDATE/DELETE | `addMember()` |
| `queryForObject(sql, Class)` | Single row | `queryForInt(sql)` |
| `queryForList(sql)` | List of primitives | `queryForList(sql)` |

### 5. RowMapper Implementation
```
jdbcTemplate.query(sql, new RowMapper<MemberVO>() {
  public MemberVO mapRow(ResultSet rs, int rowNum) {
    MemberVO vo = new MemberVO();
    vo.setId(rs.getString("id"));
    vo.setName(rs.getString("name"));
    return vo;
  }
});
```

### 6. Complete DAO Example
```
public List<MemberVO> selectAllMembers() {
  String sql = "select id, pwd, name, email, joinDate from tmember order by joinDate desc";
  return jdbcTemplate.query(sql, new MemberRowMapper());
}

public int addMember(MemberVO vo) {
  String sql = "insert into tmember(id, pwd, name, email) values(?, ?, ?, ?)";
  return jdbcTemplate.update(sql, vo.getId(), vo.getPwd(), vo.getName(), vo.getEmail());
}
```

### 7. XML DataSource Configuration
```
<bean id="dataSource" class="SimpleDriverDataSource">
  <property name="driverClass" value="${jdbc.driverClassName}"/>
  <property name="url" value="${jdbc.url}"/>
  <property name="username" value="${jdbc.username}"/>
  <property name="password" value="${jdbc.password}"/>
</bean>
```

### 8. Handler Mapping Setup
```
<bean id="urlMapping" class="SimpleUrlHandlerMapping">
  <property name="mappings">
    <props>
      <prop key="member/listMembers.do">memberController</prop>
      <prop key="member/addMember.do">memberController</prop>
    </props>
  </property>
</bean>
```

### 9. Project Structure
```
pro22/
├── WEB-INF/config/
│   ├── jdbc.properties
│   ├── action-dataSource.xml
│   └── action-service.xml
├── WEB-INF/views/
│   └── listMembers.jsp
└── src/com.spring.member/
    ├── controller/MemberControllerImpl.java
    ├── service/MemberServiceImpl.java
    └── dao/MemberDAOImpl.java
```

### 10. MVC Integration Flow
```
member/listMembers.do → MemberController.listMembers() 
→ MemberService.listMembers() → MemberDAO.selectAllMembers() 
→ JdbcTemplate.query() → listMembers.jsp
```

### 11. Benefits Over Raw JDBC
- **No boilerplate:** No try-catch-finally for Connection/Statement.
- **Exception translation:** SQLException → DataAccessException.
- **Thread-safe:** JdbcTemplate reusable across threads.
- **Parameterized queries:** SQL injection prevention.

### 12. View Example (listMembers.jsp)
```
<c:forEach var="member" items="${membersList}">
  <tr>
    <td>${member.id}</td>
    <td>${member.name}</td>
    <td>${member.email}</td>
    <td>${member.joinDate}</td>
  </tr>
</c:forEach>
```

### 13. Testing Results
`http://localhost:8090/pro22/member/listMembers.do` displays ki, park2, park, kim, lee, hong from tmember table with join dates.

### 14. Evolution Path
**Spring 2.x:** JdbcTemplate + RowMapper.  
**Spring 3.0+:** `SimpleJdbcTemplate` (NamedParameterJdbcTemplate).  
**Spring 4.0+:** `@JdbcTemplate` + `@Query`.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링 JDBC 기능

### 1. 스프링 JDBC 아키텍처
수동 JDBC 보일러플레이트(Connection, PreparedStatement, ResultSet)를 **JdbcTemplate**으로 대체. DataSource, Service, Controller 레이어에 DI 지원. 설정 분리: `action-dataSource.xml` + `action-service.xml`.

### 2. 다중 파일 설정
**web.xml:** `ContextLoaderListener`가 `action-dataSource.xml`, `action-service.xml` 로드.  
**jdbc.properties:** `driverClassName=oracle.jdbc.driver.OracleDriver`, `url=jdbc:oracle:thin:@localhost:1521:XE`.  
**SimpleDriverDataSource:** DB 인증정보를 JdbcTemplate에 주입.

### 3. 계층적 의존성 주입
```
MemberController → MemberService → MemberDAO → JdbcTemplate → DataSource
```
`<property name="memberService" ref="memberService"/>` Setter 주입으로 모두 연결.

### 4. JdbcTemplate 핵심 메서드
| 메서드 | 용도 | 예제 |
|--------|------|------|
| `query(sql, RowMapper)` | SELECT → List | `selectAllMembers()` |
| `update(sql)` | INSERT/UPDATE/DELETE | `addMember()` |
| `queryForObject(sql, Class)` | 단일 행 | `queryForInt(sql)` |
| `queryForList(sql)` | 기본형 리스트 | `queryForList(sql)` |

### 5. RowMapper 구현
```
jdbcTemplate.query(sql, new RowMapper<MemberVO>() {
  public MemberVO mapRow(ResultSet rs, int rowNum) {
    MemberVO vo = new MemberVO();
    vo.setId(rs.getString("id"));
    vo.setName(rs.getString("name"));
    return vo;
  }
});
```

### 6. 완전한 DAO 예제
```
public List<MemberVO> selectAllMembers() {
  String sql = "select id, pwd, name, email, joinDate from tmember order by joinDate desc";
  return jdbcTemplate.query(sql, new MemberRowMapper());
}

public int addMember(MemberVO vo) {
  String sql = "insert into tmember(id, pwd, name, email) values(?, ?, ?, ?)";
  return jdbcTemplate.update(sql, vo.getId(), vo.getPwd(), vo.getName(), vo.getEmail());
}
```

### 7. XML DataSource 설정
```
<bean id="dataSource" class="SimpleDriverDataSource">
  <property name="driverClass" value="${jdbc.driverClassName}"/>
  <property name="url" value="${jdbc.url}"/>
</bean>
```

### 8. Handler 매핑 설정
```
<bean id="urlMapping" class="SimpleUrlHandlerMapping">
  <property name="mappings">
    <props>
      <prop key="member/listMembers.do">memberController</prop>
    </props>
  </property>
</bean>
```

### 9. 프로젝트 구조
```
pro22/
├── WEB-INF/config/
│   ├── jdbc.properties
│   ├── action-dataSource.xml
│   └── action-service.xml
├── WEB-INF/views/
│   └── listMembers.jsp
└── src/com.spring.member/
    ├── controller/, service/, dao/
```

### 10. MVC 통합 흐름
```
member/listMembers.do → MemberController → MemberService 
→ MemberDAO → JdbcTemplate → listMembers.jsp
```

### 11. 순수 JDBC 대비 장점
- **보일러플레이트 제거:** try-catch-finally 불필요.
- **예외 변환:** SQLException → DataAccessException.
- **스레드 안전:** JdbcTemplate 재사용 가능.
- **파라미터화 쿼리:** SQL 인젝션 방지.

### 12. 뷰 예제 (listMembers.jsp)
```
<c:forEach var="member" items="${membersList}">
  <td>${member.id}</td>
  <td>${member.name}</td>
  <td>${member.email}</td>
</c:forEach>
```

### 13. 테스트 결과
`http://localhost:8090/pro22/member/listMembers.do` → ki, park2, park, kim, lee, hong 표시.

### 14. 진화 경로
**Spring 2.x:** JdbcTemplate + RowMapper.  
**Spring 3.0+:** NamedParameterJdbcTemplate.  
**Spring 4.0+:** `@JdbcTemplate` + `@Query`.

</details>
