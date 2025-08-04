# Servlet Concept

## 1. What is a Servlet?

* A **Servlet** is a **Java-based server-side program (class)** that generates **dynamic web pages** in web applications.
* Before JSP, it was the main approach for implementing dynamic web functions. Even today, it remains the foundation for JSP, Spring, and other frameworks.
* **High platform independence, reusability, and security features**.
* Runs only on the server side.
* Common container: **Tomcat** (provides an environment for JSP and Servlets).

---

## 2. Web Application Flow & Servlet Container

1. The **client (browser)** sends a request.
2. The **web server** receives the request and forwards it to the **Servlet container**.
3. The **Servlet class executes** to process the request (DB interaction, business logic, etc.).
4. The result (HTML, JSON, etc.) is returned as a **response** to the browser.

> The Servlet container manages the **creation, execution, and destruction** of Servlets and handles **multithreading**.

---

## 3. Servlet Class Hierarchy

Servlets inherit classes and interfaces in the following structure:

```
Servlet (Interface)
  ↳ GenericServlet (Abstract class)
    ↳ HttpServlet (Class)
```

* **Servlet interface**: defines core lifecycle methods like `init()`, `service()`, and `destroy()`.
* **GenericServlet**: implements `Servlet` interface, can be used for various protocols.
* **HttpServlet**: specialized for HTTP protocol; most commonly used in practice.

---

## 4. Servlet Lifecycle Methods

Servlets are called in the following order by the container:

1. **init()**: called once when the Servlet is first loaded into memory.
2. **service()**: called for each request → internally dispatches to **doGet(), doPost()**, etc., based on HTTP method.
3. **destroy()**: called once when the container shuts down or the Servlet is removed.

> **doXXX methods**:
>
> * `doGet()`, `doPost()`, `doPut()`, `doDelete()`, `doHead()`, `doOptions()`
> * Each corresponds to an HTTP request method.

---

## 5. Servlet Mapping

### 1) `web.xml`-based mapping

* Use `<servlet>` and `<servlet-mapping>` tags to register mapping information:

```xml
<servlet>
    <servlet-name>FirstServlet</servlet-name>
    <servlet-class>com.example.FirstServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>FirstServlet</servlet-name>
    <url-pattern>/first</url-pattern>
</servlet-mapping>
```

* `servlet-name` and `url-pattern` must match exactly.
* Typos or mismatches → **404 error**.

### 2) Annotation-based mapping

* Add `@WebServlet("/url")` annotation at the top of the class:

```java
@WebServlet("/third")
public class ThirdServlet extends HttpServlet { ... }
```

* The URL pattern must be **unique**. If it duplicates existing `web.xml` mappings, the server will fail to start.
* Easier to configure and maintain.

---

## 6. Managing Multiple Servlets

* Real-world applications (e.g., e-commerce) have multiple functions (product, order, board, etc.) → **create a separate Servlet for each function**.
* When managing multiple Servlets:

  * Each `<servlet>` and `<servlet-mapping>` must be managed separately.
  * `servlet-name` values must be **unique**.
  * Restart Tomcat to apply changes.

> On first request, the Servlet is loaded into memory (`init` called).
> Subsequent requests reuse the loaded instance and only call the appropriate request method (e.g., `doGet()`) → **thread-based operation** for performance optimization.

---

## 7. Hands-on Implementation Steps

1. Create a **Dynamic Web Project** in Eclipse.
2. Add `servlet-api.jar` library (included with Tomcat).
3. Create a package and a class extending `HttpServlet`.
4. Override lifecycle methods (`init`, `doGet`/`doPost`, `destroy`) and add console outputs.
5. Configure mapping using `web.xml` or `@WebServlet`.
6. Run on Tomcat server → request URL and confirm console outputs.

---

## 8. Using Sample Code

* Import example source code provided with the book/course into **Eclipse**.
* Create a new workspace to avoid project name conflicts.
* Register the project on Tomcat and run.
* Compare your implementation with the example code for better understanding.

