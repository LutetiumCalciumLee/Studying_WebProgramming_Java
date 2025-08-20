# Chapter 18: Getting Started with Spring Framework

## 1 What is a Framework?

### Framework Definition
- **Dictionary meaning**: A structure or skeleton that constitutes something
- **Software meaning**: A semi-finished product that provides functions pre-made as classes or interfaces

### Framework Advantages
- **Development efficiency**: Enables development of applications with guaranteed productivity and quality through development based on consistent standards
- **Maintenance**: Ensures high quality in post-development maintenance and feature extensibility

## 1.1 Spring Framework

### Spring Framework Overview
- **Definition**: An open-source framework for Java web application development
- **Characteristics**: A lightweight framework that is lighter than EJB (Enterprise Java Beans)

### Container Concept
- **Tomcat**: Called a servlet container because when Tomcat runs, it has full authority over servlet creation, initialization, service execution, and destruction
- **Spring**: Spring has the authority to directly manage various beans (class objects) used in applications, not the developer

## Spring Framework Characteristics

### Core Features
- **Lightweight**: Lighter and easier to learn than EJB, performs lightweight container functions
- **Inversion of Control (IoC)**: Controls loose coupling between applications using IoC technology
- **Dependency Injection (DI)**: Supports dependency injection functionality
- **Aspect-Oriented Programming (AOP)**: Manages resources using AOP functionality
- **Persistence support**: Supports various services related to persistence
- **Library integration**: Supports integration with numerous libraries

### Key Architecture Components
- **POJO-based framework**
- **Lightweight container**
- **Dependency Injection (DI)**
- **Aspect-Oriented Programming (AOP)**
- **Inversion of Control (IoC)**

### Term Definitions
- **Dependency Injection**: A method where the framework creates and uses class objects instead of developers creating them in code
- **Inversion of Control**: A method where the framework directly performs servlets or beans instead of developers creating them in code
- **Aspect-Oriented**: A method that increases modularity by separating and implementing auxiliary functions from core functions

## Spring Framework Main Features

| Feature | Description |
|---------|-------------|
| **Core** | Provides IoC functionality to separate other features and configurations |
| **Context** | Provides access methods to beans that perform various functions in the application as Spring's basic functionality |
| **DAO** | Makes JDBC functionality more convenient to use |
| **ORM** | Provides functionality integrated with persistence-related frameworks like Hibernate or MyBatis |
| **AOP** | Provides aspect-oriented functionality |
| **Web** | Provides functionality needed for web application development |
| **WebMVC** | Provides functionality related to MVC implementation in Spring |

---

## 2 Spring Framework Environment Setup

### Project Setup
- Create a new project named **pro18**
- Copy Spring 3.0 library files from the provided example source to the **/WEB-INF/lib** folder

### Project Structure
```
pro18
├── Deployment Descriptor: pro18
├── JAX-WS Web Services
├── Java Resources
├── JavaScript Resources
├── build
└── WebContent
    ├── META-INF
    └── WEB-INF
        └── lib
            ├── cglib-nodep-2.1_3.jar
            ├── cglib-nodep-2.2.jar
            ├── com.springsource.javax.validation-1.0.0.GA.jar
            ├── com.springsource.org.aopalliance-1.0.0.jar
            ├── com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
            ├── commons-beanutils-1.8.3.jar
            ├── commons-dbcp-1.4.jar
            ├── commons-logging-1.1.1.jar
            ├── commons-pool-1.5.6.jar
            ├── json_simple-1.1.jar
            ├── jstl.jar
            ├── log4j-1.2.16.jar
            ├── mybatis-3.0.5.jar
            ├── mybatis-spring-1.0.1.jar
            ├── mysql-connector-java-3.0.14-production-bin.jar
            ├── ojdbc14.jar
            ├── org.springframework.aop-3.0.6.RELEASE.jar
            ├── org.springframework.asm-3.0.6.RELEASE.jar
            ├── org.springframework.aspects-3.0.6.RELEASE.jar
            ├── org.springframework.beans-3.0.6.RELEASE.jar
            └── org.springframework.context-3.0.6.RELEASE.jar
```

### Required Libraries
The setup includes essential JAR files for:
- **Spring Framework**: Core Spring functionality
- **Database connectivity**: MySQL and Oracle drivers
- **ORM integration**: MyBatis and Spring-MyBatis integration
- **Logging**: Log4j and Commons Logging
- **Utilities**: Commons libraries for various utilities
- **JSON processing**: JSON Simple library
- **Web functionality**: JSTL for JSP pages
