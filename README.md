<details>
<summary>ENG (English Version)</summary>

# Maven and Spring Tool Suite (STS) Usage

## 1. Maven Installation

**Maven Purpose:** Build automation, dependency management (via `pom.xml`), project lifecycle management.

**Installation Steps:**
1. Download `apache-maven-3.5.4-bin.zip` from `maven.apache.org`
2. Extract to `C:\spring\apache-maven-3.5.4`
3. Set environment variables:
   - `MAVEN_HOME`: `C:\spring\apache-maven-3.5.4`
   - Add `%MAVEN_HOME%\bin` to `PATH`
4. Verify: Open CMD and type `mvn -v`

## 2. Maven Project Structure
```
pro27/
├── pom.xml                                    # Project configuration & dependencies
├── src/
│   ├── main/
│   │   ├── java/                             # Java source code
│   │   │   └── com/myspring/pro27/
│   │   │       ├── member/
│   │   │       │   ├── controller/           # Controller classes
│   │   │       │   ├── service/              # Service classes
│   │   │       │   ├── dao/                  # DAO classes
│   │   │       │   └── vo/                   # Value Objects (MemberVO)
│   │   │       └── common/
│   │   │           └── aop/                  # AOP Aspect classes
│   │   ├── resources/                        # Configuration files
│   │   │   ├── log4j.xml                    # Logging configuration
│   │   │   └── mybatis/
│   │   │       ├── mappers/
│   │   │       │   └── member.xml           # MyBatis SQL mappings
│   │   │       └── model/
│   │   │           └── modelConfig.xml      # MyBatis type aliases
│   │   └── webapp/
│   │       ├── WEB-INF/
│   │       │   ├── config/
│   │       │   │   └── jdbc.properties      # Database connection info
│   │       │   ├── spring/
│   │       │   │   ├── appServlet/
│   │       │   │   │   └── servlet-context.xml  # Spring MVC config
│   │       │   │   ├── action-mybatis.xml       # DataSource, SqlSession config
│   │       │   │   └── root-context.xml         # Root context (AOP config)
│   │       │   └── views/
│   │       │       ├── common/
│   │       │       │   ├── layout.jsp      # Tiles layout template
│   │       │       │   ├── header.jsp
│   │       │       │   ├── side.jsp
│   │       │       │   └── footer.jsp
│   │       │       ├── member/
│   │       │       │   ├── listMembers.jsp
│   │       │       │   ├── memberForm.jsp
│   │       │       │   └── modMember.jsp
│   │       │       └── home.jsp
│   │       ├── resources/
│   │       │   ├── css/
│   │       │   ├── js/
│   │       │   └── images/
│   │       └── web.xml                       # Web application deployment descriptor
│   ├── test/
│   │   ├── java/                            # JUnit test classes
│   │   └── resources/
│   │       └── log4j.xml                    # Test logging configuration
│   └── target/                              # Compiled output
│       ├── classes/                         # Compiled Java classes
│       ├── pro27-1.0.0-BUILD-SNAPSHOT.war   # Deployable WAR archive
│       └── ...

```

## 3. pom.xml - Project Object Model
```
<?xml version="1.0" encoding="UTF-8"?>
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.spring</groupId>
  <artifactId>mytest2</artifactId>
  <packaging>war</packaging>
  <version>1.0.0-BUILD-SNAPSHOT</version>
  <name>springtest2</name>
  
  <properties>
    <java-version>1.6</java-version>
    <org.springframework-version>4.0.0.RELEASE</org.springframework-version>
    <org.aspectj-version>1.6.10</org.aspectj-version>
    <org.slf4j-version>1.6.6</org.slf4j-version>
  </properties>
  
  <dependencies>
    <!-- Spring Framework -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${org.springframework-version}</version>
    </dependency>
  </dependencies>
</project>
```

## 4. Spring Tool Suite (STS) Installation

**Steps:**
1. Open **Eclipse** → Help → **Eclipse Marketplace**
2. Search "Spring Tool Suite" (STS 3.9.6.RELEASE)
3. Click **Install**
4. Accept licenses → **Restart Eclipse**

## 5. Creating Spring MVC Project

**Process:**
1. File → New → **Spring Legacy Project**
2. Select **Spring MVC Project** template
3. Input package name: `com.myspring.pro27`
4. Click **Finish**

**Generated Structure:**
- `HomeController.java` (sample)
- `pom.xml` with Spring dependencies
- `web.xml`, `servlet-context.xml`, `root-context.xml`
- `home.jsp` (sample view)

## 6. Maven Dependencies in pom.xml

### Add MyBatis and Oracle Driver
```
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.1.0</version>
</dependency>

<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>1.1.0</version>
</dependency>

<dependency>
  <groupId>commons-dbcp</groupId>
  <artifactId>commons-dbcp</artifactId>
  <version>1.2.2</version>
</dependency>

<dependency>
  <groupId>jdbc.oracle</groupId>
  <artifactId>OracleDriver</artifactId>
  <version>12.1.0.2.0</version>
  <scope>system</scope>
  <systemPath>${basedir}/src/main/webapp/WEB-INF/lib/ojdbc6.jar</systemPath>
</dependency>
```

### Add Apache Tiles for Layout
```
<dependency>
  <groupId>org.apache.tiles</groupId>
  <artifactId>tiles-core</artifactId>
  <version>2.2.2</version>
</dependency>

<dependency>
  <groupId>org.apache.tiles</groupId>
  <artifactId>tiles-jsp</artifactId>
  <version>2.2.2</version>
</dependency>

<dependency>
  <groupId>org.apache.tiles</groupId>
  <artifactId>tiles-servlet</artifactId>
  <version>2.2.2</version>
</dependency>
```

### Add AspectJ for AOP
```
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjrt</artifactId>
  <version>${org.aspectj-version}</version>
</dependency>

<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
  <version>${org.aspectj-version}</version>
</dependency>
```

## 7. Log4j Configuration

**log4j.xml (in src/test/resources):**
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration PUBLIC "-//APACHE//DTD LOG4J 1.2//EN" ...>
<log4j:configuration>
  <!-- Console Appender -->
  <appender name="console" class="org.apache.log4j.ConsoleAppender">
    <param name="Target" value="System.out"/>
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="%-5p %c %m%n"/>
    </layout>
  </appender>
  
  <!-- Daily File Appender -->
  <appender name="dailyFileAppender" class="org.apache.log4j.DailyRollingFileAppender">
    <param name="File" value="C://logs/output.log"/>
    <param name="Append" value="true"/>
    <layout class="org.apache.log4j.PatternLayout">
      <param name="DatePattern" value="'.yyyy-MM-dd'"/>
      <param name="ConversionPattern" value="%d{HHmmss} %-5p %c %m%n"/>
    </layout>
  </appender>
  
  <!-- Application Loggers -->
  <logger name="com.myspring.pro27" level="info"/>
  
  <!-- Root Logger -->
  <root>
    <priority value="debug"/>
    <appender-ref ref="console"/>
    <appender-ref ref="dailyFileAppender"/>
  </root>
</log4j:configuration>
```

## 8. Using Logger in Controller

```
private static final Logger logger = LoggerFactory.getLogger(MemberControllerImpl.class);

@RequestMapping(value="member/listMembers.do", method=RequestMethod.GET)
public ModelAndView listMembers(HttpServletRequest request, HttpServletResponse response) {
  String viewName = getViewName(request);
  logger.info("info: " + viewName);
  logger.debug("debug: " + viewName);
  
  List<MemberVO> membersList = memberService.listMembers();
  ModelAndView mav = new ModelAndView(viewName);
  mav.addObject("membersList", membersList);
  return mav;
}
```

## 9. AOP (Aspect-Oriented Programming) Setup

### Add AspectJ to root-context.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
  
  <aop:aspectj-autoproxy/>
</beans>
```

### Create LoggingAdvice (Aspect)
```
@Component
@Aspect
public class LoggingAdvice {
  private static final Logger logger = LoggerFactory.getLogger(LoggingAdvice.class);
  
  @Before("execution(com.myspring.pro27.member.service..*(..))" + 
          " or execution(com.myspring.pro27.member.dao..*(..))")
  public void startLog(JoinPoint jp) {
    logger.info("=== Method START ===");
    logger.info("1 - Arguments: " + Arrays.toString(jp.getArgs()));
    logger.info("2 - Type: " + jp.getKind());
    logger.info("3 - Method: " + jp.getSignature().getName());
  }
  
  @After("execution(com.myspring.pro27.member.service..*(..))" + 
         " or execution(com.myspring.pro27.member.dao..*(..))")
  public void after(JoinPoint jp) {
    logger.info("=== Method END ===");
  }
  
  @Around("execution(com.myspring.pro27.member.service..*(..))" + 
          " or execution(com.myspring.pro27.member.dao..*(..))")
  public Object timeLog(ProceedingJoinPoint pjp) throws Throwable {
    long startTime = System.currentTimeMillis();
    Object result = pjp.proceed();
    long endTime = System.currentTimeMillis();
    logger.info("Method: " + pjp.getSignature().getName() + 
                " Time: " + (endTime - startTime) + "ms");
    return result;
  }
}
```

## 10. Apache Tiles for Layout Management

### tiles.xml (src/main/resources/tiles/)
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC ...>
<tiles-definitions>
  <definition name="baseLayout" template="/WEB-INF/views/common/layout.jsp">
    <put-attribute name="title" value="Main"/>
    <put-attribute name="header" value="/WEB-INF/views/common/header.jsp"/>
    <put-attribute name="side" value="/WEB-INF/views/common/side.jsp"/>
    <put-attribute name="body" value=""/>
    <put-attribute name="footer" value="/WEB-INF/views/common/footer.jsp"/>
  </definition>
  
  <definition name="main" extends="baseLayout">
    <put-attribute name="title" value="Home"/>
    <put-attribute name="body" value="/WEB-INF/views/main.jsp"/>
  </definition>
  
  <definition name="memberlistMembers" extends="baseLayout">
    <put-attribute name="title" value="Member List"/>
    <put-attribute name="body" value="/WEB-INF/views/member/listMembers.jsp"/>
  </definition>
</tiles-definitions>
```

### servlet-context.xml (Enable Tiles)
```
<bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles2.TilesConfigurer">
  <property name="definitions">
    <list>
      <value>classpath:tiles/tiles.xml</value>
    </list>
  </property>
</bean>

<bean id="viewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver">
  <property name="viewClass" value="org.springframework.web.servlet.view.tiles2.TilesView"/>
</bean>
```

### layout.jsp
```
<%@ taglib uri="http://tiles.apache.org/tags-tiles" prefix="tiles" %>
<!DOCTYPE html>
<html>
<head>
  <title><tiles:insertAttribute name="title"/></title>
</head>
<body>
  <div id="container">
    <div id="header"><tiles:insertAttribute name="header"/></div>
    <div id="sidebar"><tiles:insertAttribute name="side"/></div>
    <div id="content"><tiles:insertAttribute name="body"/></div>
    <div id="footer"><tiles:insertAttribute name="footer"/></div>
  </div>
</body>
</html>
```

## 11. Login Implementation

### member.xml - SQL Query
```
<select id="loginById" resultType="memberVO" parameterType="memberVO">
  <![CDATA[
  select from tmember where id = #{id} and pwd = #{pwd}
  ]]>
</select>
```

### MemberControllerImpl - Login Handler
```
@RequestMapping(value="member/login.do", method=RequestMethod.POST)
public ModelAndView login(@ModelAttribute MemberVO member,
                          RedirectAttributes rAttr,
                          HttpServletRequest request) {
  MemberVO memberVO = memberService.login(member);
  
  if (memberVO != null) {
    HttpSession session = request.getSession();
    session.setAttribute("member", memberVO);
    session.setAttribute("isLogon", true);
    return new ModelAndView("redirect:member/listMembers.do");
  } else {
    rAttr.addAttribute("result", "loginFailed");
    return new ModelAndView("redirect:member/loginForm.do");
  }
}

@RequestMapping(value="member/logout.do", method=RequestMethod.GET)
public ModelAndView logout(HttpServletRequest request) {
  HttpSession session = request.getSession();
  session.removeAttribute("member");
  session.removeAttribute("isLogon");
  return new ModelAndView("redirect:member/listMembers.do");
}
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 메이븐과 스프링 STS 사용법

## 1. 메이븐 설치

**메이븐 목적:** 빌드 자동화, 의존성 관리 (`pom.xml`), 프로젝트 생명주기 관리.

**설치 단계:**
1. `maven.apache.org`에서 `apache-maven-3.5.4-bin.zip` 다운로드
2. `C:\spring\apache-maven-3.5.4`로 추출
3. 환경 변수 설정:
   - `MAVEN_HOME`: `C:\spring\apache-maven-3.5.4`
   - `PATH`에 `%MAVEN_HOME%\bin` 추가
4. 검증: CMD 열고 `mvn -v` 입력

## 2. 메이븐 프로젝트 구조
```
pro27/
├── pom.xml                                    # 프로젝트 설정 및 의존성
├── src/
│   ├── main/
│   │   ├── java/                             # Java 소스 코드
│   │   │   └── com/myspring/pro27/
│   │   │       ├── member/
│   │   │       │   ├── controller/           # Controller 클래스
│   │   │       │   ├── service/              # Service 클래스
│   │   │       │   ├── dao/                  # DAO 클래스
│   │   │       │   └── vo/                   # VO (MemberVO 등)
│   │   │       └── common/
│   │   │           └── aop/                  # AOP Aspect 클래스
│   │   ├── resources/                        # 설정 파일
│   │   │   ├── log4j.xml                    # 로깅 설정
│   │   │   └── mybatis/
│   │   │       ├── mappers/
│   │   │       │   └── member.xml           # MyBatis SQL 매핑
│   │   │       └── model/
│   │   │           └── modelConfig.xml      # MyBatis 타입 별칭
│   │   └── webapp/
│   │       ├── WEB-INF/
│   │       │   ├── config/
│   │       │   │   └── jdbc.properties      # 데이터베이스 연결 정보
│   │       │   ├── spring/
│   │       │   │   ├── appServlet/
│   │       │   │   │   └── servlet-context.xml  # Spring MVC 설정
│   │       │   │   ├── action-mybatis.xml       # DataSource, SqlSession 설정
│   │       │   │   └── root-context.xml         # 루트 컨텍스트 (AOP 설정)
│   │       │   └── views/
│   │       │       ├── common/
│   │       │       │   ├── layout.jsp      # Tiles 레이아웃 템플릿
│   │       │       │   ├── header.jsp
│   │       │       │   ├── side.jsp
│   │       │       │   └── footer.jsp
│   │       │       ├── member/
│   │       │       │   ├── listMembers.jsp
│   │       │       │   ├── memberForm.jsp
│   │       │       │   └── modMember.jsp
│   │       │       └── home.jsp
│   │       ├── resources/
│   │       │   ├── css/
│   │       │   ├── js/
│   │       │   └── images/
│   │       └── web.xml                       # 웹 애플리케이션 배포 설명서
│   ├── test/
│   │   ├── java/                            # JUnit 테스트 클래스
│   │   └── resources/
│   │       └── log4j.xml                    # 테스트 로깅 설정
│   └── target/                              # 컴파일된 결과물
│       ├── classes/                         # 컴파일된 Java 클래스
│       ├── pro27-1.0.0-BUILD-SNAPSHOT.war   # 배포 가능한 WAR 아카이브
│       └── ...

```

## 3. pom.xml - 프로젝트 객체 모델
```
<project>
  <groupId>com.spring</groupId>
  <artifactId>mytest2</artifactId>
  <version>1.0.0</version>
  <packaging>war</packaging>
  
  <properties>
    <org.springframework-version>4.0.0.RELEASE</org.springframework-version>
  </properties>
  
  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${org.springframework-version}</version>
    </dependency>
  </dependencies>
</project>
```

## 4. Spring Tool Suite (STS) 설치

**단계:**
1. Eclipse → Help → Eclipse Marketplace
2. "Spring Tool Suite" 검색
3. Install → 라이선스 수용 → Restart

## 5. Spring MVC 프로젝트 생성

**프로세스:**
1. File → New → Spring Legacy Project
2. Spring MVC Project 선택
3. 패키지명 입력: `com.myspring.pro27`
4. Finish

## 6. pom.xml - 의존성 추가

### MyBatis 및 Oracle
```
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.1.0</version>
</dependency>

<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>1.1.0</version>
</dependency>

<dependency>
  <groupId>commons-dbcp</groupId>
  <artifactId>commons-dbcp</artifactId>
  <version>1.2.2</version>
</dependency>
```

### Apache Tiles (레이아웃)
```
<dependency>
  <groupId>org.apache.tiles</groupId>
  <artifactId>tiles-core</artifactId>
  <version>2.2.2</version>
</dependency>

<dependency>
  <groupId>org.apache.tiles</groupId>
  <artifactId>tiles-jsp</artifactId>
  <version>2.2.2</version>
</dependency>

<dependency>
  <groupId>org.apache.tiles</groupId>
  <artifactId>tiles-servlet</artifactId>
  <version>2.2.2</version>
</dependency>
```

### AspectJ (AOP)
```
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjrt</artifactId>
  <version>1.6.10</version>
</dependency>

<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
  <version>1.6.10</version>
</dependency>
```

## 7. Log4j 설정

**log4j.xml:**
```
<log4j:configuration>
  <appender name="console" class="org.apache.log4j.ConsoleAppender">
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="%-5p %c %m%n"/>
    </layout>
  </appender>
  
  <appender name="dailyFileAppender" class="org.apache.log4j.DailyRollingFileAppender">
    <param name="File" value="C://logs/output.log"/>
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="%d{HHmmss} %-5p %c %m%n"/>
    </layout>
  </appender>
  
  <logger name="com.myspring.pro27" level="info"/>
  
  <root>
    <priority value="debug"/>
    <appender-ref ref="console"/>
    <appender-ref ref="dailyFileAppender"/>
  </root>
</log4j:configuration>
```

## 8. Controller에서 Logger 사용

```
private static final Logger logger = LoggerFactory.getLogger(MemberControllerImpl.class);

@RequestMapping(value="member/listMembers.do", method=RequestMethod.GET)
public ModelAndView listMembers() {
  logger.info("INFO 레벨");
  logger.debug("DEBUG 레벨");
  
  List<MemberVO> membersList = memberService.listMembers();
  ModelAndView mav = new ModelAndView("memberlistMembers");
  mav.addObject("membersList", membersList);
  return mav;
}
```

## 9. AOP (관점 지향 프로그래밍) 설정

### root-context.xml
```
<beans xmlns:aop="http://www.springframework.org/schema/aop">
  <aop:aspectj-autoproxy/>
</beans>
```

### LoggingAdvice (Aspect)
```
@Component
@Aspect
public class LoggingAdvice {
  @Before("execution(com.myspring.pro27.member.service..*(..)) or execution(com.myspring.pro27.member.dao..*(..))")
  public void startLog(JoinPoint jp) {
    logger.info("=== 메서드 시작 ===");
    logger.info("인자: " + Arrays.toString(jp.getArgs()));
    logger.info("메서드명: " + jp.getSignature().getName());
  }
  
  @Around("execution(com.myspring.pro27.member.service..*(..)) or execution(com.myspring.pro27.member.dao..*(..))")
  public Object timeLog(ProceedingJoinPoint pjp) throws Throwable {
    long startTime = System.currentTimeMillis();
    Object result = pjp.proceed();
    long endTime = System.currentTimeMillis();
    logger.info("실행시간: " + (endTime - startTime) + "ms");
    return result;
  }
}
```

## 10. Apache Tiles 레이아웃

### tiles.xml
```
<definition name="baseLayout" template="/WEB-INF/views/common/layout.jsp">
  <put-attribute name="header" value="/WEB-INF/views/common/header.jsp"/>
  <put-attribute name="body" value=""/>
  <put-attribute name="footer" value="/WEB-INF/views/common/footer.jsp"/>
</definition>

<definition name="memberlistMembers" extends="baseLayout">
  <put-attribute name="body" value="/WEB-INF/views/member/listMembers.jsp"/>
</definition>
```

### layout.jsp
```
<%@ taglib uri="http://tiles.apache.org/tags-tiles" prefix="tiles" %>
<div id="header"><tiles:insertAttribute name="header"/></div>
<div id="body"><tiles:insertAttribute name="body"/></div>
<div id="footer"><tiles:insertAttribute name="footer"/></div>
```

## 11. 로그인 구현

### member.xml
```
<select id="loginById" resultType="memberVO" parameterType="memberVO">
  select from tmember where id = #{id} and pwd = #{pwd}
</select>
```

### MemberControllerImpl
```
@RequestMapping(value="member/login.do", method=RequestMethod.POST)
public ModelAndView login(@ModelAttribute MemberVO member, HttpServletRequest request) {
  MemberVO result = memberService.login(member);
  
  if (result != null) {
    HttpSession session = request.getSession();
    session.setAttribute("member", result);
    session.setAttribute("isLogon", true);
    return new ModelAndView("redirect:member/listMembers.do");
  } else {
    return new ModelAndView("redirect:member/loginForm.do?result=loginFailed");
  }
}

@RequestMapping(value="member/logout.do", method=RequestMethod.GET)
public ModelAndView logout(HttpServletRequest request) {
  HttpSession session = request.getSession();
  session.removeAttribute("member");
  session.removeAttribute("isLogon");
  return new ModelAndView("redirect:member/listMembers.do");
}
```

</details>
