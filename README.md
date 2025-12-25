<details>
<summary>ENG (English Version)</summary>

# Spring Framework - Multiple Features and Functionalities

## 1. File Upload and Download

### 1.1 File Upload Configuration

**Purpose:** Handle file upload functionality using Apache Commons FileUpload library.

**Step 1: Add Dependencies to pom.xml**
```xml
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.2.1</version>
</dependency>

<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>1.4</version>
</dependency>
```

**Step 2: Configure CommonsMultipartResolver in servlet-context.xml**
```xml
<bean id="multipartResolver" 
      class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  <!-- Maximum file upload size: 50MB -->
  <property name="maxUploadSize" value="52428800"/>
  
  <!-- Maximum in-memory size: 1MB -->
  <property name="maxInMemorySize" value="1000000"/>
  
  <!-- Default encoding: UTF-8 -->
  <property name="defaultEncoding" value="utf-8"/>
</bean>
```

### 1.2 File Download Controller

**FileDownloadController.java**
```java
@Controller
public class FileDownloadController {
  private static String CURR_IMAGE_REPO_PATH = "C:/repo/";
  
  @RequestMapping("download")
  protected void download(@RequestParam String imageFileName,
                          HttpServletResponse response) throws Exception {
    OutputStream out = response.getOutputStream();
    String downFile = CURR_IMAGE_REPO_PATH + imageFileName;
    File file = new File(downFile);
    
    response.setHeader("Cache-Control", "no-cache");
    response.addHeader("Content-disposition", 
                       "attachment; fileName=" + imageFileName);
    
    FileInputStream in = new FileInputStream(file);
    byte[] buffer = new byte[1024 * 8];
    
    while (true) {
      int count = in.read(buffer);
      if (count == -1) break;
      out.write(buffer, 0, count);
    }
    in.close();
    out.close();
  }
}
```

### 1.3 File Upload Controller

**FileUploadController.java**
```java
@Controller
public class FileUploadController {
  private static final String CURR_IMAGE_REPO_PATH = "C:/repo/";
  
  @RequestMapping(value="form")
  public String form() {
    return "uploadForm";  // Displays uploadForm.jsp
  }
  
  @RequestMapping(value="upload", method=RequestMethod.POST)
  public ModelAndView upload(MultipartHttpServletRequest multipartRequest,
                             HttpServletResponse response) throws Exception {
    multipartRequest.setCharacterEncoding("utf-8");
    Map<String, Object> map = new HashMap<String, Object>();
    
    // Process form parameters
    Enumeration<String> enumeration = multipartRequest.getParameterNames();
    while (enumeration.hasMoreElements()) {
      String name = (String) enumeration.nextElement();
      String value = multipartRequest.getParameter(name);
      map.put(name, value);
    }
    
    // Process file uploads
    List<String> fileList = fileProcess(multipartRequest);
    map.put("fileList", fileList);
    
    ModelAndView mav = new ModelAndView();
    mav.addObject("map", map);
    mav.setViewName("result");
    return mav;
  }
  
  private List<String> fileProcess(MultipartHttpServletRequest multipartRequest)
                                    throws Exception {
    List<String> fileList = new ArrayList<String>();
    Iterator<String> fileNames = multipartRequest.getFileNames();
    
    while (fileNames.hasNext()) {
      String fileName = fileNames.next();
      MultipartFile mFile = multipartRequest.getFile(fileName);
      String originalFileName = mFile.getOriginalFilename();
      fileList.add(originalFileName);
      
      File file = new File(CURR_IMAGE_REPO_PATH + fileName);
      
      if (mFile.getSize() != 0) {
        if (!file.exists()) {
          if (file.getParentFile().mkdirs()) {
            file.createNewFile();
          }
        }
        mFile.transferTo(new File(CURR_IMAGE_REPO_PATH + originalFileName));
      }
    }
    return fileList;
  }
}
```

### 1.4 Upload Form JSP

**uploadForm.jsp**
```jsp
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>File Upload</title>
  <script src="http://code.jquery.com/jquery-latest.js"></script>
  <script>
    var cnt = 1;
    function fnAddFile() {
      var input = "<input type='file' name='file" + cnt + "'>";
      $("#dfile").append(input);
      cnt++;
    }
  </script>
</head>
<body>
  <h1>File Upload Form</h1>
  <form method="post" action="${contextPath}/upload" 
        enctype="multipart/form-data">
    <label>ID:</label>
    <input type="text" name="id"><br>
    
    <label>Name:</label>
    <input type="text" name="name"><br>
    
    <input type="button" value="Add File" onclick="fnAddFile()"><br>
    
    <div id="dfile"></div>
    <input type="submit" value="Upload">
  </form>
</body>
</html>
```

### 1.5 Result JSP

**result.jsp**
```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Upload Result</title>
</head>
<body>
  <h1>Upload Result</h1>
  
  <label>ID:</label>
  <input type="text" name="id" value="${map.id}" readonly><br>
  
  <label>Name:</label>
  <input type="text" name="name" value="${map.name}" readonly><br>
  
  <div class="result-images">
    <c:forEach var="imageFileName" items="${map.fileList}">
      <img src="${contextPath}/download?imageFileName=${imageFileName}">
      <br><br>
    </c:forEach>
  </div>
  
  <a href="${contextPath}/form">Back to Upload</a>
</body>
</html>
```

## 2. Thumbnail Generation with Thumbnailator

### 2.1 Add Thumbnailator Dependency

```xml
<dependency>
  <groupId>net.coobird</groupId>
  <artifactId>thumbnailator</artifactId>
  <version>0.4.8</version>
</dependency>
```

### 2.2 Thumbnail Generation in Download

**FileDownloadController.java (Enhanced)**
```java
@RequestMapping("download")
protected void download(@RequestParam String imageFileName,
                        HttpServletResponse response) throws Exception {
  OutputStream out = response.getOutputStream();
  String filePath = CURR_IMAGE_REPO_PATH + imageFileName;
  File image = new File(filePath);
  
  int lastIndex = imageFileName.lastIndexOf(".");
  String fileName = imageFileName.substring(0, lastIndex);
  
  File thumbnail = new File(CURR_IMAGE_REPO_PATH + "thumbnail/" + fileName + ".png");
  
  if (image.exists()) {
    if (!thumbnail.getParentFile().exists()) {
      thumbnail.getParentFile().mkdirs();
    }
    
    // Generate 50x50 thumbnail
    Thumbnails.of(image).size(50, 50)
              .outputFormat("png")
              .toFile(thumbnail);
    
    FileInputStream in = new FileInputStream(thumbnail);
    byte[] buffer = new byte[1024 * 8];
    
    int count;
    while ((count = in.read(buffer)) != -1) {
      out.write(buffer, 0, count);
    }
    in.close();
  }
  out.close();
}
```

### 2.3 Stream-Based Thumbnail Output

```java
if (image.exists()) {
  Thumbnails.of(image).size(50, 50)
            .outputFormat("png")
            .toOutputStream(out);  // Direct output to stream
} else {
  return;
}
```

## 3. Email (SMTP) Functionality

### 3.1 Add Mail Dependencies to pom.xml

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context-support</artifactId>
  <version>${org.springframework-version}</version>
</dependency>

<dependency>
  <groupId>javax.mail</groupId>
  <artifactId>javax.mail-api</artifactId>
  <version>1.5.4</version>
</dependency>

<dependency>
  <groupId>com.sun.mail</groupId>
  <artifactId>javax.mail</artifactId>
  <version>1.5.3</version>
</dependency>
```

### 3.2 Mail Configuration in mail-context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!-- Gmail SMTP Configuration -->
  <bean id="mailSender" 
        class="org.springframework.mail.javamail.JavaMailSenderImpl">
    
    <!-- SMTP server -->
    <property name="host" value="smtp.gmail.com"/>
    
    <!-- SMTP port (465 for SSL, 587 for TLS) -->
    <property name="port" value="465"/>
    
    <!-- Gmail username -->
    <property name="username" value="your-email@gmail.com"/>
    
    <!-- App-specific password -->
    <property name="password" value="your-app-password"/>
    
    <!-- JavaMail properties -->
    <property name="javaMailProperties">
      <props>
        <prop key="mail.transport.protocol">smtp</prop>
        <prop key="mail.smtp.auth">true</prop>
        <prop key="mail.smtp.starttls.enable">true</prop>
        <prop key="mail.smtp.socketFactory.class">javax.net.ssl.SSLSocketFactory</prop>
        <prop key="mail.debug">true</prop>
      </props>
    </property>
  </bean>
  
  <!-- Pre-configured email template -->
  <bean id="preConfiguredMessage" 
        class="org.springframework.mail.SimpleMailMessage">
    <property name="to" value="recipient@example.com"/>
    <property name="from" value="your-email@gmail.com"/>
    <property name="subject" value="Default Subject"/>
  </bean>
</beans>
```

### 3.3 Mail Controller

**MailController.java**
```java
@Controller
@EnableAsync
public class MailController {
  @Autowired
  private MailService mailService;
  
  @RequestMapping(value="sendMail.do", method=RequestMethod.GET)
  public void sendSimpleMail(HttpServletRequest request,
                             HttpServletResponse response) throws Exception {
    request.setCharacterEncoding("utf-8");
    response.setContentType("text/html; charset=utf-8");
    PrintWriter out = response.getWriter();
    
    // Send email asynchronously
    mailService.sendMail("recipient@naver.com", "Test Subject", "Test Body");
    
    // Send pre-configured email
    mailService.sendPreConfiguredMail("Message text");
    
    out.print("Email sent successfully!");
  }
}
```

### 3.4 Mail Service with Async Support

**MailService.java**
```java
@Service("mailService")
public class MailService {
  @Autowired
  private JavaMailSender mailSender;
  
  @Autowired
  private SimpleMailMessage preConfiguredMessage;
  
  @Async
  public void sendMail(String to, String subject, String body) {
    MimeMessage message = mailSender.createMimeMessage();
    
    try {
      MimeMessageHelper messageHelper = new MimeMessageHelper(message, true, "UTF-8");
      
      messageHelper.setFrom("your-email@naver.com", "Sender Name");
      messageHelper.setSubject(subject);
      messageHelper.setTo(to);
      messageHelper.setText(body);
      
      mailSender.send(message);
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
  
  @Async
  public void sendPreConfiguredMail(String message) {
    SimpleMailMessage mailMessage = new SimpleMailMessage(preConfiguredMessage);
    mailMessage.setText(message);
    mailSender.send(mailMessage);
  }
}
```

### 3.5 HTML Email

**MailController.java (HTML Email)**
```java
@RequestMapping(value="sendMail.do", method=RequestMethod.GET)
public void sendHtmlMail(HttpServletRequest request,
                         HttpServletResponse response) throws Exception {
  StringBuffer sb = new StringBuffer();
  sb.append("<html>");
  sb.append("<meta http-equiv='Content-Type' content='text/html; charset=euc-kr'>");
  sb.append("<h1>Book Recommendation</h1><br>");
  sb.append("<a href='http://www.kyobobook.co.kr/...'>Spring in Action</a><br>");
  sb.append("<img src='http://image.kyobobook.co.kr/images/book/...'/>");
  sb.append("</html>");
  
  String htmlContent = sb.toString();
  mailService.sendMail("recipient@naver.com", "Book Recommendation", htmlContent);
  
  response.setContentType("text/html; charset=utf-8");
  response.getWriter().print("HTML Email sent!");
}
```

**MailService.java (HTML Support)**
```java
@Async
public void sendMail(String to, String subject, String body) {
  MimeMessage message = mailSender.createMimeMessage();
  
  try {
    MimeMessageHelper messageHelper = new MimeMessageHelper(message, true, "UTF-8");
    messageHelper.setSubject(subject);
    messageHelper.setTo(to);
    messageHelper.setFrom("your-email@naver.com");
    
    // true parameter enables HTML content
    messageHelper.setText(body, true);
    
    mailSender.send(message);
  } catch (Exception e) {
    e.printStackTrace();
  }
}
```

## 4. Internationalization (i18n) and Localization (i10n)

### 4.1 Message Configuration in message-context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!-- Locale resolver using session -->
  <bean id="localeResolver" 
        class="org.springframework.web.servlet.i18n.SessionLocaleResolver">
  </bean>
  
  <!-- Message source for i18n -->
  <bean id="messageSource" 
        class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
    
    <!-- Message properties file locations -->
    <property name="basenames">
      <list>
        <value>classpath:locale/messages</value>
      </list>
    </property>
    
    <!-- Default encoding -->
    <property name="defaultEncoding" value="UTF-8"/>
    
    <!-- Cache refresh interval (60 seconds) -->
    <property name="cacheSeconds" value="60"/>
  </bean>
</beans>
```

### 4.2 Locale Interceptor Configuration in servlet-context.xml

```xml
<beans xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context">
  
  <mvc:interceptors>
    <mvc:interceptor>
      <mvc:mapping path="/testlocale.do"/>
      <mvc:mapping path="/**/*.do"/>
      
      <bean class="com.myspring.pro28.ex05.LocaleInterceptor"/>
    </mvc:interceptor>
  </mvc:interceptors>
</beans>
```

### 4.3 Locale Interceptor Implementation

**LocaleInterceptor.java**
```java
public class LocaleInterceptor extends HandlerInterceptorAdapter {
  
  @Override
  public boolean preHandle(HttpServletRequest request,
                          HttpServletResponse response,
                          Object handler) throws Exception {
    HttpSession session = request.getSession();
    String locale = request.getParameter("locale");
    
    if (locale != null && !locale.trim().equals("")) {
      session.setAttribute(
        "org.springframework.web.servlet.i18n.SessionLocaleResolver.LOCALE",
        new Locale(locale));
    }
    
    return true;
  }
  
  @Override
  public void postHandle(HttpServletRequest request,
                        HttpServletResponse response,
                        Object handler,
                        ModelAndView modelAndView) throws Exception {
    // Post-processing logic
  }
  
  @Override
  public void afterCompletion(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler,
                             Exception ex) throws Exception {
    // Cleanup logic
  }
}
```

### 4.4 Locale Controller

**LocaleController.java**
```java
@Controller
public class LocaleController {
  
  @RequestMapping(value="testlocale.do", method=RequestMethod.GET)
  public String locale(HttpServletRequest request,
                      HttpServletResponse response) throws Exception {
    return "locale";
  }
}
```

### 4.5 Message Properties Files

**messages.properties (Default - English)**
```properties
site.title=Member Information
site.name=Name
site.job=Job
btn.send=Send
btn.cancel=Cancel
btn.finish=Finish
name=Hong Gil-dong
job=Student
```

**messages_en.properties (English)**
```properties
site.title=Member Information
site.name=Name
site.job=Job
btn.send=Send
btn.cancel=Cancel
btn.finish=Finish
name=Hong Gil-dong
job=Student
```

**messages_ko.properties (Korean)**
```properties
site.title=회원 정보
site.name=이름
site.job=직업
btn.send=전송
btn.cancel=취소
btn.finish=완료
name=홍길동
job=학생
```

### 4.6 Locale JSP View

**locale.jsp**
```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://www.springframework.org/tags" prefix="spring" %>
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title><spring:message code="site.title" text="Member Info"/></title>
</head>
<body>
  <a href="${contextPath}/testlocale.do?locale=ko">Korean</a>
  <a href="${contextPath}/testlocale.do?locale=en">English</a>
  
  <h1><spring:message code="site.title" text="Member Info"/></h1>
  
  <p>
    <spring:message code="site.name" text="no name"/>:
    <spring:message code="name" text="no name"/>
  </p>
  
  <p>
    <spring:message code="site.job" text="no job"/>:
    <spring:message code="job" text="no job"/>
  </p>
  
  <input type="button" value="<spring:message code='btn.send'/>">
  <input type="button" value="<spring:message code='btn.cancel'/>">
  <input type="button" value="<spring:message code='btn.finish'/>">
</body>
</html>
```

## 5. Interceptor Implementation

### 5.1 HandlerInterceptor Overview

**Interceptor Processing Flow:**
```
Request
   ↓
[preHandle()] → true: proceed, false: abort
   ↓
Controller → Business Logic
   ↓
[postHandle()] → Can modify ModelAndView
   ↓
View Rendering
   ↓
[afterCompletion()] → Cleanup after view rendering
   ↓
Response
```

### 5.2 ViewName Interceptor

**ViewNameInterceptor.java**
```java
public class ViewNameInterceptor extends HandlerInterceptorAdapter {
  
  @Override
  public boolean preHandle(HttpServletRequest request,
                          HttpServletResponse response,
                          Object handler) throws Exception {
    try {
      String viewName = getViewName(request);
      request.setAttribute("viewName", viewName);
    } catch (Exception e) {
      e.printStackTrace();
    }
    return true;
  }
  
  @Override
  public void postHandle(HttpServletRequest request,
                        HttpServletResponse response,
                        Object handler,
                        ModelAndView modelAndView) throws Exception {
    // Post-processing
  }
  
  @Override
  public void afterCompletion(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler,
                             Exception ex) throws Exception {
    // Cleanup
  }
  
  private String getViewName(HttpServletRequest request) throws Exception {
    String contextPath = request.getContextPath();
    String uri = request.getAttribute("javax.servlet.include.request_uri") != null
                 ? (String) request.getAttribute("javax.servlet.include.request_uri")
                 : request.getRequestURI();
    
    int viewNameIndex = uri.indexOf(contextPath) + contextPath.length();
    String viewName = uri.substring(viewNameIndex);
    
    if (viewName.lastIndexOf(".") != -1) {
      viewName = viewName.substring(0, viewName.lastIndexOf("."));
    }
    return viewName;
  }
}
```

### 5.3 Controller Using Interceptor

**MemberControllerImpl.java**
```java
@Controller("memberController")
public class MemberControllerImpl implements MemberController {
  @Autowired
  private MemberService memberService;
  
  @RequestMapping(value="member/listMembers.do", method=RequestMethod.GET)
  public ModelAndView listMembers(HttpServletRequest request,
                                  HttpServletResponse response) throws Exception {
    String viewName = (String) request.getAttribute("viewName");
    
    List<MemberVO> membersList = memberService.listMembers();
    ModelAndView mav = new ModelAndView(viewName);
    mav.addObject("membersList", membersList);
    
    return mav;
  }
  
  @RequestMapping(value="member/loginForm.do", method=RequestMethod.GET)
  public ModelAndView form(@RequestParam(value="result", required=false) String result,
                          HttpServletRequest request,
                          HttpServletResponse response) throws Exception {
    String viewName = (String) request.getAttribute("viewName");
    
    ModelAndView mav = new ModelAndView();
    mav.addObject("result", result);
    mav.setViewName(viewName);
    
    return mav;
  }
}
```

### 5.4 Registering Interceptor in servlet-context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context">
  
  <mvc:interceptors>
    <mvc:interceptor>
      <mvc:mapping path="/*.do"/>
      <mvc:mapping path="/**/*.do"/>
      
      <bean class="com.myspring.pro27.member.interceptor.ViewNameInterceptor"/>
    </mvc:interceptor>
  </mvc:interceptors>
</beans>
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링 프레임워크 - 여러 가지 기능들

## 1. 파일 업로드 및 다운로드

### 1.1 파일 업로드 설정

**목적:** Apache Commons FileUpload 라이브러리를 사용하여 파일 업로드 기능 처리.

**Step 1: pom.xml에 의존성 추가**
```xml
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.2.1</version>
</dependency>

<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>1.4</version>
</dependency>
```

**Step 2: servlet-context.xml에 CommonsMultipartResolver 설정**
```xml
<bean id="multipartResolver" 
      class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  <!-- 최대 파일 업로드 크기: 50MB -->
  <property name="maxUploadSize" value="52428800"/>
  
  <!-- 최대 메모리 크기: 1MB -->
  <property name="maxInMemorySize" value="1000000"/>
  
  <!-- 기본 인코딩: UTF-8 -->
  <property name="defaultEncoding" value="utf-8"/>
</bean>
```

### 1.2 파일 다운로드 Controller

**FileDownloadController.java**
```java
@Controller
public class FileDownloadController {
  private static String CURR_IMAGE_REPO_PATH = "C:/repo/";
  
  @RequestMapping("download")
  protected void download(@RequestParam String imageFileName,
                          HttpServletResponse response) throws Exception {
    OutputStream out = response.getOutputStream();
    String downFile = CURR_IMAGE_REPO_PATH + imageFileName;
    File file = new File(downFile);
    
    response.setHeader("Cache-Control", "no-cache");
    response.addHeader("Content-disposition", 
                       "attachment; fileName=" + imageFileName);
    
    FileInputStream in = new FileInputStream(file);
    byte[] buffer = new byte[1024 * 8];
    
    while (true) {
      int count = in.read(buffer);
      if (count == -1) break;
      out.write(buffer, 0, count);
    }
    in.close();
    out.close();
  }
}
```

### 1.3 파일 업로드 Controller

**FileUploadController.java**
```java
@Controller
public class FileUploadController {
  private static final String CURR_IMAGE_REPO_PATH = "C:/repo/";
  
  @RequestMapping(value="form")
  public String form() {
    return "uploadForm";  // uploadForm.jsp 표시
  }
  
  @RequestMapping(value="upload", method=RequestMethod.POST)
  public ModelAndView upload(MultipartHttpServletRequest multipartRequest,
                             HttpServletResponse response) throws Exception {
    multipartRequest.setCharacterEncoding("utf-8");
    Map<String, Object> map = new HashMap<String, Object>();
    
    // 폼 매개변수 처리
    Enumeration<String> enumeration = multipartRequest.getParameterNames();
    while (enumeration.hasMoreElements()) {
      String name = (String) enumeration.nextElement();
      String value = multipartRequest.getParameter(name);
      map.put(name, value);
    }
    
    // 파일 업로드 처리
    List<String> fileList = fileProcess(multipartRequest);
    map.put("fileList", fileList);
    
    ModelAndView mav = new ModelAndView();
    mav.addObject("map", map);
    mav.setViewName("result");
    return mav;
  }
  
  private List<String> fileProcess(MultipartHttpServletRequest multipartRequest)
                                    throws Exception {
    List<String> fileList = new ArrayList<String>();
    Iterator<String> fileNames = multipartRequest.getFileNames();
    
    while (fileNames.hasNext()) {
      String fileName = fileNames.next();
      MultipartFile mFile = multipartRequest.getFile(fileName);
      String originalFileName = mFile.getOriginalFilename();
      fileList.add(originalFileName);
      
      File file = new File(CURR_IMAGE_REPO_PATH + fileName);
      
      if (mFile.getSize() != 0) {
        if (!file.exists()) {
          if (file.getParentFile().mkdirs()) {
            file.createNewFile();
          }
        }
        mFile.transferTo(new File(CURR_IMAGE_REPO_PATH + originalFileName));
      }
    }
    return fileList;
  }
}
```

### 1.4 업로드 폼 JSP

**uploadForm.jsp**
```jsp
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>파일 업로드</title>
  <script src="http://code.jquery.com/jquery-latest.js"></script>
  <script>
    var cnt = 1;
    function fnAddFile() {
      var input = "<input type='file' name='file" + cnt + "'>";
      $("#dfile").append(input);
      cnt++;
    }
  </script>
</head>
<body>
  <h1>파일 업로드 폼</h1>
  <form method="post" action="${contextPath}/upload" 
        enctype="multipart/form-data">
    <label>아이디:</label>
    <input type="text" name="id"><br>
    
    <label>이름:</label>
    <input type="text" name="name"><br>
    
    <input type="button" value="파일 추가" onclick="fnAddFile()"><br>
    
    <div id="dfile"></div>
    <input type="submit" value="업로드">
  </form>
</body>
</html>
```

### 1.5 결과 JSP

**result.jsp**
```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>업로드 결과</title>
</head>
<body>
  <h1>업로드 결과</h1>
  
  <label>아이디:</label>
  <input type="text" name="id" value="${map.id}" readonly><br>
  
  <label>이름:</label>
  <input type="text" name="name" value="${map.name}" readonly><br>
  
  <div class="result-images">
    <c:forEach var="imageFileName" items="${map.fileList}">
      <img src="${contextPath}/download?imageFileName=${imageFileName}">
      <br><br>
    </c:forEach>
  </div>
  
  <a href="${contextPath}/form">업로드로 돌아가기</a>
</body>
</html>
```

## 2. Thumbnailator를 사용한 썸네일 생성

### 2.1 Thumbnailator 의존성 추가

```xml
<dependency>
  <groupId>net.coobird</groupId>
  <artifactId>thumbnailator</artifactId>
  <version>0.4.8</version>
</dependency>
```

### 2.2 다운로드에서 썸네일 생성

**FileDownloadController.java (향상된 버전)**
```java
@RequestMapping("download")
protected void download(@RequestParam String imageFileName,
                        HttpServletResponse response) throws Exception {
  OutputStream out = response.getOutputStream();
  String filePath = CURR_IMAGE_REPO_PATH + imageFileName;
  File image = new File(filePath);
  
  int lastIndex = imageFileName.lastIndexOf(".");
  String fileName = imageFileName.substring(0, lastIndex);
  
  File thumbnail = new File(CURR_IMAGE_REPO_PATH + "thumbnail/" + fileName + ".png");
  
  if (image.exists()) {
    if (!thumbnail.getParentFile().exists()) {
      thumbnail.getParentFile().mkdirs();
    }
    
    // 50x50 썸네일 생성
    Thumbnails.of(image).size(50, 50)
              .outputFormat("png")
              .toFile(thumbnail);
    
    FileInputStream in = new FileInputStream(thumbnail);
    byte[] buffer = new byte[1024 * 8];
    
    int count;
    while ((count = in.read(buffer)) != -1) {
      out.write(buffer, 0, count);
    }
    in.close();
  }
  out.close();
}
```

### 2.3 스트림 기반 썸네일 출력

```java
if (image.exists()) {
  Thumbnails.of(image).size(50, 50)
            .outputFormat("png")
            .toOutputStream(out);  // 스트림으로 직접 출력
} else {
  return;
}
```

## 3. 이메일 (SMTP) 기능

### 3.1 pom.xml에 메일 의존성 추가

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context-support</artifactId>
  <version>${org.springframework-version}</version>
</dependency>

<dependency>
  <groupId>javax.mail</groupId>
  <artifactId>javax.mail-api</artifactId>
  <version>1.5.4</version>
</dependency>

<dependency>
  <groupId>com.sun.mail</groupId>
  <artifactId>javax.mail</artifactId>
  <version>1.5.3</version>
</dependency>
```

### 3.2 mail-context.xml에서 메일 설정

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!-- Gmail SMTP 설정 -->
  <bean id="mailSender" 
        class="org.springframework.mail.javamail.JavaMailSenderImpl">
    
    <!-- SMTP 서버 -->
    <property name="host" value="smtp.gmail.com"/>
    
    <!-- SMTP 포트 (SSL: 465, TLS: 587) -->
    <property name="port" value="465"/>
    
    <!-- Gmail 사용자명 -->
    <property name="username" value="your-email@gmail.com"/>
    
    <!-- 앱 고유 비밀번호 -->
    <property name="password" value="your-app-password"/>
    
    <!-- JavaMail 속성 -->
    <property name="javaMailProperties">
      <props>
        <prop key="mail.transport.protocol">smtp</prop>
        <prop key="mail.smtp.auth">true</prop>
        <prop key="mail.smtp.starttls.enable">true</prop>
        <prop key="mail.smtp.socketFactory.class">javax.net.ssl.SSLSocketFactory</prop>
        <prop key="mail.debug">true</prop>
      </props>
    </property>
  </bean>
  
  <!-- 미리 설정된 이메일 템플릿 -->
  <bean id="preConfiguredMessage" 
        class="org.springframework.mail.SimpleMailMessage">
    <property name="to" value="recipient@example.com"/>
    <property name="from" value="your-email@gmail.com"/>
    <property name="subject" value="기본 제목"/>
  </bean>
</beans>
```

### 3.3 메일 Controller

**MailController.java**
```java
@Controller
@EnableAsync
public class MailController {
  @Autowired
  private MailService mailService;
  
  @RequestMapping(value="sendMail.do", method=RequestMethod.GET)
  public void sendSimpleMail(HttpServletRequest request,
                             HttpServletResponse response) throws Exception {
    request.setCharacterEncoding("utf-8");
    response.setContentType("text/html; charset=utf-8");
    PrintWriter out = response.getWriter();
    
    // 비동기적으로 이메일 전송
    mailService.sendMail("recipient@naver.com", "테스트 제목", "테스트 본문");
    
    // 미리 설정된 이메일 전송
    mailService.sendPreConfiguredMail("메시지 텍스트");
    
    out.print("이메일이 성공적으로 전송되었습니다!");
  }
}
```

### 3.4 비동기 지원 메일 Service

**MailService.java**
```java
@Service("mailService")
public class MailService {
  @Autowired
  private JavaMailSender mailSender;
  
  @Autowired
  private SimpleMailMessage preConfiguredMessage;
  
  @Async
  public void sendMail(String to, String subject, String body) {
    MimeMessage message = mailSender.createMimeMessage();
    
    try {
      MimeMessageHelper messageHelper = new MimeMessageHelper(message, true, "UTF-8");
      
      messageHelper.setFrom("your-email@naver.com", "발신자 이름");
      messageHelper.setSubject(subject);
      messageHelper.setTo(to);
      messageHelper.setText(body);
      
      mailSender.send(message);
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
  
  @Async
  public void sendPreConfiguredMail(String message) {
    SimpleMailMessage mailMessage = new SimpleMailMessage(preConfiguredMessage);
    mailMessage.setText(message);
    mailSender.send(mailMessage);
  }
}
```

### 3.5 HTML 이메일

**MailController.java (HTML 이메일)**
```java
@RequestMapping(value="sendMail.do", method=RequestMethod.GET)
public void sendHtmlMail(HttpServletRequest request,
                         HttpServletResponse response) throws Exception {
  StringBuffer sb = new StringBuffer();
  sb.append("<html>");
  sb.append("<meta http-equiv='Content-Type' content='text/html; charset=euc-kr'>");
  sb.append("<h1>도서 추천</h1><br>");
  sb.append("<a href='http://www.kyobobook.co.kr/...'>Spring in Action</a><br>");
  sb.append("<img src='http://image.kyobobook.co.kr/images/book/...'/>");
  sb.append("</html>");
  
  String htmlContent = sb.toString();
  mailService.sendMail("recipient@naver.com", "도서 추천", htmlContent);
  
  response.setContentType("text/html; charset=utf-8");
  response.getWriter().print("HTML 이메일이 전송되었습니다!");
}
```

**MailService.java (HTML 지원)**
```java
@Async
public void sendMail(String to, String subject, String body) {
  MimeMessage message = mailSender.createMimeMessage();
  
  try {
    MimeMessageHelper messageHelper = new MimeMessageHelper(message, true, "UTF-8");
    messageHelper.setSubject(subject);
    messageHelper.setTo(to);
    messageHelper.setFrom("your-email@naver.com");
    
    // true 매개변수는 HTML 콘텐츠를 활성화
    messageHelper.setText(body, true);
    
    mailSender.send(message);
  } catch (Exception e) {
    e.printStackTrace();
  }
}
```

## 4. 국제화 (i18n) 및 지역화 (i10n)

### 4.1 message-context.xml에서 메시지 설정

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <!-- 세션을 사용하는 Locale Resolver -->
  <bean id="localeResolver" 
        class="org.springframework.web.servlet.i18n.SessionLocaleResolver">
  </bean>
  
  <!-- i18n용 메시지 소스 -->
  <bean id="messageSource" 
        class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
    
    <!-- 메시지 properties 파일 위치 -->
    <property name="basenames">
      <list>
        <value>classpath:locale/messages</value>
      </list>
    </property>
    
    <!-- 기본 인코딩 -->
    <property name="defaultEncoding" value="UTF-8"/>
    
    <!-- 캐시 갱신 간격 (60초) -->
    <property name="cacheSeconds" value="60"/>
  </bean>
</beans>
```

### 4.2 servlet-context.xml에서 Locale Interceptor 설정

```xml
<beans xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context">
  
  <mvc:interceptors>
    <mvc:interceptor>
      <mvc:mapping path="/testlocale.do"/>
      <mvc:mapping path="/**/*.do"/>
      
      <bean class="com.myspring.pro28.ex05.LocaleInterceptor"/>
    </mvc:interceptor>
  </mvc:interceptors>
</beans>
```

### 4.3 Locale Interceptor 구현

**LocaleInterceptor.java**
```java
public class LocaleInterceptor extends HandlerInterceptorAdapter {
  
  @Override
  public boolean preHandle(HttpServletRequest request,
                          HttpServletResponse response,
                          Object handler) throws Exception {
    HttpSession session = request.getSession();
    String locale = request.getParameter("locale");
    
    if (locale != null && !locale.trim().equals("")) {
      session.setAttribute(
        "org.springframework.web.servlet.i18n.SessionLocaleResolver.LOCALE",
        new Locale(locale));
    }
    
    return true;
  }
  
  @Override
  public void postHandle(HttpServletRequest request,
                        HttpServletResponse response,
                        Object handler,
                        ModelAndView modelAndView) throws Exception {
    // 후처리 로직
  }
  
  @Override
  public void afterCompletion(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler,
                             Exception ex) throws Exception {
    // 정리 로직
  }
}
```

### 4.4 Locale Controller

**LocaleController.java**
```java
@Controller
public class LocaleController {
  
  @RequestMapping(value="testlocale.do", method=RequestMethod.GET)
  public String locale(HttpServletRequest request,
                      HttpServletResponse response) throws Exception {
    return "locale";
  }
}
```

### 4.5 메시지 Properties 파일

**messages.properties (기본 - 영어)**
```properties
site.title=회원 정보
site.name=이름
site.job=직업
btn.send=전송
btn.cancel=취소
btn.finish=완료
name=홍길동
job=학생
```

**messages_en.properties (영어)**
```properties
site.title=Member Information
site.name=Name
site.job=Job
btn.send=Send
btn.cancel=Cancel
btn.finish=Finish
name=Hong Gil-dong
job=Student
```

**messages_ko.properties (한국어)**
```properties
site.title=회원 정보
site.name=이름
site.job=직업
btn.send=전송
btn.cancel=취소
btn.finish=완료
name=홍길동
job=학생
```

### 4.6 Locale JSP 뷰

**locale.jsp**
```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://www.springframework.org/tags" prefix="spring" %>
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title><spring:message code="site.title" text="회원 정보"/></title>
</head>
<body>
  <a href="${contextPath}/testlocale.do?locale=ko">한국어</a>
  <a href="${contextPath}/testlocale.do?locale=en">English</a>
  
  <h1><spring:message code="site.title" text="회원 정보"/></h1>
  
  <p>
    <spring:message code="site.name" text="이름 없음"/>:
    <spring:message code="name" text="이름 없음"/>
  </p>
  
  <p>
    <spring:message code="site.job" text="직업 없음"/>:
    <spring:message code="job" text="직업 없음"/>
  </p>
  
  <input type="button" value="<spring:message code='btn.send'/>">
  <input type="button" value="<spring:message code='btn.cancel'/>">
  <input type="button" value="<spring:message code='btn.finish'/>">
</body>
</html>
```

## 5. Interceptor 구현

### 5.1 HandlerInterceptor 개요

**Interceptor 처리 흐름:**
```
요청
   ↓
[preHandle()] → true: 진행, false: 중단
   ↓
Controller → 비즈니스 로직
   ↓
[postHandle()] → ModelAndView 수정 가능
   ↓
뷰 렌더링
   ↓
[afterCompletion()] → 뷰 렌더링 후 정리
   ↓
응답
```

### 5.2 ViewName Interceptor

**ViewNameInterceptor.java**
```java
public class ViewNameInterceptor extends HandlerInterceptorAdapter {
  
  @Override
  public boolean preHandle(HttpServletRequest request,
                          HttpServletResponse response,
                          Object handler) throws Exception {
    try {
      String viewName = getViewName(request);
      request.setAttribute("viewName", viewName);
    } catch (Exception e) {
      e.printStackTrace();
    }
    return true;
  }
  
  @Override
  public void postHandle(HttpServletRequest request,
                        HttpServletResponse response,
                        Object handler,
                        ModelAndView modelAndView) throws Exception {
    // 후처리
  }
  
  @Override
  public void afterCompletion(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler,
                             Exception ex) throws Exception {
    // 정리
  }
  
  private String getViewName(HttpServletRequest request) throws Exception {
    String contextPath = request.getContextPath();
    String uri = request.getAttribute("javax.servlet.include.request_uri") != null
                 ? (String) request.getAttribute("javax.servlet.include.request_uri")
                 : request.getRequestURI();
    
    int viewNameIndex = uri.indexOf(contextPath) + contextPath.length();
    String viewName = uri.substring(viewNameIndex);
    
    if (viewName.lastIndexOf(".") != -1) {
      viewName = viewName.substring(0, viewName.lastIndexOf("."));
    }
    return viewName;
  }
}
```

### 5.3 Interceptor를 사용하는 Controller

**MemberControllerImpl.java**
```java
@Controller("memberController")
public class MemberControllerImpl implements MemberController {
  @Autowired
  private MemberService memberService;
  
  @RequestMapping(value="member/listMembers.do", method=RequestMethod.GET)
  public ModelAndView listMembers(HttpServletRequest request,
                                  HttpServletResponse response) throws Exception {
    String viewName = (String) request.getAttribute("viewName");
    
    List<MemberVO> membersList = memberService.listMembers();
    ModelAndView mav = new ModelAndView(viewName);
    mav.addObject("membersList", membersList);
    
    return mav;
  }
  
  @RequestMapping(value="member/loginForm.do", method=RequestMethod.GET)
  public ModelAndView form(@RequestParam(value="result", required=false) String result,
                          HttpServletRequest request,
                          HttpServletResponse response) throws Exception {
    String viewName = (String) request.getAttribute("viewName");
    
    ModelAndView mav = new ModelAndView();
    mav.addObject("result", result);
    mav.setViewName(viewName);
    
    return mav;
  }
}
```

### 5.4 servlet-context.xml에서 Interceptor 등록

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context">
  
  <mvc:interceptors>
    <mvc:interceptor>
      <mvc:mapping path="/*.do"/>
      <mvc:mapping path="/**/*.do"/>
      
      <bean class="com.myspring.pro27.member.interceptor.ViewNameInterceptor"/>
    </mvc:interceptor>
  </mvc:interceptors>
</beans>
```

</details>
