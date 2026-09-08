# Spring Security – Basic Authentication

## Introduction

Basic Authentication is another authentication method supported by **Spring Security**.

Unlike **Form Login Authentication (stateful)**, Basic Authentication is a **stateless authentication mechanism**.

Stateless means:

```
Server does NOT maintain authentication state
No session is created
Each request must carry credentials
```

---

# What is Basic Authentication?

Basic Authentication is a simple authentication mechanism where the **client sends username and password with every request**.

Credentials are sent in the **Authorization HTTP Header**.

Example header:

```
Authorization: Basic base64(username:password)
```

Example:

```
Authorization: Basic dXNlcjpwYXNz
```

Where:

```
username: user
password: pass
```

Encoded using **Base64 encoding**.

⚠ Important:

Base64 is **encoding, NOT encryption**.

Anyone intercepting the request can decode it easily.

Therefore:

```
Basic Authentication MUST always be used over HTTPS
Never use it with HTTP
```

---

# Creating a User

Before authentication, a user must exist.

Example configuration in `application.properties`:

```
spring.security.user.name=user
spring.security.user.password=pass
spring.security.user.roles=ADMIN
```

When the application starts:

```
User created in memory
Username = user
Password = pass
Role = ADMIN
```

---

# Sending Credentials in Request

Example request:

```
GET /api/users
```

Headers:

```
Authorization: Basic base64(user:pass)
```

When decoded:

```
user:pass
```

Spring Security extracts credentials from the header.

---

# Why Credentials are Passed in Headers

There are three main reasons.

## 1. Standardization

Basic Authentication follows **RFC 7617**.

This standard defines that credentials must be sent in the **Authorization header**.

Benefits:

```
Consistent authentication mechanism
Works across all clients and APIs
```

---

## 2. Security

Many servers log:

```
Request Body
Query Parameters
```

Credentials in these places might get logged.

Headers are generally **not logged**, reducing exposure.

---

## 3. Works with All HTTP Methods

Some HTTP methods:

```
GET
DELETE
```

Do not have request bodies.

Headers work for **all HTTP methods**.

---

# Basic Authentication Flow

```
Client
  |
  | Request with Authorization Header
  v
Security Filter Chain
  |
  v
BasicAuthenticationFilter
  |
  v
AuthenticationManager
  |
  v
DaoAuthenticationProvider
  |
  v
UserDetailsService
  |
  v
PasswordEncoder
  |
  v
SecurityContextHolder
  |
  v
AuthorizationFilter
  |
  v
Controller
```

---

# Step-by-Step Authentication Flow

## Step 1 – Request Sent

Client sends request:

```
GET /api/users
Authorization: Basic base64(username:password)
```

---

## Step 2 – BasicAuthenticationFilter

Spring Security invokes:

```
BasicAuthenticationFilter
```

Responsibilities:

```
Extract Authorization header
Decode Base64 credentials
Retrieve username and password
```

Decoded result:

```
username = user
password = pass
```

---

## Step 3 – Create Authentication Object

An authentication object is created.

```
UsernamePasswordAuthenticationToken
```

Structure:

```
principal = username
credentials = password
authenticated = false
```

---

## Step 4 – AuthenticationManager

Authentication object is sent to:

```
AuthenticationManager
```

Which delegates to:

```
DaoAuthenticationProvider
```

---

## Step 5 – Password Encoding

Incoming password is encoded using:

```
PasswordEncoder
```

---

## Step 6 – Fetch User Details

User details are fetched using:

```
UserDetailsService
```

Source can be:

```
InMemoryUserDetailsManager
Database
```

Example stored user:

```
username = user
password = hashed password
role = ADMIN
```

---

## Step 7 – Credential Validation

Spring Security compares:

```
Incoming username/password
Stored username/password
```

If valid:

```
authenticated = true
roles = ADMIN
```

---

## Step 8 – Store in Security Context

Authentication object stored in:

```
SecurityContext
```

Which is placed in:

```
SecurityContextHolder
```

SecurityContextHolder is accessible during the entire request lifecycle.

---

## Step 9 – Authorization Filter

Next filter:

```
AuthorizationFilter
```

Checks:

```
Does user have permission to access the resource?
```

Example configuration:

```
/api/users → ROLE_USER required
```

If user role matches:

```
Access granted
```

If mismatch:

```
403 Forbidden
```

---

# Implementation in Spring Boot

## Dependency

```
spring-boot-starter-security
```

---

## Security Configuration

Spring Security default authentication method is:

```
Form Login
```

To use Basic Authentication, override it.

Example:

```java
@Bean
SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

 http
   .httpBasic(Customizer.withDefaults())
   .sessionManagement(session ->
        session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
   .csrf(csrf -> csrf.disable())
   .authorizeHttpRequests(auth -> auth
        .requestMatchers("/api/users").hasRole("USER")
        .anyRequest().authenticated());

 return http.build();
}
```

---

# Key Configuration Points

## Enable Basic Authentication

```
httpBasic()
```

Overrides default form login.

---

## Stateless Authentication

```
SessionCreationPolicy.STATELESS
```

Meaning:

```
No session created
Credentials required for every request
```

---

## Disable CSRF

```
csrf().disable()
```

CSRF protection is unnecessary for **stateless authentication**.

---

## Authorization Rule

Example:

```
/api/users → ROLE_USER required
```

If user role is `ADMIN` instead of `USER`:

```
403 Forbidden
```

---

# Disadvantages of Basic Authentication

## 1. Credentials Sent in Every Request

Each request carries:

```
username
password
```

This increases security risk.

---

## 2. No Token or Session Control

If credentials are leaked:

```
Attacker gains full access
```

Only solution:

```
Change password
```

---

## 3. Poor User Experience

User credentials must be verified **for every request**.

---

## 4. Performance Overhead

For every request:

```
Decode Base64
Hash password
Fetch user from DB
Compare credentials
```

This increases processing cost.

---

## 5. Database Lookup

Each request requires a **database query**.

This increases latency.

---

# Summary

Basic Authentication works as follows:

```
Client sends username/password in Authorization header
       |
BasicAuthenticationFilter extracts credentials
       |
AuthenticationManager validates credentials
       |
UserDetailsService fetches user
       |
PasswordEncoder validates password
       |
SecurityContextHolder stores authentication
       |
AuthorizationFilter checks permissions
       |
Controller executes request
```

---

# Key Characteristics

```
Stateless authentication
No session management
Credentials required for every request
Simple but insecure without HTTPS
```

---

# Why JWT Became Popular

Basic Authentication problems:

```
Credentials sent every request
Security risks
Performance overhead
Database lookup each request
```

JWT solves these problems by using **tokens instead of credentials**.

Next topic:

```
JWT Authentication
```
