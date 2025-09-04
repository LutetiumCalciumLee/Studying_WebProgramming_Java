# Chapter 22: Spring JDBC Features

---

## 1. Configuration for Database Integration
- Spring JDBC setup is initiated by configuring `web.xml` to use `ContextLoaderListener`, which loads database-related configuration files (`action-service.xml`, `action-dataSource.xml`).
- The `action-dataSource.xml` file configures the database connection by defining a `dataSource` bean. It uses `PropertyPlaceholderConfigurer` to pull connection details (driver, URL, username, password) from an external `jdbc.properties` file.
- Dependency injection is used to link components: the `dataSource` bean is injected into the `memberDAO` bean, which is then injected into the `memberService` bean, and finally, the service is injected into the `memberController`.

## 2. Using JdbcTemplate for Data Access
- In the Data Access Object (DAO) layer (`MemberDAOImpl`), a `JdbcTemplate` instance is created by passing the injected `dataSource` bean to its constructor. This simplifies database operations.
- To retrieve data, the `jdbcTemplate.query()` method is used. It takes a SQL query string and a `RowMapper` implementation, which maps each row of the `ResultSet` to a `MemberVO` object, returning a list of objects.
- For data manipulation (insert, update, delete), the `jdbcTemplate.update()` method is called with a SQL DML statement. This method returns the number of rows affected by the query.

## 3. Practice: Displaying Member List
- The request flow begins when a user accesses a URL like `/member/listMembers.do`. The `DispatcherServlet` routes this to the `listMembers` method in `MemberControllerImpl`.
- The controller calls the `listMembers()` method on the injected `memberService`, which in turn calls the `selectAllMembers()` method on the `memberDAO` to fetch the data from the database.
- The controller receives the list of members (`membersList`), adds it to a `ModelAndView` object, and returns it. `ViewResolver` then selects the appropriate JSP file (`listMembers.jsp`).
- The `listMembers.jsp` view uses JSTL's `<c:forEach>` tag to iterate through the `membersList` and display the member information (ID, password, name, email, join date) in an HTML table.
