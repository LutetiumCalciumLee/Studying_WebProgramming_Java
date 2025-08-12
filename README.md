# Using the Servlet Extension API

## 1. Servlet Basics

* A **Servlet** is a Java-based server-side program that generates **dynamic web content**.
* Runs only on the **server**, inside a **Servlet Container** (e.g., Tomcat).
* High **platform independence**, **reusability**, and **security**.
* Forms the basis of JSP, Spring, and other Java web frameworks.

---

## 2. Web Application Request Flow

1. **Client (Browser)** sends an HTTP request.
2. **Web Server** forwards it to the **Servlet Container**.
3. Container finds the matching servlet, calls `service()` method.
4. Servlet processes the request and returns a **response** to the client.
5. For dynamic pages, often interacts with a **database** before responding.

---

## 3. Servlet Lifecycle

1. **Loading & Instantiation** – Servlet class loaded into memory.
2. **Initialization (`init()`)** – Runs once when the servlet is first loaded.
3. **Request Handling (`service()`)** – Called for each client request.
4. **Destruction (`destroy()`)** – Runs once before the servlet is unloaded.

---

## 4. Request Dispatching

### `forward()`

* Transfers request **within the server** to another resource (JSP, HTML, or another servlet).
* The browser **URL does not change**.
* The same `request` and `response` objects are passed along.
* Suitable when you want to keep request data intact.

### `sendRedirect()`

* Instructs browser to send a **new request** to a different URL.
* **URL changes** in the browser.
* New request/response objects are created.
* Suitable for redirecting after form submissions or to an external site.

---

## 5. Servlet Context vs Servlet Config

### `ServletContext`

* **Application-wide scope** (shared across all servlets).
* Stores initialization parameters from `web.xml`.
* Used for shared data, caching, or application-level configuration.

### `ServletConfig`

* **Servlet-specific scope** (parameters for one servlet only).
* Stores initialization parameters from `web.xml` or annotations for that servlet.

---

## 6. Session & Request Attributes

* **Request Scope** – Data lasts for a single request/forward chain.
* **Session Scope** – Data lasts until the user session expires or is invalidated.
* **Application Scope** – Data shared for the lifetime of the application.

---

## 7. `loadOnStartup`

* Pre-loads and initializes a servlet **when the application starts**, not on the first request.
* **Improves first-request speed** for resource-heavy initialization.
* Smaller numbers = higher priority during startup.
* Can be set via:

  ```java
  @WebServlet(loadOnStartup=1)
  ```

  or in `web.xml`:

  ```xml
  <load-on-startup>1</load-on-startup>
  ```
* Common use: preloading menus, cache data, DB connections.

---

## 8. Best Practices

* Use `forward()` for internal navigation when preserving request data.
* Use `sendRedirect()` for new requests or external URLs.
* Store common data in `ServletContext`, servlet-specific configs in `ServletConfig`.
* Use `loadOnStartup` for servlets that need to prepare resources at startup.
* Always release resources in `destroy()` to avoid memory leaks.
