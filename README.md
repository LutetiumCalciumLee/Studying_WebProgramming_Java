# Chapter 23: Using the MyBatis Framework
***
## 1. Introduction to MyBatis
MyBatis is a persistence framework that simplifies database access in Java applications by separating SQL queries from the application code. Unlike traditional JDBC, where SQL statements are embedded within Java methods, MyBatis externalizes them into XML files. This approach improves code readability and makes SQL queries easier to manage and maintain. The framework handles the complexities of mapping query parameters and results between Java objects (like VOs or HashMaps) and the database, and also provides features for data source management and transaction control.

## 2. Configuration and Core Components
Setting up MyBatis involves two main types of XML files:
- **`SqlMapConfig.xml`**: This is the central configuration file. It defines crucial settings such as database connection details (driver, URL, credentials) within an `<environments>` block, sets up type aliases for Java objects (e.g., `MemberVO`) using `<typeAliases>` for brevity, and registers the locations of mapper files using the `<mappers>` tag.
- **Mapper XML Files (e.g., `member.xml`)**: These files contain the actual SQL statements. Each file is assigned a unique namespace, and individual queries (SELECT, INSERT, UPDATE, DELETE) are defined with a unique ID. They use `<resultMap>` to define the mapping between database table columns and the properties of a Java object.

The core of the interaction in the Java code is handled by a few key objects:
1.  A `SqlSessionFactory` is created using a `SqlSessionFactoryBuilder` that reads the `SqlMapConfig.xml` file.
2.  This factory is then used to open a `SqlSession`, which is the primary interface for executing queries.
3.  The `SqlSession` object provides methods to execute the SQL statements defined in the mapper files.

## 3. Implementing CRUD Operations
MyBatis provides straightforward methods on the `SqlSession` object for performing Create, Read, Update, and Delete (CRUD) operations. Each method corresponds to the type of SQL statement being executed:
- **Read**: `selectList()` is used for queries that return multiple records (as a `List`), while `selectOne()` is used for queries expected to return a single record or value.
- **Create**: `insert()` is called to execute an `<insert>` statement. It takes the ID of the query and a parameter object (like a `MemberVO` or a `HashMap`) containing the data to be inserted.
- **Update**: `update()` is used to execute an `<update>` statement, typically passing a parameter object with the new data and the key to identify the record to be updated.
- **Delete**: `delete()` executes a `<delete>` statement, usually taking a key (like an ID) as a parameter to specify which record to remove.

For any data modification operation (insert, update, delete), it is essential to explicitly call `session.commit()` to save the changes to the database before closing the session. Parameters are passed into SQL statements from Java code using the `#{propertyName}` syntax.

## 4. Using Dynamic SQL
A powerful feature of MyBatis is its ability to build SQL queries dynamically based on conditions. This is primarily used to create flexible `WHERE` clauses and is handled with special XML tags inside the mapper files:
- **`<if>`**: This tag conditionally includes a part of the SQL query. For example, in a search function, a condition like `AND name = #{name}` can be included only if the `name` parameter is not null or empty.
- **`<choose>`, `<when>`, `<otherwise>`**: This structure works like a Java `switch` statement, allowing only one of a set of conditions to be included in the SQL query.
- **`<foreach>`**: This tag is used to iterate over a collection (like a list of IDs) and is commonly used to build SQL `IN` clauses dynamically.
- **`<where>`**: This tag smartly handles the inclusion of the `WHERE` keyword and can automatically remove leading `AND` or `OR` conjunctions that might be left over by the `<if>` conditions, preventing syntax errors.
