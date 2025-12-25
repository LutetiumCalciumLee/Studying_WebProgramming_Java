<details>
<summary>ENG (English Version)</summary>

# Spring and MyBatis Integration

### 1. Dependencies
**mybatis:** `mybatis-3.0.5.jar`  
**mybatis-spring:** `mybatis-spring-1.0.1.jar`  
**Spring:** `spring-context`, `spring-beans`, `spring-core`  
**Database:** `ojdbc6.jar` (Oracle)

### 2. Configuration Files
- **web.xml:** `ContextLoaderListener` loads `action-mybatis.xml`, `action-service.xml`
- **action-servlet.xml:** MVC routing (Controller, ViewResolver, HandlerMapping)
- **action-mybatis.xml:** DataSource, SqlSessionFactory, SqlSession, DAO beans
- **action-service.xml:** Service → DAO injection
- **jdbc.properties:** DB credentials

### 3. action-mybatis.xml - DataSource Setup
```
<bean id="dataSource" class="org.apache.ibatis.datasource.pooled.PooledDataSource">
  <property name="driver" value="${jdbc.driverClassName}"/>
  <property name="url" value="${jdbc.url}"/>
  <property name="username" value="${jdbc.username}"/>
  <property name="password" value="${jdbc.password}"/>
</bean>
```

### 4. action-mybatis.xml - SqlSessionFactory
```
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource"/>
  <property name="configLocation" value="classpath:mybatis/model/modelConfig.xml"/>
  <property name="mapperLocations" value="classpath:mybatis/mappers/*.xml"/>
</bean>
```

### 5. action-mybatis.xml - SqlSessionTemplate
```
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
  <constructor-arg index="0" ref="sqlSessionFactory"/>
</bean>
```

### 6. action-mybatis.xml - DAO Bean
```
<bean id="memberDAO" class="com.spring.member.dao.MemberDAOImpl">
  <property name="sqlSession" ref="sqlSession"/>
</bean>
```

### 7. action-service.xml
```
<bean id="memberService" class="com.spring.member.service.MemberServiceImpl">
  <property name="memberDAO" ref="memberDAO"/>
</bean>
```

### 8. MemberServiceImpl
```
public class MemberServiceImpl implements MemberService {
  private MemberDAO memberDAO;
  
  public void setMemberDAO(MemberDAO memberDAO) {
    this.memberDAO = memberDAO;
  }
  
  public List<MemberVO> listMembers() throws DataAccessException {
    return memberDAO.selectAllMemberList();
  }
}
```

### 9. MemberDAOImpl - selectList
```
public List<MemberVO> selectAllMemberList() throws DataAccessException {
  List<MemberVO> membersList = sqlSession.selectList("mapper.member.selectAllMemberList");
  return membersList;
}
```

### 10. MemberDAOImpl - insert
```
public int insertMember(MemberVO memberVO) throws DataAccessException {
  int result = sqlSession.insert("mapper.member.insertMember", memberVO);
  return result;
}
```

### 11. MemberDAOImpl - delete
```
public int deleteMember(String id) throws DataAccessException {
  int result = sqlSession.delete("mapper.member.deleteMember", id);
  return result;
}
```

### 12. member.xml - SELECT
```
<resultMap id="memResult" type="memberVO">
  <result property="id" column="id"/>
  <result property="pwd" column="pwd"/>
  <result property="name" column="name"/>
  <result property="email" column="email"/>
  <result property="joinDate" column="joinDate"/>
</resultMap>

<select id="selectAllMemberList" resultMap="memResult">
  <![CDATA[select from tmember order by joinDate desc]]>
</select>

<insert id="insertMember" parameterType="memberVO">
  <![CDATA[insert into tmember(id, pwd, name, email) values(#{id}, #{pwd}, #{name}, #{email})]]>
</insert>
```

### 13. modelConfig.xml - TypeAlias
```
<configuration>
  <typeAliases>
    <typeAlias type="com.spring.member.vo.MemberVO" alias="memberVO"/>
  </typeAliases>
</configuration>
```

### 14. MemberControllerImpl
```
public class MemberControllerImpl extends MultiActionController {
  private MemberService memberService;
  
  public void setMemberService(MemberService memberService) {
    this.memberService = memberService;
  }
  
  public ModelAndView listMembers(HttpServletRequest request, HttpServletResponse response) {
    List<MemberVO> membersList = memberService.listMembers();
    ModelAndView mav = new ModelAndView("listMembers");
    mav.addObject("membersList", membersList);
    return mav;
  }
}
```

### 15. Integration Flow
```
MemberController → MemberService → MemberDAO → SqlSession → member.xml SQL
→ MyBatis mapping → List<MemberVO> → ModelAndView → listMembers.jsp
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링과 마이바티스 연동

### 1. 의존성
**mybatis:** `mybatis-3.0.5.jar`  
**mybatis-spring:** `mybatis-spring-1.0.1.jar`  
**Spring:** `spring-context`, `spring-beans`, `spring-core`  
**DB:** `ojdbc6.jar` (Oracle)

### 2. 설정 파일
- **web.xml:** `ContextLoaderListener`가 `action-mybatis.xml`, `action-service.xml` 로드
- **action-servlet.xml:** MVC 라우팅 (Controller, ViewResolver, HandlerMapping)
- **action-mybatis.xml:** DataSource, SqlSessionFactory, SqlSession, DAO 빈
- **action-service.xml:** Service → DAO 주입
- **jdbc.properties:** DB 자격증명

### 3. action-mybatis.xml - DataSource 설정
```
<bean id="dataSource" class="org.apache.ibatis.datasource.pooled.PooledDataSource">
  <property name="driver" value="${jdbc.driverClassName}"/>
  <property name="url" value="${jdbc.url}"/>
</bean>
```

### 4. action-mybatis.xml - SqlSessionFactory
```
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource"/>
  <property name="configLocation" value="classpath:mybatis/model/modelConfig.xml"/>
  <property name="mapperLocations" value="classpath:mybatis/mappers/*.xml"/>
</bean>
```

### 5. action-mybatis.xml - SqlSessionTemplate
```
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
  <constructor-arg index="0" ref="sqlSessionFactory"/>
</bean>
```

### 6. action-mybatis.xml - DAO 빈
```
<bean id="memberDAO" class="com.spring.member.dao.MemberDAOImpl">
  <property name="sqlSession" ref="sqlSession"/>
</bean>
```

### 7. action-service.xml
```
<bean id="memberService" class="com.spring.member.service.MemberServiceImpl">
  <property name="memberDAO" ref="memberDAO"/>
</bean>
```

### 8. MemberServiceImpl
```
public class MemberServiceImpl implements MemberService {
  private MemberDAO memberDAO;
  
  public void setMemberDAO(MemberDAO memberDAO) {
    this.memberDAO = memberDAO;
  }
  
  public List<MemberVO> listMembers() {
    return memberDAO.selectAllMemberList();
  }
}
```

### 9. MemberDAOImpl - selectList
```
public List<MemberVO> selectAllMemberList() {
  List<MemberVO> membersList = sqlSession.selectList("mapper.member.selectAllMemberList");
  return membersList;
}
```

### 10. MemberDAOImpl - insert
```
public int insertMember(MemberVO memberVO) {
  int result = sqlSession.insert("mapper.member.insertMember", memberVO);
  return result;
}
```

### 11. MemberDAOImpl - delete
```
public int deleteMember(String id) {
  int result = sqlSession.delete("mapper.member.deleteMember", id);
  return result;
}
```

### 12. member.xml - SELECT
```
<resultMap id="memResult" type="memberVO">
  <result property="id" column="id"/>
  <result property="pwd" column="pwd"/>
  <result property="name" column="name"/>
  <result property="email" column="email"/>
  <result property="joinDate" column="joinDate"/>
</resultMap>

<select id="selectAllMemberList" resultMap="memResult">
  select from tmember order by joinDate desc
</select>

<insert id="insertMember" parameterType="memberVO">
  insert into tmember(id, pwd, name, email) values(#{id}, #{pwd}, #{name}, #{email})
</insert>
```

### 13. modelConfig.xml - TypeAlias
```
<configuration>
  <typeAliases>
    <typeAlias type="com.spring.member.vo.MemberVO" alias="memberVO"/>
  </typeAliases>
</configuration>
```

### 14. MemberControllerImpl
```
public class MemberControllerImpl extends MultiActionController {
  private MemberService memberService;
  
  public void setMemberService(MemberService memberService) {
    this.memberService = memberService;
  }
  
  public ModelAndView listMembers(HttpServletRequest request, HttpServletResponse response) {
    List<MemberVO> membersList = memberService.listMembers();
    ModelAndView mav = new ModelAndView("listMembers");
    mav.addObject("membersList", membersList);
    return mav;
  }
}
```

### 15. 전반적인 흐름
```
MemberController → MemberService → MemberDAO → SqlSession → member.xml SQL
→ MyBatis 매핑 → List<MemberVO> → ModelAndView → listMembers.jsp
```

</details>
