# Servlet Request

## 1. Overview of Web Applications

* Web applications operate based on a **request/response model between client (browser) and server**.
* Static HTML pages alone cannot provide interactive user experiences, so **dynamic web technologies (Servlet, JSP)** are needed.
* Servlet is a Java-based server-side program responsible for dynamic processing in web applications.

---

## 2. Why Servlet is Needed

* Used to generate dynamic web pages in response to client requests.
* Examples:

  * Login processing
  * Writing and listing posts on a bulletin board

---

## 3. Characteristics of Servlet

* **Java-based server-side technology**.
* Preceded JSP and forms the foundation of JSP, Spring, and other frameworks.
* Excellent platform independence, security, and reusability.
* Clients do not call Servlets directly; the flow is **Browser → Web Server → Servlet Container → Servlet**.

---

## 4. Servlet Workflow

1. Client sends a request via browser
2. Web server receives the request and passes it to the **Servlet Container**
3. Servlet Container executes the appropriate **Servlet object**
4. Servlet processes the request and generates a **response**
5. Response (HTML, JSON, etc.) is returned to the browser

---

## 5. Role of Servlet Container

* Manages the **lifecycle** of Servlets:

  * Object creation
  * Initialization (`init`)
  * Request handling (`service`)
  * Cleanup (`destroy`)
* Supports **multi-threading** to efficiently handle multiple requests simultaneously.

---

## 6. Servlet Class Structure

```plaintext
Servlet (Interface)
  ↳ GenericServlet (Abstract)
    ↳ HttpServlet (Class)
```

* **Servlet**: Defines core methods (`init`, `service`, `destroy`)
* **GenericServlet**: Protocol-independent abstract class
* **HttpServlet**: HTTP-specific class, most commonly used

---

## 7. Servlet Lifecycle

1. `init()` – executed once at the start
2. `service()` – executed on every request (calls HTTP method-specific handlers like `doGet()`, `doPost()`)
3. `destroy()` – executed once when the Servlet is destroyed

---

## 8. Servlet Mapping Methods

### 1) `web.xml` Configuration

```xml
<servlet>
    <servlet-name>ExampleServlet</servlet-name>
    <servlet-class>com.test.ExampleServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ExampleServlet</servlet-name>
    <url-pattern>/example</url-pattern>
</servlet-mapping>
```

### 2) Annotation-based Configuration

```java
@WebServlet("/example")
public class ExampleServlet extends HttpServlet {
    ...
}
```

* Duplicate URL mappings cause errors during server startup
* Annotation method is simpler and easier to maintain

---

## 9. Managing Multiple Servlets

* Implement Servlets separately by function (e.g., bulletin board, login, product)
* Each Servlet name (`servlet-name`) and URL pattern must be **unique**
* Tomcat restart is required after configuration changes

