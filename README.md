# Chapter 21: Spring MVC Features

## 1. Overview and architecture
- Spring MVC follows the Model 2 pattern with the central DispatcherServlet coordinating HandlerMapping, Controller, ModelAndView, ViewResolver, and View to process requests and render responses.
- It integrates smoothly with Tiles/SiteMesh and tag libraries, making message output, themes, and form handling easier across modules.

## 2. Request handling flow
- The browser sends a URL to DispatcherServlet, which asks HandlerMapping for a mapped Controller, invokes it, and receives a ModelAndView containing the logical view name and model data.
- DispatcherServlet delegates to ViewResolver to pick the View, the View renders the output, and the final response is returned to the client.

## 3. Practice 1: SimpleUrlController
- Setup: Map *.do to DispatcherServlet in web.xml; define beans in action-servlet.xml with SimpleUrlHandlerMapping mapping /test/index.do to a controller bean.
- Controller: Implements Controller and returns ModelAndView("index.jsp"); the JSP in /WEB-INF/test/index.jsp is rendered when calling /pro21/test/index.do.

## 4. Practice 2: MultiActionController with PropertiesMethodNameResolver
- View resolution: Configure InternalResourceViewResolver (prefix=/test/, suffix=.jsp) so logical names map to JSPs under /test.
- Routing: Map /test/*.do to UserController and use PropertiesMethodNameResolver so /test/login.do calls login(), which reads parameters, binds them to the model, and returns a logical view (e.g., result).

## 5. Practice 3: Displaying member info
- Controller: Add memberInfo() to bind id, pwd, name, email to the ModelAndView and set view to memberInfo.
- Views: memberForm.jsp posts to /test/memberInfo.do; memberInfo.jsp displays the submitted fields in a table.

## 6. Mapping summary
- Clear URL-to-method mapping improves readability: /test/login.do → login(), /test/memberInfo.do → memberInfo().
- Consolidating multiple actions in one controller via MultiActionController keeps configuration concise while keeping method-level clarity.

## 7. Practice 4: Deriving view name from URL
- Goal: Remove hardcoded view names by extracting the logical view name from the request URI (strip context path and .do), e.g., /test/login.do → /login.
- Implementation: A helper getViewName parses request URI, handling parameters and path info; the controller sets ModelAndView to this derived name so login.jsp is resolved automatically.
