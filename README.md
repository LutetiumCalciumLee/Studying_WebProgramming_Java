<details>
<summary>ENG (English Version)</summary>

# Spring Annotation Features

### 1. Annotation-Based Configuration
Replaces XML bean definitions with Java annotations. Enables **component scanning** and **dependency injection** via `@Component`, `@Controller`, `@Service`, `@Repository`, `@Autowired`.

### 2. action-servlet.xml Setup
```
<bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"/>
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter"/>
<context:component-scan base-package="com.spring"/>
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <property name="prefix" value="WEB-INF/views/"/>
  <property name="suffix" value=".jsp"/>
</bean>
```

### 3. @Controller with @RequestMapping
```
@Controller
public class MainController {
  @RequestMapping(value="test/main1.do", method=RequestMethod.GET)
  public ModelAndView main1(HttpServletRequest request, HttpServletResponse response) {
    ModelAndView mav = new ModelAndView();
    mav.addObject("msg", "main1");
    mav.setViewName("main");
    return mav;
  }
  
  @RequestMapping(value="test/main2.do")
  public ModelAndView main2() {
    ModelAndView mav = new ModelAndView();
    mav.addObject("msg", "main2");
    mav.setViewName("main");
    return mav;
  }
}
```

### 4. @RequestParam with Single Values
```
@RequestMapping(value="testlogin.do", method={RequestMethod.GET, RequestMethod.POST})
public ModelAndView login(@RequestParam("userID") String userID, 
                          @RequestParam("userName") String userName,
                          HttpServletRequest request, HttpServletResponse response) {
  ModelAndView mav = new ModelAndView("result");
  mav.addObject("userID", userID);
  mav.addObject("userName", userName);
  return mav;
}
```

### 5. @RequestParam with required
```
@RequestMapping(value="testlogin2.do", method={RequestMethod.GET, RequestMethod.POST})
public ModelAndView login2(@RequestParam("userID") String userID,
                           @RequestParam(value="userName", required=true) String userName,
                           @RequestParam(value="email", required=false) String email,
                           HttpServletRequest request, HttpServletResponse response) {
  ModelAndView mav = new ModelAndView("result");
  mav.addObject("userID", userID);
  mav.addObject("userName", userName);
  return mav;
}
```

### 6. @RequestParam with Map
```
@RequestMapping(value="testlogin3.do", method={RequestMethod.GET, RequestMethod.POST})
public ModelAndView login3(@RequestParam Map<String, String> info,
                           HttpServletRequest request, HttpServletResponse response) {
  String userID = info.get("userID");
  String userName = info.get("userName");
  ModelAndView mav = new ModelAndView("result");
  mav.addObject("info", info);
  mav.setViewName("result");
  return mav;
}
```

### 7. @ModelAttribute with VO
```
@RequestMapping(value="testlogin4.do", method={RequestMethod.GET, RequestMethod.POST})
public ModelAndView login4(@ModelAttribute("info") LoginVO loginVO,
                           HttpServletRequest request, HttpServletResponse response) {
  String userID = loginVO.getUserID();
  String userName = loginVO.getUserName();
  ModelAndView mav = new ModelAndView("result");
  return mav;
}
```

### 8. Model Interface
```
@RequestMapping(value="testlogin5.do", method={RequestMethod.GET, RequestMethod.POST})
public String login5(Model model, HttpServletRequest request, HttpServletResponse response) {
  request.setCharacterEncoding("utf-8");
  model.addAttribute("userID", "hong");
  model.addAttribute("userName", "Hong");
  return "result";
}
```

### 9. @Service with @Autowired
```
@Service("memberService")
@Transactional(propagation=Propagation.REQUIRED)
public class MemberServiceImpl implements MemberService {
  @Autowired
  private MemberDAO memberDAO;
  
  @Override
  public List<MemberVO> listMembers() throws DataAccessException {
    List<MemberVO> membersList = memberDAO.selectAllMemberList();
    return membersList;
  }
}
```

### 10. @Repository with SqlSession
```
@Repository("memberDAO")
public class MemberDAOImpl implements MemberDAO {
  @Autowired
  private SqlSession sqlSession;
  
  @Override
  public List<MemberVO> selectAllMemberList() throws DataAccessException {
    List<MemberVO> membersList = sqlSession.selectList("mapper.member.selectAllMemberList");
    return membersList;
  }
}
```

### 11. @Controller with @Autowired
```
@Controller("memberController")
public class MemberControllerImpl implements MemberController {
  @Autowired
  private MemberService memberService;
  @Autowired
  private MemberVO memberVO;
  
  @RequestMapping(value="member/listMembers.do", method=RequestMethod.GET)
  public ModelAndView listMembers(HttpServletRequest request, HttpServletResponse response) {
    List<MemberVO> membersList = memberService.listMembers();
    ModelAndView mav = new ModelAndView("listMembers");
    mav.addObject("membersList", membersList);
    return mav;
  }
}
```

### 12. @Component
```
@Component("memberVO")
public class MemberVO {
  private String id;
  private String pwd;
  private String name;
  private String email;
  private Date joinDate;
  
  public MemberVO() {}
  
  public MemberVO(String id, String pwd, String name, String email) {
    this.id = id;
    this.pwd = pwd;
    this.name = name;
    this.email = email;
  }
}
```

### 13. action-mybatis.xml (with DAO commented out)
```
<bean id="dataSource" class="org.apache.ibatis.datasource.pooled.PooledDataSource">
  <property name="driver" value="${jdbc.driverClassName}"/>
  <property name="url" value="${jdbc.url}"/>
</bean>

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource"/>
  <property name="configLocation" value="classpath:mybatis/models/modelConfig.xml"/>
  <property name="mapperLocations" value="classpath:mybatis/mappers/*.xml"/>
</bean>

<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
  <constructor-arg index="0" ref="sqlSessionFactory"/>
</bean>
<!-- DAO Bean defined via @Repository annotation -->
```

### 14. Annotation Flow
```
@RequestMapping URL → @Controller method → @Autowired Service 
→ @Autowired DAO → @Repository (SqlSession) → MyBatis SQL → Result
```

### 15. Comparison: XML vs Annotation
| Aspect | XML | Annotation |
|--------|-----|-----------|
| Bean Definition | `<bean>` tags | `@Component`, `@Service`, etc. |
| Dependency Injection | `<property>` | `@Autowired` |
| URL Mapping | `<bean>` + properties | `@RequestMapping` |
| Configuration File Size | Large | Minimal |
| Type Safety | Runtime | Compile-time checks |

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링 애너테이션 기능

### 1. 애너테이션 기반 설정
XML 빈 정의를 Java 애너테이션으로 대체. `@Component`, `@Controller`, `@Service`, `@Repository`, `@Autowired`로 컴포넌트 스캔과 의존성 주입 가능.

### 2. action-servlet.xml 설정
```
<bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"/>
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter"/>
<context:component-scan base-package="com.spring"/>
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <property name="prefix" value="WEB-INF/views/"/>
  <property name="suffix" value=".jsp"/>
</bean>
```

### 3. @Controller with @RequestMapping
```
@Controller
public class MainController {
  @RequestMapping(value="test/main1.do", method=RequestMethod.GET)
  public ModelAndView main1() {
    ModelAndView mav = new ModelAndView();
    mav.addObject("msg", "main1");
    mav.setViewName("main");
    return mav;
  }
}
```

### 4. @RequestParam (단일 값)
```
@RequestMapping(value="testlogin.do", method={RequestMethod.GET, RequestMethod.POST})
public ModelAndView login(@RequestParam("userID") String userID,
                          @RequestParam("userName") String userName) {
  ModelAndView mav = new ModelAndView("result");
  mav.addObject("userID", userID);
  mav.addObject("userName", userName);
  return mav;
}
```

### 5. @RequestParam with required
```
@RequestMapping(value="testlogin2.do", method={RequestMethod.GET, RequestMethod.POST})
public ModelAndView login2(@RequestParam("userID") String userID,
                           @RequestParam(value="userName", required=true) String userName,
                           @RequestParam(value="email", required=false) String email) {
  ModelAndView mav = new ModelAndView("result");
  return mav;
}
```

### 6. @RequestParam with Map
```
@RequestMapping(value="testlogin3.do", method={RequestMethod.GET, RequestMethod.POST})
public ModelAndView login3(@RequestParam Map<String, String> info) {
  String userID = info.get("userID");
  ModelAndView mav = new ModelAndView("result");
  mav.addObject("info", info);
  return mav;
}
```

### 7. @ModelAttribute with VO
```
@RequestMapping(value="testlogin4.do", method={RequestMethod.GET, RequestMethod.POST})
public ModelAndView login4(@ModelAttribute("info") LoginVO loginVO) {
  String userID = loginVO.getUserID();
  ModelAndView mav = new ModelAndView("result");
  return mav;
}
```

### 8. Model 인터페이스
```
@RequestMapping(value="testlogin5.do", method={RequestMethod.GET, RequestMethod.POST})
public String login5(Model model) {
  model.addAttribute("userID", "hong");
  model.addAttribute("userName", "Hong");
  return "result";
}
```

### 9. @Service with @Autowired
```
@Service("memberService")
@Transactional(propagation=Propagation.REQUIRED)
public class MemberServiceImpl implements MemberService {
  @Autowired
  private MemberDAO memberDAO;
  
  public List<MemberVO> listMembers() {
    return memberDAO.selectAllMemberList();
  }
}
```

### 10. @Repository with SqlSession
```
@Repository("memberDAO")
public class MemberDAOImpl implements MemberDAO {
  @Autowired
  private SqlSession sqlSession;
  
  public List<MemberVO> selectAllMemberList() {
    return sqlSession.selectList("mapper.member.selectAllMemberList");
  }
}
```

### 11. @Controller with @Autowired
```
@Controller("memberController")
public class MemberControllerImpl {
  @Autowired
  private MemberService memberService;
  
  @RequestMapping(value="member/listMembers.do", method=RequestMethod.GET)
  public ModelAndView listMembers() {
    List<MemberVO> membersList = memberService.listMembers();
    ModelAndView mav = new ModelAndView("listMembers");
    mav.addObject("membersList", membersList);
    return mav;
  }
}
```

### 12. @Component
```
@Component("memberVO")
public class MemberVO {
  private String id;
  private String pwd;
  private String name;
  private String email;
  
  public MemberVO() {}
  
  public MemberVO(String id, String pwd, String name, String email) {
    this.id = id;
    this.pwd = pwd;
    this.name = name;
    this.email = email;
  }
}
```

### 13. action-mybatis.xml (DAO는 애너테이션으로)
```
<bean id="dataSource" class="org.apache.ibatis.datasource.pooled.PooledDataSource">
  <property name="driver" value="${jdbc.driverClassName}"/>
  <property name="url" value="${jdbc.url}"/>
</bean>

<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
  <constructor-arg index="0" ref="sqlSessionFactory"/>
</bean>
<!-- DAO Bean은 @Repository 애너테이션으로 정의 -->
```

### 14. 애너테이션 흐름
```
@RequestMapping URL → @Controller 메서드 → @Autowired Service 
→ @Autowired DAO → @Repository (SqlSession) → MyBatis SQL
```

### 15. 비교: XML vs 애너테이션
| 항목 | XML | 애너테이션 |
|------|-----|----------|
| Bean 정의 | `<bean>` 태그 | `@Component` 등 |
| 의존성 주입 | `<property>` | `@Autowired` |
| URL 매핑 | `<bean>` + properties | `@RequestMapping` |
| 설정 파일 크기 | 크다 | 최소 |
| 타입 안전성 | 런타임 | 컴파일 타임 |

</details>
