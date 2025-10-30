# Chapter 28. Spring Web Application Features

## 1. File Upload & Download
A feature for handling multi-part file uploads and serving files for download in a web application.

**Uploads (using `CommonsMultipartResolver`)**
- **Dependencies**: Add `commons-fileupload` and `commons-io` to `pom.xml`.
- **Configuration**: Define a `CommonsMultipartResolver` bean in `servlet-context.xml` to set properties like `maxUploadSize` and `defaultEncoding`.
- **Controller**: Handles `MultipartHttpServletRequest` to process and save uploaded files.
- **View (JSP)**: The HTML form must use `enctype="multipart/form-data"` to enable file submission.

**Downloads**
- A **Controller** is implemented to read a specified file from the server's file system.
- It writes the file's content to the `HttpServletResponse` output stream, triggering a browser download.
