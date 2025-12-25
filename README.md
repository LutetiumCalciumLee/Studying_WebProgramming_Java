<details>
<summary>ENG (English Version)</summary>

# Spring REST API Usage

## 1. What is REST?

**REST (Representational State Transfer)** is an architectural style for web services that uses HTTP methods to perform CRUD operations on resources identified by URIs.

**Key Characteristics:**
- **URI-based Resource Identification:** Each resource has a unique URI
- **HTTP Method-based Operations:** GET (read), POST (create), PUT (update), DELETE (delete)
- **Stateless Communication:** Each request contains all necessary information
- **JSON/XML Responses:** Standardized data formats for client-server communication

**Related Terms:**
- **REST API:** Web API that follows REST principles
- **RESTful API:** A REST-compliant web service
- **Open API:** Publicly available REST APIs

## 2. @RestController

**Purpose:** Combines `@Controller` and `@ResponseBody` annotations to return data directly as JSON/XML instead of view names.

### 2.1 Basic RestController Setup

**Step 1: Add Jackson Dependency (JSON support)**
```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.5.4</version>
</dependency>
```

### 2.2 Simple String Response

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("hello")
  public String hello() {
    return "Hello REST!!";
  }
}
```

**HTTP Request:** `GET http://localhost:8090/pro29/test/hello`

**Response:**
```
Hello REST!!
```

### 2.3 Object Response (JSON)

**TestController.java (Enhanced)**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("member")
  public MemberVO member() {
    MemberVO vo = new MemberVO();
    vo.setId("hong");
    vo.setPwd("1234");
    vo.setName("Hong Gil-dong");
    vo.setEmail("hong@test.com");
    return vo;  // Automatically converted to JSON
  }
}
```

**MemberVO.java**
```java
public class MemberVO {
  private String id;
  private String pwd;
  private String name;
  private String email;
  
  // Getters and Setters
  public String getId() { return id; }
  public void setId(String id) { this.id = id; }
  
  public String getPwd() { return pwd; }
  public void setPwd(String pwd) { this.pwd = pwd; }
  
  public String getName() { return name; }
  public void setName(String name) { this.name = name; }
  
  public String getEmail() { return email; }
  public void setEmail(String email) { this.email = email; }
  
  @Override
  public String toString() {
    return String.format("id=%s, pwd=%s, name=%s, email=%s", 
                         id, pwd, name, email);
  }
}
```

**HTTP Request:** `GET http://localhost:8090/pro29/test/member`

**JSON Response:**
```json
{
  "id": "hong",
  "pwd": "1234",
  "name": "Hong Gil-dong",
  "email": "hong@test.com"
}
```

### 2.4 List Response

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("membersList")
  public List<MemberVO> listMembers() {
    List<MemberVO> list = new ArrayList<MemberVO>();
    
    for (int i = 0; i < 10; i++) {
      MemberVO vo = new MemberVO();
      vo.setId("hong" + i);
      vo.setPwd("123" + i);
      vo.setName("Name" + i);
      vo.setEmail("hong" + i + "@test.com");
      list.add(vo);
    }
    
    return list;  // List automatically converted to JSON array
  }
}
```

**HTTP Request:** `GET http://localhost:8090/pro29/test/membersList`

**JSON Response:**
```json
[
  {"id":"hong0","pwd":"1230","name":"Name0","email":"hong0@test.com"},
  {"id":"hong1","pwd":"1231","name":"Name1","email":"hong1@test.com"},
  ...
]
```

### 2.5 Map Response

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("membersMap")
  public Map<Integer, MemberVO> membersMap() {
    Map<Integer, MemberVO> map = new HashMap<Integer, MemberVO>();
    
    for (int i = 0; i < 10; i++) {
      MemberVO vo = new MemberVO();
      vo.setId("hong" + i);
      vo.setPwd("123" + i);
      vo.setName("Name" + i);
      vo.setEmail("hong" + i + "@test.com");
      map.put(i, vo);
    }
    
    return map;  // Map converted to JSON object
  }
}
```

**HTTP Request:** `GET http://localhost:8090/pro29/test/membersMap`

**JSON Response:**
```json
{
  "0": {"id":"hong0","pwd":"1230","name":"Name0","email":"hong0@test.com"},
  "1": {"id":"hong1","pwd":"1231","name":"Name1","email":"hong1@test.com"},
  ...
}
```

## 3. @PathVariable

**Purpose:** Extract values from URI path segments.

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping(value="notice/{num}", method=RequestMethod.GET)
  public int notice(@PathVariable int num) throws Exception {
    return num;  // Returns the path variable value
  }
}
```

**HTTP Request:** `GET http://localhost:8090/pro29/test/notice/112`

**Response:**
```
112
```

**Extract Multiple Variables:**
```java
@RequestMapping(value="users/{userId}/posts/{postId}")
public String getUserPost(@PathVariable int userId, 
                          @PathVariable int postId) {
  return "User: " + userId + ", Post: " + postId;
}
```

## 4. @RequestBody and @ResponseBody

### 4.1 @RequestBody - Receive JSON

**Purpose:** Parse incoming JSON request body into Java object.

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  private static Logger logger = LoggerFactory.getLogger(TestController.class);
  
  @RequestMapping(value="info", method=RequestMethod.POST)
  public void modify(@RequestBody MemberVO vo) {
    logger.info(vo.toString());  // Log the received object
  }
}
```

**JavaScript/jQuery (JSONTest.jsp)**
```jsp
<script src="http://code.jquery.com/jquery-latest.js"></script>
<script>
  function checkJson() {
    var member = {
      id: "park",
      name: "Park Hoon",
      pwd: "1234",
      email: "park@test.com"
    };
    
    $.ajax({
      type: "POST",
      url: "${contextPath}/test/info",
      contentType: "application/json",
      data: JSON.stringify(member),
      success: function(data, textStatus) {
        alert("Successfully received!");
      },
      error: function(data, textStatus) {
        alert("Error occurred");
      }
    });
  }
</script>
<input type="button" id="checkJson" value="Send JSON" onclick="checkJson()">
```

**Server Console Output:**
```
INFO TestController - id=park, pwd=1234, name=Park Hoon, email=park@test.com
```

### 4.2 @ResponseBody - Send JSON

**Purpose:** Convert return value to JSON format (handled automatically by @RestController).

**Controller.java**
```java
@Controller
public class ResController {
  
  @RequestMapping("res1")
  @ResponseBody  // Explicitly specify JSON response
  public Map<String, Object> res1() {
    Map<String, Object> map = new HashMap<String, Object>();
    map.put("id", "hong");
    map.put("name", "Hong Gil-dong");
    return map;
  }
  
  @RequestMapping("res2")
  public ModelAndView res2() {
    return new ModelAndView("home");  // Returns JSP view
  }
}
```

**Response for res1:**
```json
{"id":"hong","name":"Hong Gil-dong"}
```

**Response for res2:**
```html
<!-- home.jsp HTML content -->
Hello world!
```

### 4.3 ResponseEntity - Control HTTP Status

**Purpose:** Control HTTP status code and headers in response.

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("membersList2")
  public ResponseEntity<List<MemberVO>> listMembers2() {
    List<MemberVO> list = new ArrayList<MemberVO>();
    
    for (int i = 0; i < 10; i++) {
      MemberVO vo = new MemberVO();
      vo.setId("lee" + i);
      vo.setPwd("123" + i);
      vo.setName("Name" + i);
      vo.setEmail("lee" + i + "@test.com");
      list.add(vo);
    }
    
    // Return with HTTP 500 status
    return new ResponseEntity<List<MemberVO>>(
      list, HttpStatus.INTERNAL_SERVER_ERROR);
  }
}
```

**With Custom Headers:**
```java
@RequestMapping("res3")
public ResponseEntity<String> res3() {
  HttpHeaders responseHeaders = new HttpHeaders();
  responseHeaders.add("Content-Type", "text/html; charset=utf-8");
  
  String message = "<script>";
  message += "alert('Success!');";
  message += "location.href='pro29/test/membersList2';";
  message += "</script>";
  
  return new ResponseEntity<String>(message, responseHeaders, 
                                    HttpStatus.CREATED);
}
```

## 5. REST API URI Design

**HTTP Methods and Operations:**
| Method | Operation | Example |
|--------|-----------|---------|
| **GET** | Read | GET /boards (list), GET /boards/123 (detail) |
| **POST** | Create | POST /boards (create new) |
| **PUT** | Update | PUT /boards/123 (update #123) |
| **DELETE** | Delete | DELETE /boards/123 (delete #123) |

### 5.1 Complete CRUD REST API

**ArticleVO.java**
```java
public class ArticleVO {
  private Integer articleNO;
  private String writer;
  private String title;
  private String content;
  
  // Getters and Setters
  public Integer getArticleNO() { return articleNO; }
  public void setArticleNO(Integer articleNO) { this.articleNO = articleNO; }
  
  public String getWriter() { return writer; }
  public void setWriter(String writer) { this.writer = writer; }
  
  public String getTitle() { return title; }
  public void setTitle(String title) { this.title = title; }
  
  public String getContent() { return content; }
  public void setContent(String content) { this.content = content; }
  
  @Override
  public String toString() {
    return String.format("NO=%d, writer=%s, title=%s, content=%s",
                         articleNO, writer, title, content);
  }
}
```

**BoardController.java - Full CRUD**
```java
@RestController
@RequestMapping("boards")
public class BoardController {
  private static Logger logger = LoggerFactory.getLogger(BoardController.class);
  
  // READ - Get all
  @RequestMapping(value="all", method=RequestMethod.GET)
  public ResponseEntity<List<ArticleVO>> listArticles() {
    logger.info("listArticles");
    List<ArticleVO> list = new ArrayList<ArticleVO>();
    
    for (int i = 0; i < 10; i++) {
      ArticleVO vo = new ArticleVO();
      vo.setArticleNO(i);
      vo.setWriter("Hong");
      vo.setTitle("Title" + i);
      vo.setContent("Content" + i);
      list.add(vo);
    }
    
    return new ResponseEntity<List<ArticleVO>>(list, HttpStatus.OK);
  }
  
  // READ - Get specific article
  @RequestMapping(value="{articleNO}", method=RequestMethod.GET)
  public ResponseEntity<ArticleVO> findArticle(
      @PathVariable Integer articleNO) {
    logger.info("findArticle");
    
    ArticleVO vo = new ArticleVO();
    vo.setArticleNO(articleNO);
    vo.setWriter("Hong");
    vo.setTitle("Title" + articleNO);
    vo.setContent("Content" + articleNO);
    
    return new ResponseEntity<ArticleVO>(vo, HttpStatus.OK);
  }
  
  // CREATE - Add new article
  @RequestMapping(value="", method=RequestMethod.POST)
  public ResponseEntity<String> addArticle(@RequestBody ArticleVO articleVO) {
    ResponseEntity<String> resEntity = null;
    
    try {
      logger.info("addArticle");
      logger.info(articleVO.toString());
      resEntity = new ResponseEntity<String>("ADD SUCCEEDED", HttpStatus.OK);
    } catch (Exception e) {
      resEntity = new ResponseEntity<String>(e.getMessage(), 
                                             HttpStatus.BAD_REQUEST);
    }
    
    return resEntity;
  }
  
  // UPDATE - Modify article
  @RequestMapping(value="{articleNO}", method=RequestMethod.PUT)
  public ResponseEntity<String> modArticle(
      @PathVariable Integer articleNO,
      @RequestBody ArticleVO articleVO) {
    ResponseEntity<String> resEntity = null;
    
    try {
      logger.info("modArticle");
      logger.info("ArticleNO: " + articleNO);
      logger.info(articleVO.toString());
      resEntity = new ResponseEntity<String>("MOD SUCCEEDED", HttpStatus.OK);
    } catch (Exception e) {
      resEntity = new ResponseEntity<String>(e.getMessage(), 
                                             HttpStatus.BAD_REQUEST);
    }
    
    return resEntity;
  }
  
  // DELETE - Remove article
  @RequestMapping(value="{articleNO}", method=RequestMethod.DELETE)
  public ResponseEntity<String> removeArticle(
      @PathVariable Integer articleNO) {
    ResponseEntity<String> resEntity = null;
    
    try {
      logger.info("removeArticle");
      logger.info("ArticleNO: " + articleNO);
      resEntity = new ResponseEntity<String>("REMOVE SUCCEEDED", HttpStatus.OK);
    } catch (Exception e) {
      resEntity = new ResponseEntity<String>(e.getMessage(), 
                                             HttpStatus.BAD_REQUEST);
    }
    
    return resEntity;
  }
}
```

### 5.2 Testing REST API with AJAX

**JSONTest2.jsp**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" %>
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>REST API Test</title>
  <script src="http://code.jquery.com/jquery-latest.js"></script>
  <script>
    function checkJson() {
      var article = {
        articleNO: 114,
        writer: "Park",
        title: "REST API Tutorial",
        content: "Learn REST API in Spring"
      };
      
      // POST - Create new article
      $.ajax({
        type: "POST",
        url: "${contextPath}/boards",
        contentType: "application/json",
        data: JSON.stringify(article),
        success: function(data, textStatus) {
          alert("POST Success: " + data);
        },
        error: function(data, textStatus) {
          alert("POST Error");
        }
      });
      
      // PUT - Update article #114
      $.ajax({
        type: "PUT",
        url: "${contextPath}/boards/114",
        contentType: "application/json",
        data: JSON.stringify(article),
        success: function(data, textStatus) {
          alert("PUT Success: " + data);
        },
        error: function(data, textStatus) {
          alert("PUT Error");
        }
      });
    }
  </script>
</head>
<body>
  <h1>REST API Test</h1>
  <input type="button" id="checkJson" value="Test CRUD" onclick="checkJson()">
  <div id="output"></div>
</body>
</html>
```

**Server Console Output:**
```
INFO BoardController - listArticles
INFO BoardController - findArticle
INFO BoardController - addArticle
INFO BoardController - NO=114, writer=Park, title=REST API Tutorial, content=Learn REST API in Spring
INFO BoardController - modArticle
INFO BoardController - ArticleNO: 114
INFO BoardController - NO=114, writer=Park, title=REST API Tutorial, content=Learn REST API in Spring
INFO BoardController - removeArticle
INFO BoardController - ArticleNO: 114
```

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 스프링 REST API 사용하기

## 1. REST란?

**REST (Representational State Transfer)**는 URI로 식별되는 리소스에 대해 HTTP 메서드를 사용하여 CRUD 작업을 수행하는 웹 서비스 아키텍처 스타일입니다.

**주요 특징:**
- **URI 기반 리소스 식별:** 각 리소스는 고유한 URI를 가짐
- **HTTP 메서드 기반 작업:** GET (읽기), POST (생성), PUT (수정), DELETE (삭제)
- **상태 비보존 통신:** 각 요청은 필요한 모든 정보를 포함함
- **JSON/XML 응답:** 클라이언트-서버 통신의 표준화된 데이터 포맷

**관련 용어:**
- **REST API:** REST 원칙을 따르는 웹 API
- **RESTful API:** REST 준수 웹 서비스
- **Open API:** 공개적으로 이용 가능한 REST API

## 2. @RestController

**목적:** `@Controller`와 `@ResponseBody`를 결합하여 뷰 이름 대신 JSON/XML 데이터를 직접 반환합니다.

### 2.1 기본 RestController 설정

**Step 1: Jackson 의존성 추가 (JSON 지원)**
```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.5.4</version>
</dependency>
```

### 2.2 간단한 문자열 응답

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("hello")
  public String hello() {
    return "Hello REST!!";
  }
}
```

**HTTP 요청:** `GET http://localhost:8090/pro29/test/hello`

**응답:**
```
Hello REST!!
```

### 2.3 객체 응답 (JSON)

**TestController.java (향상된 버전)**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("member")
  public MemberVO member() {
    MemberVO vo = new MemberVO();
    vo.setId("hong");
    vo.setPwd("1234");
    vo.setName("홍길동");
    vo.setEmail("hong@test.com");
    return vo;  // 자동으로 JSON으로 변환됨
  }
}
```

**MemberVO.java**
```java
public class MemberVO {
  private String id;
  private String pwd;
  private String name;
  private String email;
  
  // Getter와 Setter
  public String getId() { return id; }
  public void setId(String id) { this.id = id; }
  
  public String getPwd() { return pwd; }
  public void setPwd(String pwd) { this.pwd = pwd; }
  
  public String getName() { return name; }
  public void setName(String name) { this.name = name; }
  
  public String getEmail() { return email; }
  public void setEmail(String email) { this.email = email; }
  
  @Override
  public String toString() {
    return String.format("id=%s, pwd=%s, name=%s, email=%s", 
                         id, pwd, name, email);
  }
}
```

**HTTP 요청:** `GET http://localhost:8090/pro29/test/member`

**JSON 응답:**
```json
{
  "id": "hong",
  "pwd": "1234",
  "name": "홍길동",
  "email": "hong@test.com"
}
```

### 2.4 리스트 응답

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("membersList")
  public List<MemberVO> listMembers() {
    List<MemberVO> list = new ArrayList<MemberVO>();
    
    for (int i = 0; i < 10; i++) {
      MemberVO vo = new MemberVO();
      vo.setId("hong" + i);
      vo.setPwd("123" + i);
      vo.setName("이름" + i);
      vo.setEmail("hong" + i + "@test.com");
      list.add(vo);
    }
    
    return list;  // 리스트는 자동으로 JSON 배열로 변환됨
  }
}
```

**HTTP 요청:** `GET http://localhost:8090/pro29/test/membersList`

**JSON 응답:**
```json
[
  {"id":"hong0","pwd":"1230","name":"이름0","email":"hong0@test.com"},
  {"id":"hong1","pwd":"1231","name":"이름1","email":"hong1@test.com"},
  ...
]
```

### 2.5 맵 응답

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("membersMap")
  public Map<Integer, MemberVO> membersMap() {
    Map<Integer, MemberVO> map = new HashMap<Integer, MemberVO>();
    
    for (int i = 0; i < 10; i++) {
      MemberVO vo = new MemberVO();
      vo.setId("hong" + i);
      vo.setPwd("123" + i);
      vo.setName("이름" + i);
      vo.setEmail("hong" + i + "@test.com");
      map.put(i, vo);
    }
    
    return map;  // 맵은 JSON 객체로 변환됨
  }
}
```

**HTTP 요청:** `GET http://localhost:8090/pro29/test/membersMap`

**JSON 응답:**
```json
{
  "0": {"id":"hong0","pwd":"1230","name":"이름0","email":"hong0@test.com"},
  "1": {"id":"hong1","pwd":"1231","name":"이름1","email":"hong1@test.com"},
  ...
}
```

## 3. @PathVariable

**목적:** URI 경로 세그먼트에서 값을 추출합니다.

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping(value="notice/{num}", method=RequestMethod.GET)
  public int notice(@PathVariable int num) throws Exception {
    return num;  // 경로 변수 값을 반환
  }
}
```

**HTTP 요청:** `GET http://localhost:8090/pro29/test/notice/112`

**응답:**
```
112
```

**여러 변수 추출:**
```java
@RequestMapping(value="users/{userId}/posts/{postId}")
public String getUserPost(@PathVariable int userId, 
                          @PathVariable int postId) {
  return "User: " + userId + ", Post: " + postId;
}
```

## 4. @RequestBody와 @ResponseBody

### 4.1 @RequestBody - JSON 수신

**목적:** 수신 JSON 요청 본문을 Java 객체로 파싱합니다.

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  private static Logger logger = LoggerFactory.getLogger(TestController.class);
  
  @RequestMapping(value="info", method=RequestMethod.POST)
  public void modify(@RequestBody MemberVO vo) {
    logger.info(vo.toString());  // 수신한 객체 로깅
  }
}
```

**JavaScript/jQuery (JSONTest.jsp)**
```jsp
<script src="http://code.jquery.com/jquery-latest.js"></script>
<script>
  function checkJson() {
    var member = {
      id: "park",
      name: "박훈",
      pwd: "1234",
      email: "park@test.com"
    };
    
    $.ajax({
      type: "POST",
      url: "${contextPath}/test/info",
      contentType: "application/json",
      data: JSON.stringify(member),
      success: function(data, textStatus) {
        alert("성공적으로 수신되었습니다!");
      },
      error: function(data, textStatus) {
        alert("오류 발생");
      }
    });
  }
</script>
<input type="button" id="checkJson" value="JSON 전송" onclick="checkJson()">
```

**서버 콘솔 출력:**
```
INFO TestController - id=park, pwd=1234, name=박훈, email=park@test.com
```

### 4.2 @ResponseBody - JSON 전송

**목적:** 반환 값을 JSON 형식으로 변환합니다 (@RestController에서 자동 처리).

**Controller.java**
```java
@Controller
public class ResController {
  
  @RequestMapping("res1")
  @ResponseBody  // JSON 응답 명시적 지정
  public Map<String, Object> res1() {
    Map<String, Object> map = new HashMap<String, Object>();
    map.put("id", "hong");
    map.put("name", "홍길동");
    return map;
  }
  
  @RequestMapping("res2")
  public ModelAndView res2() {
    return new ModelAndView("home");  // JSP 뷰 반환
  }
}
```

**res1 응답:**
```json
{"id":"hong","name":"홍길동"}
```

**res2 응답:**
```html
<!-- home.jsp HTML 콘텐츠 -->
Hello world!
```

### 4.3 ResponseEntity - HTTP 상태 제어

**목적:** 응답의 HTTP 상태 코드와 헤더를 제어합니다.

**TestController.java**
```java
@RestController
@RequestMapping("test")
public class TestController {
  
  @RequestMapping("membersList2")
  public ResponseEntity<List<MemberVO>> listMembers2() {
    List<MemberVO> list = new ArrayList<MemberVO>();
    
    for (int i = 0; i < 10; i++) {
      MemberVO vo = new MemberVO();
      vo.setId("lee" + i);
      vo.setPwd("123" + i);
      vo.setName("이름" + i);
      vo.setEmail("lee" + i + "@test.com");
      list.add(vo);
    }
    
    // HTTP 500 상태로 반환
    return new ResponseEntity<List<MemberVO>>(
      list, HttpStatus.INTERNAL_SERVER_ERROR);
  }
}
```

**사용자 정의 헤더 포함:**
```java
@RequestMapping("res3")
public ResponseEntity<String> res3() {
  HttpHeaders responseHeaders = new HttpHeaders();
  responseHeaders.add("Content-Type", "text/html; charset=utf-8");
  
  String message = "<script>";
  message += "alert('성공!');";
  message += "location.href='pro29/test/membersList2';";
  message += "</script>";
  
  return new ResponseEntity<String>(message, responseHeaders, 
                                    HttpStatus.CREATED);
}
```

## 5. REST API URI 설계

**HTTP 메서드와 작업:**
| 메서드 | 작업 | 예시 |
|--------|------|------|
| **GET** | 읽기 | GET /boards (목록), GET /boards/123 (상세) |
| **POST** | 생성 | POST /boards (새로 생성) |
| **PUT** | 수정 | PUT /boards/123 (#123 수정) |
| **DELETE** | 삭제 | DELETE /boards/123 (#123 삭제) |

### 5.1 완전한 CRUD REST API

**ArticleVO.java**
```java
public class ArticleVO {
  private Integer articleNO;
  private String writer;
  private String title;
  private String content;
  
  // Getter와 Setter
  public Integer getArticleNO() { return articleNO; }
  public void setArticleNO(Integer articleNO) { this.articleNO = articleNO; }
  
  public String getWriter() { return writer; }
  public void setWriter(String writer) { this.writer = writer; }
  
  public String getTitle() { return title; }
  public void setTitle(String title) { this.title = title; }
  
  public String getContent() { return content; }
  public void setContent(String content) { this.content = content; }
  
  @Override
  public String toString() {
    return String.format("NO=%d, writer=%s, title=%s, content=%s",
                         articleNO, writer, title, content);
  }
}
```

**BoardController.java - 완전한 CRUD**
```java
@RestController
@RequestMapping("boards")
public class BoardController {
  private static Logger logger = LoggerFactory.getLogger(BoardController.class);
  
  // READ - 모두 조회
  @RequestMapping(value="all", method=RequestMethod.GET)
  public ResponseEntity<List<ArticleVO>> listArticles() {
    logger.info("listArticles");
    List<ArticleVO> list = new ArrayList<ArticleVO>();
    
    for (int i = 0; i < 10; i++) {
      ArticleVO vo = new ArticleVO();
      vo.setArticleNO(i);
      vo.setWriter("홍길동");
      vo.setTitle("제목" + i);
      vo.setContent("내용" + i);
      list.add(vo);
    }
    
    return new ResponseEntity<List<ArticleVO>>(list, HttpStatus.OK);
  }
  
  // READ - 특정 글 조회
  @RequestMapping(value="{articleNO}", method=RequestMethod.GET)
  public ResponseEntity<ArticleVO> findArticle(
      @PathVariable Integer articleNO) {
    logger.info("findArticle");
    
    ArticleVO vo = new ArticleVO();
    vo.setArticleNO(articleNO);
    vo.setWriter("홍길동");
    vo.setTitle("제목" + articleNO);
    vo.setContent("내용" + articleNO);
    
    return new ResponseEntity<ArticleVO>(vo, HttpStatus.OK);
  }
  
  // CREATE - 새 글 추가
  @RequestMapping(value="", method=RequestMethod.POST)
  public ResponseEntity<String> addArticle(@RequestBody ArticleVO articleVO) {
    ResponseEntity<String> resEntity = null;
    
    try {
      logger.info("addArticle");
      logger.info(articleVO.toString());
      resEntity = new ResponseEntity<String>("추가 성공", HttpStatus.OK);
    } catch (Exception e) {
      resEntity = new ResponseEntity<String>(e.getMessage(), 
                                             HttpStatus.BAD_REQUEST);
    }
    
    return resEntity;
  }
  
  // UPDATE - 글 수정
  @RequestMapping(value="{articleNO}", method=RequestMethod.PUT)
  public ResponseEntity<String> modArticle(
      @PathVariable Integer articleNO,
      @RequestBody ArticleVO articleVO) {
    ResponseEntity<String> resEntity = null;
    
    try {
      logger.info("modArticle");
      logger.info("ArticleNO: " + articleNO);
      logger.info(articleVO.toString());
      resEntity = new ResponseEntity<String>("수정 성공", HttpStatus.OK);
    } catch (Exception e) {
      resEntity = new ResponseEntity<String>(e.getMessage(), 
                                             HttpStatus.BAD_REQUEST);
    }
    
    return resEntity;
  }
  
  // DELETE - 글 삭제
  @RequestMapping(value="{articleNO}", method=RequestMethod.DELETE)
  public ResponseEntity<String> removeArticle(
      @PathVariable Integer articleNO) {
    ResponseEntity<String> resEntity = null;
    
    try {
      logger.info("removeArticle");
      logger.info("ArticleNO: " + articleNO);
      resEntity = new ResponseEntity<String>("삭제 성공", HttpStatus.OK);
    } catch (Exception e) {
      resEntity = new ResponseEntity<String>(e.getMessage(), 
                                             HttpStatus.BAD_REQUEST);
    }
    
    return resEntity;
  }
}
```

### 5.2 AJAX를 사용한 REST API 테스트

**JSONTest2.jsp**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" %>
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>REST API 테스트</title>
  <script src="http://code.jquery.com/jquery-latest.js"></script>
  <script>
    function checkJson() {
      var article = {
        articleNO: 114,
        writer: "박훈",
        title: "REST API 튜토리얼",
        content: "스프링에서 REST API 배우기"
      };
      
      // POST - 새 글 생성
      $.ajax({
        type: "POST",
        url: "${contextPath}/boards",
        contentType: "application/json",
        data: JSON.stringify(article),
        success: function(data, textStatus) {
          alert("POST 성공: " + data);
        },
        error: function(data, textStatus) {
          alert("POST 실패");
        }
      });
      
      // PUT - 글 #114 수정
      $.ajax({
        type: "PUT",
        url: "${contextPath}/boards/114",
        contentType: "application/json",
        data: JSON.stringify(article),
        success: function(data, textStatus) {
          alert("PUT 성공: " + data);
        },
        error: function(data, textStatus) {
          alert("PUT 실패");
        }
      });
    }
  </script>
</head>
<body>
  <h1>REST API 테스트</h1>
  <input type="button" id="checkJson" value="CRUD 테스트" onclick="checkJson()">
  <div id="output"></div>
</body>
</html>
```

**서버 콘솔 출력:**
```
INFO BoardController - listArticles
INFO BoardController - findArticle
INFO BoardController - addArticle
INFO BoardController - NO=114, writer=박훈, title=REST API 튜토리얼, content=스프링에서 REST API 배우기
INFO BoardController - modArticle
INFO BoardController - ArticleNO: 114
INFO BoardController - NO=114, writer=박훈, title=REST API 튜토리얼, content=스프링에서 REST API 배우기
INFO BoardController - removeArticle
INFO BoardController - ArticleNO: 114
```

</details>
