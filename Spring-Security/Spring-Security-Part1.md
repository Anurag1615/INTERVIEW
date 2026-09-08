# Spring Boot Security Architecture

## Overview
Spring Security protects applications from attacks like:

- CSRF (Cross-Site Request Forgery)
- XSS (Cross-Site Scripting)
- SQL Injection

It provides two main security mechanisms:

1. **Authentication** – Verifying who the user is.
2. **Authorization** – Verifying what the user is allowed to do.

---

# Authentication vs Authorization

## Authentication
Authentication verifies the **identity of the user**.

Example:

User provides:

- Username
- Password

System verifies if the credentials are correct.


User -> Enter Username + Password -> System verifies identity


Meaning:


WHO ARE YOU?


---

## Authorization

Authorization verifies **what actions the user can perform**.

Example:


User authenticated
|
|---- Allowed -> Read Data
|
|---- Not Allowed -> Update/Delete Data


Meaning:


WHAT ARE YOU ALLOWED TO DO?


---

# Spring Boot Request Flow (Without Security)


Client Request
|
v
Servlet Container
|
v
Filter Chain
(Filter1 -> Filter2 -> Filter3 -> FilterN)
|
v
DispatcherServlet
|
v
Interceptor
|
v
Controller (API)


---

# Where Spring Security Fits

Spring Security works using **Filters**.

When Spring Security is enabled, it adds **Security Filters** to the existing filter chain.


Client Request
|
v
Servlet Container
|
v
Filter Chain
|
|--- Filter1
|--- Filter2
|--- Security Filter Chain
| |
| |--- Security Filter 1
| |--- Security Filter 2
| |--- Security Filter N
|
|--- FilterN
|
v
DispatcherServlet
|
v
Controller


---

# Authentication Methods in Spring Security

Spring Security supports multiple authentication methods.

### 1. Form Based Authentication
User logs in with username and password.


username + password -> session created


---

### 2. Basic Authentication

Credentials are sent in request headers.


Authorization: Basic base64(username:password)


---

### 3. JWT Authentication

Used in stateless APIs.


Authorization: Bearer JWT_TOKEN


---

### 4. OAuth Authentication

Used for social login.

Examples:

- Google Login
- Facebook Login
- GitHub Login

---

# Spring Security Authentication Flow


Client Request
|
v
Security Filter
|
v
Authentication Manager
|
v
Authentication Provider
|
v
UserDetailsService
|
v
Password Encoder
|
v
Security Context
|
v
Controller


---

# Authentication Object

Spring Security uses an **Authentication object**.

Example structure:


Authentication {
principal
credentials
roles
isAuthenticated = false
}


Initially:


isAuthenticated = false


After successful authentication:


isAuthenticated = true


---

# Authentication Manager

Authentication Manager processes authentication requests.

Interface:


AuthenticationManager


Default Implementation:


ProviderManager


It acts as a **bridge between filters and providers**.


Filter -> AuthenticationManager -> AuthenticationProvider


---

# Authentication Provider

The **Authentication Provider** performs the actual authentication.

Example:


DaoAuthenticationProvider


Responsibilities:

- Validate username/password
- Fetch user details
- Verify credentials

---

# Password Encoder

Spring Security **never stores raw passwords**.

Passwords are stored as **hashed values**.


User Password -> Hash -> Store in DB


During login:


User Password -> Hash -> Compare with Stored Hash


---

# UserDetailsService

Used to fetch user information.

Interface:


UserDetailsService


Two common implementations:

### InMemoryUserDetailsManager

Users stored in memory.

Example:


username = admin
password = encoded_password


Used mainly for testing.

---

### JdbcUserDetailsManager

User data stored in database.

Example:


Users Table
Authorities Table


Used in production systems.

---

# Authentication Result

After successful authentication:


Authentication {
principal = user
roles = ROLE_USER
isAuthenticated = true
}


---

# Security Context

Authenticated user details are stored in:


SecurityContext


Managed by:


SecurityContextHolder


Request flow after authentication:


SecurityContext
|
v
DispatcherServlet
|
v
Controller


Controllers can access authenticated user information.

Example:

```java
Authentication auth =
SecurityContextHolder.getContext().getAuthentication();

String username = auth.getName();
