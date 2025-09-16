# Chapter 24: Integrating Spring and MyBatis
***
## 1. Introduction to Spring and MyBatis Integration
This chapter demonstrates how to integrate the MyBatis persistence framework with the Spring framework. The key objective is to leverage Spring's powerful dependency injection and declarative transaction management to simplify and manage the MyBatis data access layer. Instead of manually creating and managing `SqlSessionFactory` and `SqlSession` objects, the Spring container is configured to handle their lifecycle. This allows developers to focus on business logic while the frameworks manage the underlying resources, resulting in cleaner, more maintainable code.

## 2. Configuration and Core Components
The integration is primarily achieved through a series of XML configuration files that define how Spring and MyBatis will work together.

*   **`web.xml`**: This file is the entry point for the web application. It is configured to use Spring's `ContextLoaderListener`, which initializes the Spring application context and loads the configuration files for the service and data access layers (`action-service.xml`, `action-mybatis.xml`).
*   **`action-servlet.xml`**: This is the Spring MVC configuration file. It sets up the `InternalResourceViewResolver` to locate JSP files and defines the controller beans (`MemberController`). It also maps URL patterns (e.g., `/member/*.do`) to the appropriate controller and its methods using `SimpleUrlHandlerMapping` and `PropertiesMethodNameResolver`.
*   **`action-mybatis.xml`**: This is the central file for the Spring-MyBatis integration. It defines the core beans required for database interaction:
    *   A `DataSource` is configured using connection details from an external `jdbc.properties` file.
    *   `SqlSessionFactoryBean` is used to create a MyBatis `SqlSessionFactory`. It is injected with the `DataSource` and is configured with the locations of the MyBatis configuration file (`modelConfig.xml`) and the mapper XML files (`mappers/*.xml`).
    *   `SqlSessionTemplate` is a Spring-managed, thread-safe implementation of the MyBatis `SqlSession`. It is created using the `SqlSessionFactory` and is the bean that gets injected into the DAO layer to execute SQL queries.
*   **Mapper and Model Configuration**:
    *   `modelConfig.xml` is a standard MyBatis configuration file used here to define type aliases (e.g., aliasing `com.spring.member.vo.MemberVO` to `memberVO`) for cleaner syntax in mapper files.
    *   `member.xml` is the MyBatis mapper file containing the actual SQL queries for CRUD operations (e.g., `selectAllMemberList`, `insertMember`) with their unique IDs.

## 3. Implementing the MVC Architecture
The application is structured following the Model-View-Controller (MVC) pattern, with distinct layers for data access, business logic, and presentation. Spring's dependency injection is used to wire these components together.

*   **DAO Layer (`MemberDAOImpl.java`)**: This class is responsible for direct database interaction. It contains a `sqlSession` property and a setter method, allowing Spring to inject the `SqlSessionTemplate` bean. Methods in the DAO (e.g., `selectAllMemberList()`, `insertMember()`) use the injected `sqlSession` object to execute the corresponding SQL statements defined in the `member.xml` mapper by referencing their namespace and ID (e.g., `mapper.member.selectAllMemberList`).
*   **Service Layer (`MemberServiceImpl.java`)**: This class implements the business logic. It has a `memberDAO` property that is injected by Spring. The service methods (e.g., `listMembers()`, `addMember()`) call the corresponding methods in the DAO to perform data operations, acting as a bridge between the controller and the data layer.
*   **Controller Layer (`MemberControllerImpl.java`)**: This class handles incoming web requests. It extends Spring's `MultiActionController` to manage multiple actions within a single class. It has a `memberService` property injected by Spring. Handler methods like `listMembers()` and `addMember()` call the service layer to process requests, and then create and return a `ModelAndView` object. This object contains the data to be displayed and the logical name of the view (JSP file) that will render the response.

## 4. View Layer and Request Handling
The user interface is built with JSP files, and the flow of the application is controlled by URL requests handled by the controller.

*   **JSP Files**:
    *   `listMembers.jsp`: Displays a table of member data passed from the controller. It uses JSTL tags to iterate over the list of members. It includes links for actions like "Delete" and "Sign Up," which point to specific controller URLs.
    *   `memberForm.jsp`: Provides a form for users to input new member information. Submitting the form sends a POST request to the `/member/addMember.do` URL.
*   **Request Flow**: When a user clicks a link or submits a form, a request is sent to a specific URL (e.g., `/member/removeMember.do?id=djkim`). The `SimpleUrlHandlerMapping` directs this request to the `memberController`. The `PropertiesMethodNameResolver` then maps the URL to the corresponding method in the controller (`removeMember`). The controller method processes the request, interacts with the service layer, and finally redirects the user back to the member list page to show the updated data.

