# Chapter 26: Using Spring Annotations
***
## 1. Introduction to Spring Annotations
This chapter introduces how to configure a Spring application using Java annotations as a replacement for traditional XML-based setups. By using annotations, bean configurations can be handled directly within Java code, which offers advantages in maintainability over managing XML files, especially as application functionality becomes more complex. The core objective is to utilize the `<context:component-scan>` tag to scan classes within a specific package and have the Spring container automatically register classes with stereotype annotations—such as `@Controller`, `@Service`, `@Repository`, and `@Component`—as beans. This approach reduces configuration volume and improves readability.

## 2. Configuration and Core Annotations
To implement an annotation-driven MVC, you must first enable the components that process annotations in the XML configuration file. Afterward, various annotations are used in Java code to handle URL requests and pass data.

*   **`action-servlet.xml`**: This configuration file is where core beans for handling annotation-based requests are registered.
    *   `DefaultAnnotationHandlerMapping`: Processes `@RequestMapping` annotations applied at the class level.
    *   `AnnotationMethodHandlerAdapter`: Processes `@RequestMapping` annotations applied at the method level.
    *   `<context:component-scan>`: Scans the package specified in the `base-package` attribute and its sub-packages to automatically create beans from classes with stereotype annotations.
*   **Request Handling Annotations**: These are the primary annotations used in a controller to receive URL requests and process parameters.
    *   `@RequestMapping`: Applied to classes or methods to map them to specific URL patterns and HTTP request methods (GET, POST).
    *   `@RequestParam`: Directly binds HTTP request parameters to a method's arguments, making the code more concise than using `request.getParameter()`. It can also receive all parameters at once in a `Map<String, String>` format.
    *   `@ModelAttribute`: Automatically binds request parameters to the properties of a command object (VO). Assigning a name to this annotation allows direct access to the object in the JSP using that name, eliminating the need for `ModelAndView.addObject()`.
*   **`Model` Class**: Instead of using a `ModelAndView` object, data can be passed to the view (JSP) using the `addAttribute()` method of a `Model` object. This approach is useful when the controller method only needs to return the view name as a string.

## 3. Implementing the MVC Architecture with Annotations
Annotations allow for a concise implementation of the MVC architecture by defining components and injecting dependencies without XML configuration. The `@Autowired` annotation plays a key role in this process.

*   **Controller Layer (`MemberControllerImpl.java`)**: The `@Controller` annotation is applied to the class to register it as a bean with a specified ID. `@RequestMapping` is used to map a URL to a specific method. Instead of declaring dependencies with a `<property>` tag in XML, the `@Autowired` annotation is added to a field to automatically inject a bean of the required type, such as a service object.
*   **Service Layer (`MemberServiceImpl.java`)**: The `@Service` annotation marks the class as a service bean responsible for business logic. The `@Transactional` annotation can be used alongside it to apply declarative transactions to the methods of the class. Additionally, `@Autowired` automatically injects the `MemberDAO` bean.
*   **DAO Layer (`MemberDAOImpl.java`)**: The `@Repository` annotation designates the class as a Data Access Object (DAO) bean. This annotation also enables exception translation. The injection of `SqlSessionTemplate` from XML is replaced by automatically injecting an `SqlSession` object into a field using `@Autowired`.
*   **VO Layer (`MemberVO.java`)**: The `@Component` annotation can be used to register the class as a generic bean. Other components, like a controller, can then inject this VO object using `@Autowired`.

## 4. Dependency Injection with `@Autowired`
The `@Autowired` annotation is a powerful feature that instructs the Spring container to automatically inject dependencies. Using this feature eliminates the need to manually configure dependency relationships with `<bean>` and `<property>` tags in XML files, making the configuration significantly more concise.

*   **How it Works**: `@Autowired` resolves dependencies based on type. For example, if `@Autowired` is placed above the `private MemberService memberService;` field in the `MemberControllerImpl` class, the Spring container finds and automatically injects the `MemberServiceImpl` bean registered with `@Service("memberService")`. Similarly, the service layer injects the DAO, and the DAO layer injects the `SqlSession` bean in the same manner.
*   **Benefits**: Developer-created classes (`Controller`, `Service`, `DAO`, `VO`) are registered as beans using their respective stereotype annotations, and their dependencies are resolved via `@Autowired`. As a result, configuration files like `action-mybatis.xml` are left with only bean definitions for framework-provided classes, such as `DataSource` or `SqlSessionFactoryBean`. This significantly reduces the developer's configuration overhead and promotes a more code-centric development style.
