# Chapter 14: Expression Language and JSTL Summary

---

## 14.1 What is Expression Language (EL)?
- **Definition**: A data output feature introduced in JSP 2.0 that allows convenient use of Java code expressions
- **Format**: `${expression or value}`
- **Features**:
  - More convenient value output than traditional expressions
  - Can include variables and operators
  - Can output JSP built-in object attributes and JavaBean properties
  - Requires `isELIgnored = false` setting in page directive

### Expression Language Operators
- **Arithmetic operators**: +, -, *, /, div, %, mod
- **Comparison operators**: ==, eq, !=, ne, , gt, =, ge
- **Logical operators**: &&, and, ||, or, !, not
- **Empty operator**: Checks for null or empty string
- **Conditional operator**: `${condition ? value1 : value2}`

---

## 14.2 Expression Language Built-in Objects
### Main Built-in Objects
- **Scope objects**: pageScope, requestScope, sessionScope, applicationScope
- **Request parameters**: param, paramValues
- **Header values**: header, headerValues
- **Cookie values**: cookie
- **JSP content**: pageContext
- **Initial parameters**: initParam

### Scope Priority
page > request > session > application

---

## 14.3 Binding Attribute Output
- Expression language allows direct output of attribute values bound to each built-in object without Java code
- Collection objects, HashMap, and has-a relationship beans are also accessible

---

## 14.4 Custom Tags
- **Definition**: A feature that replaces Java code such as conditional statements and loops with tags
- **Types**: 
  - JSTL (JSP Standard Tag Library)
  - Developer-defined custom tags

---

## 14.5 JSTL (JSP Standard Tag Library)
### JSTL Library Types
| Library | Prefix | URI | Function |
|---------|--------|-----|----------|
| Core | c | http://java.sun.com/jsp/jstl/core | Variables, control flow, URL handling |
| Internationalization | fmt | http://java.sun.com/jsp/jstl/fmt | Locale, messages, date formatting |
| XML | x | http://java.sun.com/jsp/jstl/xml | XML processing |
| Database | sql | http://java.sun.com/jsp/jstl/sql | Database processing |
| Functions | fn | http://java.sun.com/jsp/jstl/functions | String processing |

---

## 14.6 Core Tag Library
### Main Core Tags
- **``**: Variable declaration and value assignment
- **``**: Variable removal
- **``**: Conditional statement processing
- **``**: Multiple condition processing similar to switch statement
- **``**: Loop processing
- **``**: URL generation
- **``**: Redirect processing
- **``**: Value output

### `` Tag varStatus Attributes
- **index**: Current index (starts from 0)
- **count**: Iteration count (starts from 1)
- **first**: Whether it's the first iteration
- **last**: Whether it's the last iteration

---

## 14.7 Core Tag Practice Examples
- Login processing example
- Grade converter example
- Multiplication table output example
- Image list output example

---

## 14.8 Internationalization Tag Library
- **``**: Setting locale (language)
- **``**: Displaying internationalized messages
- **``**: Bundle specification

---

## 14.9 Converting Korean to ASCII Code
- Unicode conversion through Properties Editor installation
- Multi-language support through .properties files

---

## 14.10 Formatting Tag Library
### Main Formatting Tags
- **``**: Number format specification
- **``**: Date format specification
- **``**: Time zone setting

---

## 14.11 String Processing Functions
### Main String Functions
- **fn:length()**: String length
- **fn:contains()**: String containment check
- **fn:indexOf()**: String position
- **fn:replace()**: String replacement
- **fn:substring()**: Substring extraction
- **fn:toUpperCase()/toLowerCase()**: Upper/lowercase conversion

---

## 14.12 Member Management Using Expression Language and JSTL
- Implementation of member management system using only Expression Language and JSTL without Java code
- Member information CRUD processing integrated with MemberBean and MemberDAO

---
