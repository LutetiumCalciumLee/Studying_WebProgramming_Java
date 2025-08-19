# Chapter 17: Efficient Development with Model 2 Approach
***
## 17.1 Web Application Models
- **Definition**: A standardized source structure commonly used in application development
- **Types**: Model 1 and Model 2

### 17.1.1 Model 1 Approach
- **Characteristics**: JSP handles both business logic operations (like database connections) and display operations
- **Structure**: JSP is responsible for all client requests and business logic processing
- **Advantages**: Easy and convenient feature implementation
- **Disadvantages**: Difficult to maintain

### 17.1.2 Model 2 Approach
- **Characteristics**: Separates web application functions (client request processing, response processing, business logic processing)
- **Principle**: Based on object-oriented programming concept of modularizing each function
- **Advantages**:
  - Easy development and maintenance due to separated functions
  - High reusability of each function (module)
  - Easy development through division of work between designers and developers
- **Disadvantages**: Requires learning of Model 2-related functions and concepts

***
## 17.2 MVC Design Pattern
- **Definition**: Model-View-Controller abbreviation, adapting design patterns used in general PC program development for web applications
- **Purpose**: Developing web applications by dividing them into screen parts, request processing parts, and logic processing parts

### MVC Components and Functions
- **Controller (Servlet)**: 
  - Handles user requests and flow control
  - Analyzes client requests
  - Calls necessary models for requests
  - Selects JSP to display results processed by Model
- **Model (DAO, VO)**: 
  - Performs business logic such as database connections
  - Generally consists of DAO and VO classes
- **View (JSP)**: 
  - Handles screen functions
  - Displays results processed by Model on screen

***
## 17.3 Member Management using MVC

### 17.3.1 Member Information Inquiry Function
**Process Flow**:
1. Browser requests `/mem.do`
2. Servlet MemberController receives request and calls MemberDAO's `listMembers()` method
3. MemberDAO queries member information with SQL and returns data set in MemberVO
4. MemberController forwards queried member information to member list page (listMembers.jsp)
5. Member list page displays forwarded member information as a list

### 17.3.2 Member Information Addition Function
- **Command Pattern**: Method for browsers to request controllers using URL patterns
- **URL Format**: `http://localhost:8090/pro17/member/listMembers.do`
  - `/member`: Indicates member function
  - `/listMembers.do`: Indicates member inquiry function among member functions
- **Implementation**: Uses HttpServletRequest's `getPathInfo()` method to receive request names from URL patterns

### 17.3.3 Member Information Modification and Deletion
- **Modification Process**: Update member information through modification form and redirect to member list
- **Deletion Process**: Delete member using member ID and SQL statements

***
## 17.4 Implementing Reply Board with Model 2

### Reply Board Table (t_board) Structure
| Column | Attribute | Data Type | Size | Unique Key | NULL | Key | Default |
|--------|-----------|-----------|------|------------|------|-----|---------|
| articleNO | Article Number | number | 10 | Y | N | Primary Key | |
| parentNO | Parent Article Number | number | 10 | N | N | | 0 |
| title | Article Title | varchar2 | 100 | N | N | | |
| content | Article Content | varchar2 | 4000 | N | N | | |
| imageFileName | Image File Name | varchar2 | 100 | N | N | | |
| writeDate | Write Date | date | | N | N | | sysdate |
| id | Author ID | varchar2 | 20 | N | N | Foreign Key | |

### Service Class Purpose
- **Unit Function**: A logical function from user's perspective
- **Examples**:
  - Board article viewing and view count update
  - Product order registration and customer point update in shopping mall
  - Sender and receiver balance update in bank transfers
- **Benefits**: Much more advantageous for maintenance and system scalability

### 17.4.1 Board Article List View Implementation
**Hierarchical SQL Structure**:
1. `START WITH parentNO=0`: Specifies conditions for top-level rows
2. `CONNECT BY PRIOR articleNO = parentNO`: Describes hierarchical connections
3. `ORDER BY articleNO DESC`: Final sorting in descending order

### 17.4.2 Board Article Writing Implementation
**Process**:
1. Request article writing form from article list page
2. Enter article and request controller with `/board/addArticle.do`
3. Controller passes article information to Service class to add to table
4. Controller requests `/board/listArticles.do` again to display all articles

**File Upload Process**:
1. Upload files to temp folder temporarily
2. Add article to table and get new article number
3. Create folder with article number and move files from temp folder

### 17.4.3 Article Detail Function Implementation
**Process**:
1. Click article title to request `/board/viewArticle.do?articleNO=article_number`
2. Controller queries article information and forwards to detail page
3. Display article information and image files

### 17.4.4 Article Modification Function Implementation
**Process**:
1. Click modify to activate input fields
2. Modify article information and image, then request `/board/modArticle.do`
3. Use `upload()` method to store modified data and return
4. Reflect modified data in table and move modified images
5. Delete original image files

### 17.4.5 Article Deletion Function Implementation
**Process**:
1. Click delete to request `/board/removeArticle.do`
2. Delete article and related child articles
3. Delete image file storage folders

### 17.4.6 Reply Writing Function Implementation
**Process**:
1. Click reply writing to send parent article number with `/board/replyForm.do`
2. Write reply and request `/board/addReply.do`
3. Add reply information to board table with parent article number

**Implementation Details**:
- Store parent article number in session as parentNO attribute
- Retrieve parentNO from session when adding reply to table

### 17.4.7 Board Paging Function Implementation
**Paging Method**:
- 10 articles displayed per page
- 10 pages form one section
- Section 1: pages 1-10, Section 2: pages 11-20
- User clicks  sends section=1, pageNum=2 to server
---
