<details>
<summary>ENG (English Version)</summary>

# Spring MVC Functionality

### 1. Spring MVC Architecture
**DispatcherServlet** receives all requests → **HandlerMapping** routes to **Controller** → **ModelAndView** returned → **ViewResolver** renders JSP/HTML. Centralizes request handling unlike Model2's multiple servlets.

### 2. SimpleUrlController Pattern
Single-purpose controller for one URL. `SimpleUrlHandlerMapping` maps `test/index.do` → `SimpleUrlController.handleRequest()`. Returns `new ModelAndView("index.jsp")`.

### 3. web.xml Configuration
```
<servlet>
  <servlet-name>action</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
  <servlet-name>action</servlet-name>
  <url-pattern>*.do</url-pattern>
</servlet-mapping>
```
All `*.do` requests → `action-servlet.xml` DispatcherServlet.

### 4. action-servlet.xml Setup
```
<bean id="simpleUrlController" class="com.spring.ex01.SimpleUrlController"/>
<bean id="urlMapping" class="SimpleUrlHandlerMapping">
  <property name="mappings">
    <props>
      <prop key="test/index.do">simpleUrlController</prop>
    </props>
  </property>
</bean>
```

### 5. MultiActionController Pattern
One controller handles multiple URLs via **PropertiesMethodNameResolver**. `testlogin.do` → `login()`, `testmemberInfo.do` → `memberInfo()`. Reduces controller count.

### 6. View Resolution
**InternalResourceViewResolver:**
```
<bean id="viewResolver" class="InternalResourceViewResolver">
  <property name="prefix" value="/test/"/>
  <property name="suffix" value=".jsp"/>
</bean>
```
`ModelAndView mav = new ModelAndView("result")` → `/test/result.jsp`.

### 7. Method Resolution Flow
```
testlogin.do → PropertiesMethodNameResolver → "login" → UserController.login()
testmemberInfo.do → "memberInfo" → UserController.memberInfo()
```

### 8. Dynamic View Name Extraction
`getViewName(request)` extracts JSP name from URL: `/pro21/testlogin.do` → `login`. Enables convention-based view resolution without hardcoded strings.

### 9. Request Flow Diagram
```
Browser → DispatcherServlet → HandlerMapping → Controller → ModelAndView 
→ ViewResolver → JSP → Response
```

### 10. Key Components Comparison
| Component | Role | Configuration |
|-----------|------|---------------|
| DispatcherServlet | Front Controller | web.xml |
| HandlerMapping | URL→Controller | action-servlet.xml |
| Controller | Business Logic | @Controller/ModelAndView |
| ViewResolver | View Resolution | prefix/suffix |

### 11. Project Structure
```
pro21/
├── src/com.spring.ex02/UserController.java
├── WebContent/test/
│   ├── loginForm.jsp
│   ├── result.jsp
│   └── memberInfo.jsp
└── WEB-INF/
    ├── web.xml
    └── action-servlet.xml
```

### 12. Form Processing Example
```
public ModelAndView login(HttpServletRequest request) {
  String userID = request.getParameter("userID");
  mav.addObject("userID", userID);
  mav.setViewName("result");
  return mav;
}
```

### 13. Evolution Path
**Spring 2.x:** AbstractController hierarchy (SimpleUrlController, MultiActionController).  
**Spring 3.0+:** `@Controller` + `@RequestMapping`.  
**Spring 4.0+:** `@RestController` for JSON APIs.

### 14. Benefits Over Model2
- **Centralized:** Single DispatcherServlet vs multiple servlets.
- **Declarative:** XML mapping vs pathInfo parsing.
- **Flexible:** Multiple view technologies (JSP, Tiles, Freemarker).
- **Integrated:** Seamlessly works with Spring IoC/DI/AOP.

### 15. Testing URLs
- `http://localhost:8090/pro21/test/index.do` → index.jsp
- `http://localhost:8090/pro21/testlogin.do` → login() → result.jsp

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링 MVC 기능

### 1. 스프링 MVC 아키텍처
**DispatcherServlet**이 모든 요청 수신 → **HandlerMapping**이 **Controller**로 라우팅 → **ModelAndView** 반환 → **ViewResolver**가 JSP/HTML 렌더링. Model2의 다중 서블릿과 달리 요청 처리 중앙화.

### 2. SimpleUrlController 패턴
단일 URL용 전용 컨트롤러. `SimpleUrlHandlerMapping`이 `test/index.do` → `SimpleUrlController.handleRequest()` 매핑. `new ModelAndView("index.jsp")` 반환.

### 3. web.xml 설정
```
<servlet-name>action</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<url-pattern>*.do</url-pattern>
```
모든 `*.do` 요청 → `action-servlet.xml` DispatcherServlet.

### 4. action-servlet.xml 설정
```
<bean id="simpleUrlController" class="com.spring.ex01.SimpleUrlController"/>
<bean id="urlMapping" class="SimpleUrlHandlerMapping">
  <property name="mappings">
    <props><prop key="test/index.do">simpleUrlController</prop></props>
  </property>
</bean>
```

### 5. MultiActionController 패턴
**PropertiesMethodNameResolver**로 단일 컨트롤러가 다중 URL 처리. `testlogin.do` → `login()`, `testmemberInfo.do` → `memberInfo()`. 컨트롤러 수 감소.

### 6. 뷰 해석
**InternalResourceViewResolver:**
```
<property name="prefix" value="/test/"/>
<property name="suffix" value=".jsp"/>
```
`ModelAndView("result")` → `/test/result.jsp`.

### 7. 메서드 해석 흐름
```
testlogin.do → PropertiesMethodNameResolver → "login" → UserController.login()
testmemberInfo.do → "memberInfo" → UserController.memberInfo()
```

### 8. 동적 뷰명 추출
`getViewName(request)`가 URL에서 JSP명 추출: `/pro21/testlogin.do` → `login`. 하드코딩 없는 컨벤션 기반 뷰 해석.

### 9. 요청 흐름 다이어그램
```
브라우저 → DispatcherServlet → HandlerMapping → Controller → ModelAndView 
→ ViewResolver → JSP → 응답
```

### 10. 핵심 컴포넌트 비교
| 컴포넌트 | 역할 | 설정 |
|----------|------|------|
| DispatcherServlet | Front Controller | web.xml |
| HandlerMapping | URL→Controller | action-servlet.xml |
| Controller | 비즈니스 로직 | @Controller/ModelAndView |
| ViewResolver | 뷰 해석 | prefix/suffix |

### 11. 프로젝트 구조
```
pro21/
├── src/com.spring.ex02/UserController.java
├── WebContent/test/
│   ├── loginForm.jsp, result.jsp, memberInfo.jsp
└── WEB-INF/
    ├── web.xml, action-servlet.xml
```

### 12. 폼 처리 예제
```
public ModelAndView login(HttpServletRequest request) {
  String userID = request.getParameter("userID");
  mav.addObject("userID", userID);
  mav.setViewName("result");
  return mav;
}
```

### 13. 진화 경로
**Spring 2.x:** AbstractController 계층(SimpleUrlController, MultiActionController).  
**Spring 3.0+:** `@Controller` + `@RequestMapping`.  
**Spring 4.0+:** `@RestController` JSON API.

### 14. Model2 대비 장점
- **중앙화:** 단일 DispatcherServlet vs 다중 서블릿.
- **선언적:** XML 매핑 vs pathInfo 파싱.
- **유연성:** JSP, Tiles, Freemarker 등 다중 뷰 기술.
- **통합:** Spring IoC/DI/AOP과 완벽 통합.

### 15. 테스트 URL
- `http://localhost:8090/pro21/test/index.do` → index.jsp
- `http://localhost:8090/pro21/testlogin.do` → login() → result.jsp

</details>
