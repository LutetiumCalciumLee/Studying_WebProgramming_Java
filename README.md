# Chapter 10: Servlet Filter and Listener Features

## 1. Servlet Attributes and Scope Concepts
* **Servlet Attributes** are objects (information) bound and stored in ServletContext, HttpSession, and HttpServletRequest objects
* **Servlet Scope** defines the access range for attributes bound to servlet APIs
* Provides **data sharing mechanisms** across different parts of web applications
* Essential for **state management** in stateless HTTP protocol

***

## 2. Servlet Attribute Scope Types

### Application Scope (ServletContext)
* **Accessible throughout the entire application**
* Shared among all servlets and users
* Persists until application shutdown
* Used for global configuration, shared resources

### Session Scope (HttpSession)
* **Accessible only from the specific browser session**
* User-specific data storage
* Persists until session timeout or invalidation
* Used for login status, shopping cart data

### Request Scope (HttpServletRequest)
* **Accessible only within the request/response cycle**
* Data survives through forwards but not redirects
* Used for MVC data transfer between servlets and JSPs

***

## 3. Servlet Attribute Management Methods
```java
// Setting attributes
setAttribute(String name, Object value)

// Retrieving attributes
getAttribute(String name)

// Removing attributes
removeAttribute(String name)
```

***

## 4. URL Pattern Matching in Servlets

### Pattern Types & Priority Order
1. **Exact name match**: `/first/test` (Highest priority)
2. **Directory match**: `/first/*`
3. **Extension match**: `*.do`
4. **All requests**: `/*` (Lowest priority)

### Key URL Methods
* `getContextPath()` – Gets only the context name
* `getRequestURL()` – Gets the complete URL
* `getServletPath()` – Gets only the servlet mapping name
* `getRequestURI()` – Gets the URI

***

## 5. Filter API Functionality

### Filter Definition
* **Intercepts requests and responses** before reaching servlets
* Processes **common cross-cutting concerns** across multiple servlets
* Implements **chain of responsibility pattern** for request processing
* Provides **pre and post-processing** capabilities

### Filter Use Cases
**Request Filtering:**
* User authentication and authorization
* Request logging and auditing
* Character encoding setup
* Input validation and sanitization

**Response Filtering:**
* Response compression
* Response encryption
* Performance monitoring
* Output formatting

***

## 6. Filter Lifecycle Methods

### Core Filter Interface Methods
```java
// Initialization - called once when filter is created
init(FilterConfig config)

// Main processing - called for each request/response
doFilter(ServletRequest request, ServletResponse response, FilterChain chain)

// Cleanup - called once when filter is destroyed
destroy()
```

### Filter Chain Processing
```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
    // Pre-processing (request filter)
    request.setCharacterEncoding("utf-8");
    
    chain.doFilter(request, response); // Continue to next filter/servlet
    
    // Post-processing (response filter)
    // Cleanup or response modification
}
```

***

## 7. Filter Configuration Methods

### Annotation-based Configuration
```java
@WebFilter("/*")
public class EncodingFilter implements Filter { ... }
```

### XML-based Configuration
```xml

    encode
    sec03.ex01.EncoderFilter


    encode
    /*

```

***

## 8. Servlet Listener API Types

### Context Listeners
* **ServletContextListener** – Application startup/shutdown events
* **ServletContextAttributeListener** – Application-scope attribute changes

### Session Listeners
* **HttpSessionListener** – Session creation/destruction events
* **HttpSessionAttributeListener** – Session attribute changes
* **HttpSessionBindingListener** – Object binding/unbinding to sessions
* **HttpSessionActivationListener** – Session activation/passivation events

### Request Listeners
* **ServletRequestListener** – Request initialization/destruction events
* **ServletRequestAttributeListener** – Request attribute changes

***

## 9. Listener Registration Methods

### Annotation-based Registration
```java
@WebListener
public class SessionCounterListener implements HttpSessionListener { ... }
```

### XML-based Registration
```xml

    com.example.SessionCounterListener

```

***

## 10. Common Listener Implementation Patterns

### Session Management Example
* **Track active user sessions** using HttpSessionListener
* **Monitor login attempts** with session attribute listeners
* **Implement concurrent login restrictions** per user ID
* **Manage resource cleanup** when sessions expire

### Application Monitoring
* **Initialize shared resources** at application startup
* **Track application-wide statistics** and performance metrics
* **Handle graceful shutdown** and resource cleanup
* **Monitor attribute changes** for debugging and auditing

---
