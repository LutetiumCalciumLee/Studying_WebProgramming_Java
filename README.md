# Servlet Business Logic Processing
---

## 1. Overview of Servlets and Business Logic

* Learn how a Servlet processes web requests and handles **business logic** by interacting with a database.
* Examples: user registration, login, product ordering, etc., all require database interaction.
* Structure involves:

  * DAO (Data Access Object)
  * VO (Value Object or DTO)
  * SQL processing using JDBC

---

## 2. JDBC Integration and Processing Flow

* Entire process flow:

  1. Web browser sends a request.
  2. Servlet processes the request.
  3. Calls DAO.
  4. Executes SQL query.
  5. Stores results in an ArrayList.
  6. Outputs results as HTML.

* Sample table: `t_member`

  * Columns: `id`, `pwd`, `name`, `email`, `joinDate`

---

## 3. Using PreparedStatement for SQL

* Reuses compiled SQL for better performance.
* Use `?` placeholders for parameter binding → easier maintenance.

```java
String query = "SELECT * FROM t_member WHERE id = ?";
PreparedStatement pstmt = conn.prepareStatement(query);
pstmt.setString(1, id);
ResultSet rs = pstmt.executeQuery();
```

---

## 4. Connection Pool and JNDI

* Problem: Connecting/disconnecting to DB on every request degrades performance.
* Solution: Use a connection pool to reuse DB connections.
* Example configuration (`context.xml` in Tomcat):

```xml
<Resource name="jdbc/oracle" 
          auth="Container" 
          type="javax.sql.DataSource"
          maxActive="20"
          maxIdle="10"
          maxWait="10000"
          driverClassName="oracle.jdbc.OracleDriver"
          url="jdbc:oracle:thin:@localhost:1521:xe"
          username="scott" 
          password="tiger"/>
```

* Using connection pool in DAO:

```java
Context ctx = new InitialContext();
DataSource ds = (DataSource) ctx.lookup("java:comp/env/jdbc/oracle");
Connection conn = ds.getConnection();
```

---

## 5. Implementing Member Registration (Insert)

* Input form: `memberForm.html`

  * Fields: `id`, `pwd`, `name`, `email`
  * Method: POST
  * Hidden field: `command=addMember`

```html
<form method="post" action="/member3">
  <input type="hidden" name="command" value="addMember">
  <!-- Other input fields -->
</form>
```

* Servlet-side logic:

```java
if (command.equals("addMember")) {
    String id = request.getParameter("id");
    MemberVO memberVO = new MemberVO(id, ...);
    memberDAO.addMember(memberVO);
}
```

* DAO logic:

```java
String query = "INSERT INTO t_member (id, pwd, name, email) VALUES (?, ?, ?, ?)";
PreparedStatement pstmt = conn.prepareStatement(query);
pstmt.setString(1, member.getId());
...
pstmt.executeUpdate();
```

---

## 6. Implementing Member Deletion (Delete)

* Delete link example:

```html
<a href="/member3?command=delMember&id=user1">Delete</a>
```

* Servlet logic:

```java
if (command.equals("delMember")) {
    String id = request.getParameter("id");
    memberDAO.delMember(id);
}
```

* DAO logic:

```java
String query = "DELETE FROM t_member WHERE id=?";
PreparedStatement pstmt = conn.prepareStatement(query);
pstmt.setString(1, id);
pstmt.executeUpdate();
```
