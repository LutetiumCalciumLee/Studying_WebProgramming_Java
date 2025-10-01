# Chapter 27: Using Maven and Spring STS
***
## 1. Introduction to Maven and Spring Tool Suite (STS)
This chapter provides a guide on using **Maven**, an open-source build automation tool, and **Spring Tool Suite (STS)**, an Eclipse-based IDE, for developing Spring applications. Maven simplifies project management by handling dependency libraries and maintaining a consistent project structure through its Project Object Model (POM) defined in the `pom.xml` file. STS offers a specialized environment for Spring development, which can be set up either by installing it as a plugin within an existing Eclipse installation or by downloading it directly from the Spring website. The chapter focuses on using these tools to streamline the setup and management of a Spring MVC project.

## 2. Environment Setup and Project Creation
The initial setup involves installing Maven and configuring system environment variables, followed by integrating STS into the Eclipse IDE.

*   **Maven Installation**:
    *   Download the Maven binary zip file from the official Apache Maven website.
    *   Extract the files and place the Maven folder in a designated directory (e.g., `C:\\spring`).
    *   Set the `MAVEN_HOME` environment variable to the Maven installation directory and add `%MAVEN_HOME%\\bin` to the system's `Path` variable to enable Maven commands globally.
*   **STS Installation**:
    *   In Eclipse, use the "Eclipse Marketplace" to search for "sts".
    *   Install the "Spring Tools 3 Add-On," which provides the necessary tooling for creating and managing Spring projects.
*   **Creating a Spring MVC Project**:
    *   Use the "Spring Legacy Project" wizard in STS to create a new project.
    *   Select the "Spring MVC Project" template, which requires a one-time download of the template files.
    *   Define a top-level package name (e.g., `com.myspring.pro27`), which also sets the context path for the web application.
    *   Upon creation, Maven automatically downloads the required dependencies specified in the `pom.xml` file and stores them in the local `.m2/repository` directory.

## 3. Project Structure and Configuration
A Maven-based Spring project in STS has a standardized structure and relies on several key configuration files to function correctly.

*   **Project Directory Structure**:
    *   `src/main/java`: Contains the application's Java source code, such as controllers and services.
    *   `src/main/resources`: Holds resource files like MyBatis mappers and `log4j.xml`.
    *   `src/main/webapp`: Contains web resources, including the `WEB-INF` directory where configuration files (`web.xml`, `servlet-context.xml`) and JSP views are located.
    *   `pom.xml`: The core Maven configuration file that defines project information, properties, and dependencies.
*   **Core Configuration Files**:
    *   **`pom.xml`**: Manages all project dependencies. Libraries for Spring MVC, MyBatis, data sources (like `commons-dbcp`), and logging (`log4j`) are added within the `<dependencies>` tag. For proprietary drivers like Oracle's ojdbc, a `system` scope can be used to reference a local JAR file.
    *   **`web.xml`**: Configures the root Spring container using `ContextLoaderListener` (loading `root-context.xml`) and the `DispatcherServlet` for handling web requests (loading `servlet-context.xml`).
    *   **`servlet-context.xml`**: Configures web-specific components. It enables annotation-driven controllers, sets up a `ViewResolver` (initially `InternalResourceViewResolver` for JSPs, later replaced for Tiles), and defines the base package for component scanning.

## 4. Integrating Libraries for a Full-Stack Application
The chapter details how to integrate essential libraries like MyBatis for data persistence, Log4j for logging, and Tiles for building consistent UI layouts.

*   **MyBatis Integration**:
    *   Add MyBatis and `mybatis-spring` dependencies to `pom.xml`.
    *   Configure a `DataSource` and `SqlSessionFactoryBean` in a configuration file like `action-mybatis.xml`. This file is loaded by `web.xml`.
    *   Create mapper XML files (e.g., `member.xml`) to define SQL queries and a `modelConfig.xml` for type aliases.
*   **Logging with Log4j and AOP**:
    *   STS automatically includes `log4j` and a basic `log4j.xml` file.
    *   Configure appenders (e.g., `ConsoleAppender`, `DailyRollingFileAppender`) and layouts (`PatternLayout`) to control log output format and destination.
    *   Set log levels (`INFO`, `DEBUG`, etc.) for different packages to manage log verbosity. Setting the root logger to `DEBUG` can also display MyBatis SQL queries and results in the console.
    *   Aspect-Oriented Programming (AOP) can be used for advanced logging by creating an advice class (e.g., `LoggingAdvice`) with `@Aspect` and applying it to service and DAO layers using pointcut expressions.
*   **Layout Management with Tiles**:
    *   Add Tiles dependencies (`tiles-core`, `tiles-jsp`, `tiles-servlet`) to `pom.xml`.
    *   In `servlet-context.xml`, replace the `InternalResourceViewResolver` with a `TilesConfigurer` bean and a `UrlBasedViewResolver` that uses `TilesView`.
    *   Create a Tiles definition XML file (e.g., `tiles_member.xml`) in `src/main/resources` to define reusable layouts (e.g., `baseLayout`) and specific page definitions that extend them.
    *   A main layout JSP (`layout.jsp`) uses `<tiles:insertAttribute>` tags to assemble the final page from different components like a header, side menu, body, and footer. This allows for a modular and maintainable user interface.
