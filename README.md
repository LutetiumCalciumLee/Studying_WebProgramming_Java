# Chapter 19: Spring Dependency Injection and Inversion of Control
---
## 1 Applying Dependency Injection

### Dependency Injection
- Instead of developers assigning relationships between components (classes) directly through code, the container defines the relationships.
- Since direct relationships do not occur in code, each class can be modified more freely (loosely coupled).

### Tight Coupling vs. Loose Coupling
- In real life, if a car’s air conditioner breaks, naturally only the AC is repaired or replaced. But if the AC function were designed to be related to the car engine, even a minor AC issue would require touching the engine, which would be a problem. In other words, good cars strongly couple parts with the same function and ensure that unrelated functions do not affect each other.
- Programs are similar. They consist of independent features. In an online store, for example, there are product management, order management, member management, and bulletin board management. Each function consists of several classes that perform detailed features.
- If a class functioning as a component changes and classes unrelated to its function also need to be modified, various issues can arise as in the car example.
- Therefore, related functions should be tightly coupled, and unrelated functions should be loosely coupled for good programs. The opposite should be avoided.

### 1.1 Bulletin Board Feature Before Using DI

- BoardController creates and uses BoardService directly in code.
- BoardService creates BoardDAO directly in code and connects to the database.
- BoardDAO obtains a DataSource through JNDI and connects to Oracle.

### Problems with Plain Java Implementation
- Currently, the BoardDAO class implements the bulletin board feature connected to Oracle.
- If the database changes from Oracle to MySQL, the BoardDAO class must be modified in detail.
- Furthermore, the BoardService class that uses BoardDAO might also need changes.
- Therefore, directly creating and using objects in Java code (tightly coupled) can cause complex issues.
- If changes in one class continuously affect other parts, directly creating objects in Java code is not a good approach.

### 1.2 Applying Interfaces to the Bulletin Board Feature

- Oracle-based class hierarchy:
  - BoardController → BoardService → BoardDAO
  - BoardControllerImpl → BoardServiceImpl → BoardOracleDAOImpl

- In BoardServiceImpl, an Oracle-specific implementation is instantiated to connect to Oracle.

- MySQL-based class hierarchy:
  - BoardDAO → BoardOracleDAOImpl / BoardMySqlDAOImpl

- In BoardServiceImpl, switch to BoardMySqlDAOImpl to connect to MySQL.

- During development, when adding MySQL integration, there is no need to modify the existing BoardOracleDAOImpl; simply implement another BoardMySqlDAOImpl that implements the BoardDAO interface and use it in BoardServiceImpl.
- However, even with interfaces, BoardServiceImpl itself still needs direct source code modifications.

### 1.3 Applying Dependency Injection to the Bulletin Board Feature

- Advantages of DI:
  - Minimizes dependency between classes to simplify code.
  - Makes the application easier to maintain and manage.
  - In traditional implementation, developers directly controlled object creation and destruction inside code, but with DI, the container controls object creation, destruction, and dependencies.

- Inversion of Control (IoC):
  - In traditional code, the developer controlled objects directly, but with Spring, Spring itself manages object control.
  - There are various types of IoC; generally in Spring, IoC is implemented via DI, so the term DI is used more often than IoC.

- Spring DI Methods:
  1. Constructor-based injection
  2. Setter-based injection

- External configuration injects dependent objects:
  - The container injects a BoardDAOImpl object into BoardService using either constructor or setter.

- Example:
  - Constructor injection: BoardServiceImpl(BoardDAO boardDAO)
  - Setter injection: setBoardDAO(BoardDAO boardDAO)
---
## 2 Practicing Dependency Injection

### 2.1 DI Using Setter

- Create project pro19 and add Spring DI-related libraries to the lib folder.
- Configure Build Path to include JARs (cglib, commons-logging, commons-dbcp, commons-pool, spring.jar).
- Create person.xml for bean configuration.

- Bean tag attributes:
  - id: Unique bean name used for access.
  - name: Alias of the object.
  - class: Class to instantiate, including the package name.
  - constructor-arg: Used when injecting values via constructor.
  - property: Used when injecting values via setter.
  - lazy-init:
    - Enables creating the bean upon first request instead of at Tomcat startup.
    - Accepts true, false, default.
    - If not set or set to false, the bean is created at Tomcat startup.
    - If set to true, the bean is created when used.

- person.xml example (setter injection):
  - Create PersonServiceImpl bean with id="personService".
  - Initialize the name property to "홍길동" using <value>.

- Implement classes:
  - PersonService interface declares sayHello().
  - PersonServiceImpl has private fields name and age; setName(String name) setter is provided; sayHello() prints name and age.

- Test:
  - PersonTest loads person.xml via XmlBeanFactory, retrieves the bean by id "personService", and calls sayHello().
  - Console output shows the name set via <value>, but age prints 0 since it was not injected.

### 2.2 DI Using Constructor

- Create package com.spring.ex02 and classes PersonService, PersonServiceImpl, PersonTest2.
- person.xml example (constructor injection):
  - Bean personService1: pass "이순신" to initialize name.
  - Bean personService2: pass "손흥민" and "23" to initialize name and age.

- PersonServiceImpl implements two constructors:
  - PersonServiceImpl(String name) used when one argument is configured.
  - PersonServiceImpl(String name, int age) used when two arguments are configured.

- Test:
  - PersonTest2 loads person.xml, retrieves personService1 and personService2, calls sayHello().
  - Console output:
    - name: 이순신, age: 0
    - name: 손흥민, age: 23
---
## 3 Practicing DI Using Member Feature

- Class hierarchy:
  - MemberService ↔ MemberDAO (dependency)
  - MemberServiceImpl ↔ MemberDAOImpl

- Create member.xml in the same project.

- member.xml configuration:
  - Create two beans simultaneously.
  - Bean id="memberService" class="com.spring.ex03.MemberServiceImpl":
    - Inject bean id="memberDAO" into property memberDAO using ref.
  - Bean id="memberDAO" class="com.spring.ex03.MemberDAOImpl".

- Implement MemberServiceImpl:
  - Declare private MemberDAO memberDAO.
  - Provide setter setMemberDAO(MemberDAO memberDAO).
  - listMembers() calls memberDAO.listMembers().

- Implement MemberDAOImpl:
  - listMembers() prints test messages indicating member info retrieval.

- Test (MemberTest1):
  - Load member.xml via XmlBeanFactory and create/inject beans as configured.
  - Retrieve bean "memberService" and call listMembers().
  - Console confirms MemberDAO’s listMembers() was called.

## Additional Study: lazy-init Practice

- Project structure includes com.spring.ex04 and lazy.xml.

- lazy.xml:
  - firstBean: class com.spring.ex04.First, lazy-init="false"
  - secondBean: class com.spring.ex04.Second, lazy-init="true"
  - thirdBean: class com.spring.ex04.Third, lazy-init="default"

- Classes:
  - First: constructor prints "First constructor called"
  - Second: constructor prints "Second constructor called"
  - Third: constructor prints "Third constructor called"

- Execution class LazyTest:
  - Load ApplicationContext with lazy.xml.
  - Print "Getting SecondBean"
  - Call context.getBean("secondBean")

- Result:
  - First and Third beans are created at application startup.
  - Second bean is created when context.getBean() is called.
