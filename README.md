<details>
<summary>ENG (English Version)</summary>

# MyBatis Framework Usage

### 1. MyBatis Purpose
Eliminates raw JDBC boilerplate (Connection, Statement, manual ResultSet mapping). **XML-based SQL** stored separately from Java code. Supports dynamic SQL, parameterized queries, result mapping with VO/HashMap.

### 2. Architecture Components
- **SqlMapConfig.xml:** Configuration file (DB connection, mapper locations).
- **Mapper XML:** SQL statements (e.g., `member.xml`).
- **SqlSession:** Runtime object executing queries.
- **SqlSessionFactory:** Creates SqlSession instances.
- **ResultMap:** Maps query columns to object properties.

### 3. SqlSessionFactory Setup
```
public static SqlSessionFactory getInstance() {
  if (sqlMapper == null) {
    String resource = "mybatis/SqlMapConfig.xml";
    Reader reader = Resources.getResourceAsReader(resource);
    sqlMapper = new SqlSessionFactoryBuilder().build(reader);
  }
  return sqlMapper;
}
```

### 4. Basic SELECT with String Return
**member.xml:**
```
<select id="selectName" resultType="String">
  <![CDATA[select name from tmember where id = #{id}]]>
</select>
```

**Java:**
```
SqlSession session = sqlMapper.openSession();
String name = session.selectOne("mapper.member.selectName", "hong");
```

### 5. SELECT with ResultMap (List)
```
<resultMap id="memResult" type="MemberVO">
  <result property="id" column="id"/>
  <result property="name" column="name"/>
  <result property="email" column="email"/>
  <result property="joinDate" column="joinDate"/>
</resultMap>

<select id="selectAllMemberList" resultMap="memResult">
  <![CDATA[select from tmember order by joinDate desc]]>
</select>
```

**Java:**
```
List<MemberVO> list = session.selectList("mapper.member.selectAllMemberList");
```

### 6. SELECT with Single Object
```
MemberVO member = session.selectOne("mapper.member.selectMemberById", "park");
```

### 7. INSERT with Object Parameter
**member.xml:**
```
<insert id="insertMember" parameterType="MemberVO">
  <![CDATA[
  insert into tmember(id, pwd, name, email) 
  values(#{id}, #{pwd}, #{name}, #{email})
  ]]>
</insert>
```

**Java:**
```
int result = session.insert("mapper.member.insertMember", memberVO);
session.commit();
```

### 8. INSERT with HashMap
```
<insert id="insertMember2" parameterType="java.util.HashMap">
  <![CDATA[
  insert into tmember(id, pwd, name, email) 
  values(#{id}, #{pwd}, #{name}, #{email})
  ]]>
</insert>
```

**Java:**
```
Map<String, String> memberMap = new HashMap<>();
memberMap.put("id", id);
memberMap.put("pwd", pwd);
int result = session.insert("mapper.member.insertMember2", memberMap);
```

### 9. UPDATE
```
<update id="updateMember" parameterType="MemberVO">
  <![CDATA[
  update tmember set pwd=#{pwd}, name=#{name}, email=#{email} where id=#{id}
  ]]>
</update>
```

**Java:**
```
int result = session.update("mapper.member.updateMember", memberVO);
session.commit();
```

### 10. DELETE
```
<delete id="deleteMember" parameterType="String">
  <![CDATA[delete from tmember where id = #{id}]]>
</delete>
```

**Java:**
```
int result = session.delete("mapper.member.deleteMember", id);
session.commit();
```

### 11. Dynamic SQL - if
```
<select id="searchMember" parameterType="MemberVO" resultMap="memResult">
  <![CDATA[select from tmember where]]>
  <if test="name != null and name != ''">
    and name = #{name}
  </if>
  <if test="email != null and email != ''">
    and email = #{email}
  </if>
</select>
```

### 12. Dynamic SQL - choose
```
<select id="selectByCondition">
  select from tmember where
  <choose>
    <when test="searchType == 'id'">id = #{value}</when>
    <when test="searchType == 'name'">name = #{value}</when>
    <otherwise>1=1</otherwise>
  </choose>
</select>
```

### 13. Dynamic SQL - forEach
**For SELECT IN clause:**
```
<select id="foreachSelect" resultMap="memResult" parameterType="java.util.List">
  <![CDATA[
  select from tmember where name in
  ]]>
  <foreach item="item" collection="list" open="(" separator="," close=")">
    #{item}
  </foreach>
</select>
```

**Java:**
```
List<String> nameList = new ArrayList<>();
nameList.add("hong");
nameList.add("lee");
List<MemberVO> list = session.selectList("mapper.member.foreachSelect", nameList);
```

### 14. Dynamic SQL - forEach INSERT
```
<insert id="foreachInsert" parameterType="java.util.List">
  INSERT ALL
  <foreach item="item" collection="list" open="" separator="" close="SELECT FROM DUAL">
    INTO tmember(id, pwd, name, email) VALUES(#{item.id}, #{item.pwd}, #{item.name}, #{item.email})
  </foreach>
</insert>
```

**Java:**
```
List<MemberVO> memList = new ArrayList<>();
memList.add(new MemberVO("m1", "1234", "", "m1test.com"));
memList.add(new MemberVO("m2", "1234", "", "m2test.com"));
int result = session.insert("mapper.member.foreachInsert", memList);
```

### 15. SQL Fragment Reuse
```
<sql id="selectBase">
  select id, pwd, name, email, joinDate from tmember
</sql>

<select id="selectAll">
  <include refid="selectBase"/>
  order by joinDate desc
</select>
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 마이바티스 프레임워크 사용

### 1. MyBatis 목적
순수 JDBC 보일러플레이트 제거(Connection, Statement, 수동 ResultSet 매핑). **XML 기반 SQL**을 Java 코드와 분리. 동적 SQL, 파라미터화 쿼리, VO/HashMap 결과 매핑 지원.

### 2. 아키텍처 컴포넌트
- **SqlMapConfig.xml:** 설정 파일(DB 연결, Mapper 위치).
- **Mapper XML:** SQL 문(예: `member.xml`).
- **SqlSession:** 쿼리 실행 런타임 객체.
- **SqlSessionFactory:** SqlSession 생성.
- **ResultMap:** 컬럼을 객체 속성으로 매핑.

### 3. SqlSessionFactory 설정
```
public static SqlSessionFactory getInstance() {
  if (sqlMapper == null) {
    String resource = "mybatis/SqlMapConfig.xml";
    Reader reader = Resources.getResourceAsReader(resource);
    sqlMapper = new SqlSessionFactoryBuilder().build(reader);
  }
  return sqlMapper;
}
```

### 4. 기본 SELECT (String 반환)
**member.xml:**
```
<select id="selectName" resultType="String">
  <![CDATA[select name from tmember where id = #{id}]]>
</select>
```

**Java:**
```
SqlSession session = sqlMapper.openSession();
String name = session.selectOne("mapper.member.selectName", "hong");
```

### 5. SELECT with ResultMap (List)
```
<resultMap id="memResult" type="MemberVO">
  <result property="id" column="id"/>
  <result property="name" column="name"/>
</resultMap>

<select id="selectAllMemberList" resultMap="memResult">
  <![CDATA[select from tmember order by joinDate desc]]>
</select>
```

**Java:**
```
List<MemberVO> list = session.selectList("mapper.member.selectAllMemberList");
```

### 6. SELECT (단일 객체)
```
MemberVO member = session.selectOne("mapper.member.selectMemberById", "park");
```

### 7. INSERT with Object Parameter
**member.xml:**
```
<insert id="insertMember" parameterType="MemberVO">
  insert into tmember(id, pwd, name, email) values(#{id}, #{pwd}, #{name}, #{email})
</insert>
```

**Java:**
```
int result = session.insert("mapper.member.insertMember", memberVO);
session.commit();
```

### 8. INSERT with HashMap
```
<insert id="insertMember2" parameterType="java.util.HashMap">
  insert into tmember(id, pwd, name, email) values(#{id}, #{pwd}, #{name}, #{email})
</insert>
```

**Java:**
```
Map<String, String> memberMap = new HashMap<>();
memberMap.put("id", id);
int result = session.insert("mapper.member.insertMember2", memberMap);
```

### 9. UPDATE
```
<update id="updateMember" parameterType="MemberVO">
  update tmember set pwd=#{pwd}, name=#{name}, email=#{email} where id=#{id}
</update>
```

**Java:**
```
int result = session.update("mapper.member.updateMember", memberVO);
session.commit();
```

### 10. DELETE
```
<delete id="deleteMember" parameterType="String">
  delete from tmember where id = #{id}
</delete>
```

**Java:**
```
int result = session.delete("mapper.member.deleteMember", id);
session.commit();
```

### 11. 동적 SQL - if
```
<select id="searchMember" parameterType="MemberVO">
  select from tmember where
  <if test="name != null and name != ''">
    and name = #{name}
  </if>
  <if test="email != null and email != ''">
    and email = #{email}
  </if>
</select>
```

### 12. 동적 SQL - choose
```
<select id="selectByCondition">
  select from tmember where
  <choose>
    <when test="searchType == 'id'">id = #{value}</when>
    <when test="searchType == 'name'">name = #{value}</when>
    <otherwise>1=1</otherwise>
  </choose>
</select>
```

### 13. 동적 SQL - forEach (SELECT IN)
```
<select id="foreachSelect" parameterType="java.util.List">
  select from tmember where name in
  <foreach item="item" collection="list" open="(" separator="," close=")">
    #{item}
  </foreach>
</select>
```

**Java:**
```
List<String> nameList = new ArrayList<>();
nameList.add("hong");
nameList.add("lee");
List<MemberVO> list = session.selectList("mapper.member.foreachSelect", nameList);
```

### 14. 동적 SQL - forEach INSERT
```
<insert id="foreachInsert" parameterType="java.util.List">
  INSERT ALL
  <foreach item="item" collection="list" open="" separator="" close="SELECT FROM DUAL">
    INTO tmember(id, pwd, name, email) VALUES(#{item.id}, #{item.pwd}, #{item.name}, #{item.email})
  </foreach>
</insert>
```

**Java:**
```
List<MemberVO> memList = new ArrayList<>();
memList.add(new MemberVO("m1", "1234", "", "m1test.com"));
int result = session.insert("mapper.member.foreachInsert", memList);
```

### 15. SQL Fragment 재사용
```
<sql id="selectBase">
  select id, pwd, name, email, joinDate from tmember
</sql>

<select id="selectAll">
  <include refid="selectBase"/>
  order by joinDate desc
</select>
```

</details>
