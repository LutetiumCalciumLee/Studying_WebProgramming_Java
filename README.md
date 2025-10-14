# Servlet & JSP

## 1. What Is a Servlet?  
A Java-based **dynamic web component** implemented as a `.java` class.  
Runs inside a web application server (WAS) to handle HTTP requests and responses.  
Lifecycle:  
- `init()` → `service()` → `destroy()`

## 2. JSP (Jakarta Server Pages)  
A view technology that embeds Java code within HTML.  
At compile time, JSP → Servlet → `.class`, then executed by the servlet engine.

## 3. Servlet vs. JSP  

| Criterion       | Servlet                                  | JSP                                           |
|-----------------|------------------------------------------|-----------------------------------------------|
| Primary Role    | Controller / Model                       | View                                          |
| Coding Style    | Java code with embedded HTML             | HTML with embedded Java or custom tags (JSTL) |
| Strengths       | Performance, portability, easy MVC use   | Easy UI authoring, maintainability            |
| Weaknesses      | Cumbersome for page layout               | Unsuitable for heavy logic                    |

## 4. MVC (Model-View-Controller) Pattern  
- **Controller (Servlet)**: Routes requests and controls flow  
- **Model (DAO / Service / VO)**: Business logic and database access  
- **View (JSP / JSTL / EL)**: Renders the response  
Layered separation supports maintainability and team collaboration.

## 5. Servlet API Class Hierarchy  
```
Servlet (interface)
 └─ GenericServlet (abstract)
     └─ HttpServlet
```
Key methods in `HttpServlet`: `doGet()`, `doPost()`, `doPut()`, etc.

## 6. Key Features  
- **Servlet Mapping**: `@WebServlet("/*")` or `` in `web.xml`  
- **Filters**: Pre/post processing for encoding, authentication, logging  
- **Listeners**: Detect lifecycle events for context, session, request  
- **Session & Cookies**: Maintain user state for login, carts, etc.  
- **JSTL & EL**: Removes scriptlets from JSP for conditionals, loops, i18n, formatting  

## 7. Model2 (Modern MVC) Board Example Flow  
```
Browser → Controller (/board/list.do)
        → Service → DAO (SQL) → DB
        ← View (JSP) ← Model (List)
```
Business operations (create, update, delete, replies, paging) are encapsulated at the service layer.

## 8. Servlet Lifecycle Overview  
```
Class load → Instance creation
      ↓
   init()     (once)
      ↓
   service()  (per request)
      ↓
   destroy()  (on unload)
```
