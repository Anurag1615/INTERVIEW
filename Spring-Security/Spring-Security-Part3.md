# Spring Boot Security — Form Login Authentication

## Introduction

Form Login Authentication is the **default authentication mechanism in Spring Security**.

Before jumping into modern authentication mechanisms like **JWT or OAuth**, it is important to understand **how authentication was handled traditionally** and what limitations led to the development of newer methods.

Form login authentication is a **stateful authentication mechanism**, meaning the server maintains the **authentication state of the user** using a session.

---

# What is Stateful Authentication?

Stateful authentication means the server stores information about the user's authentication state.

```
Client → Login Request → Server
Server → Create Session → Store Authentication State
Client → Sends Session ID for future requests
```

The client does not need to send the **username and password again** after authentication.

Instead, the client sends the **session ID** with each request.

---

# Basic Flow of Form Login Authentication

```
Client (Browser)
      |
      | 1. Username + Password
      v
Server
      |
      | Validate Credentials
      v
Create HTTP Session
      |
      | Return Session ID (Cookie)
      v
Client
      |
      | Send Session ID in every request
      v
Server validates session and processes request
```

---

# Default Login and Logout Endpoints

Spring Security provides default endpoints for form authentication.

```
Login Endpoint  : /login
Logout Endpoint : /logout
```

- `/login` → Used to authenticate the user
- `/logout` → Used to invalidate the session

---

# Session Creation Process

When a user logs in:

1. User submits **username and password**.
2. Spring Security validates the credentials.
3. If authentication succeeds, an **HTTP session object is created**.
4. The session contains:

```
Session ID
Creation Time
Last Access Time
Expiry Time
Security Context
```

5. The session ID is sent back to the client as a **cookie**.

Example cookie:

```
Set-Cookie: JSESSIONID=ABCD123456
```

---

# Subsequent Requests

After login:

```
Client Request
Cookie: JSESSIONID=ABCD123456
```

Server:

1. Reads the session ID.
2. Searches the session store.
3. If session exists and is valid → request is processed.
4. If session is invalid → redirect to `/login`.

---

# Session Timeout

Default session timeout in Spring Boot:

```
30 minutes
```

You can configure session timeout using:

```properties
server.servlet.session.timeout=1m
```

Example:

```
1m → Session expires after 1 minute of inactivity
```

If the user continues interacting with the application, the **session expiry time is extended**.

---

# Where Sessions Are Stored

By default, sessions are stored **in memory (Servlet container)**.

Example:

```
Tomcat Memory
```

However, in distributed systems with multiple servers:

```
Client
   |
Load Balancer
  /    \
Server1  Server2
```

If sessions are stored in memory:

- Request 1 → Server1
- Request 2 → Server2

Server2 cannot find the session.

This causes the user to be logged out.

---

# Storing Sessions in Database

To solve this problem, sessions can be stored in a **database**.

Dependency:

```xml
<dependency>
 <groupId>org.springframework.session</groupId>
 <artifactId>spring-session-jdbc</artifactId>
</dependency>
```

Configuration:

```properties
spring.session.jdbc.initialize-schema=always
server.servlet.session.timeout=5m
```

Spring Boot automatically creates tables:

```
SPRING_SESSION
SPRING_SESSION_ATTRIBUTES
```

Example columns:

```
SESSION_ID
CREATION_TIME
LAST_ACCESS_TIME
MAX_INACTIVE_INTERVAL
EXPIRY_TIME
PRINCIPAL_NAME
```

---

# Form Login Authentication Flow (Detailed)

## Step 1 — Login Request

User sends:

```
POST /login
username
password
csrf_token
```

---

## Step 2 — Security Filter Chain

The request enters the **Spring Security filter chain**.

First filter involved:

```
UsernamePasswordAuthenticationFilter
```

This filter:

- Extracts username
- Extracts password
- Creates authentication object

Example:

```
UsernamePasswordAuthenticationToken
```

Authentication object contains:

```
principal   = username
credentials = password
authenticated = false
```

---

## Step 3 — Authentication Manager

The authentication object is sent to:

```
AuthenticationManager
```

It delegates authentication to:

```
DaoAuthenticationProvider
```

---

## Step 4 — Password Validation

Steps performed:

1. Password is encoded using `PasswordEncoder`.
2. User details are fetched from:

```
Database OR InMemoryUserDetailsManager
```

3. Passwords are compared.

---

## Step 5 — Authentication Success

If authentication succeeds:

```
authenticated = true
roles = USER / ADMIN
```

Authentication object is updated.

---

## Step 6 — Security Context

Authentication object is stored inside:

```
SecurityContext
```

---

## Step 7 — HTTP Session Creation

Security context is stored in:

```
HTTP Session
```

Example session structure:

```
HTTP Session
   |
   |-- Session ID
   |-- Expiry Time
   |-- Security Context
```

---

# Session Data Example

Example database entry:

```
SESSION_ID = ABC123
PRINCIPAL_NAME = user
LAST_ACCESS_TIME = timestamp
EXPIRY_TIME = timestamp
```

Security context contains:

```
username
roles
authentication status
```

---

# Request After Login

When the user sends another request:

```
GET /users
Cookie: JSESSIONID=ABC123
```

Spring Security performs:

1. Fetch HTTP session using session ID.
2. Extract security context.
3. Store it in `SecurityContextHolder`.

---

# Authorization Filter

Next filter:

```
AuthorizationFilter
```

It checks:

```
Does user have permission to access the resource?
```

Example rule:

```java
.requestMatchers("/users").hasRole("USER")
```

If role matches:

```
Request allowed
```

If role mismatch:

```
403 Forbidden
```

---

# Role Example

User stored with role:

```
ROLE_USER
```

API restriction:

```java
.requestMatchers("/users").hasRole("USER")
```

Access granted.

If user has role:

```
ROLE_ADMIN
```

Then access denied:

```
403 Forbidden
```

---

# Allow Public Endpoints

Example configuration:

```java
@Bean
SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

 http.authorizeHttpRequests(auth -> auth
      .requestMatchers("/users").permitAll()
      .anyRequest().authenticated()
 );

 return http.build();
}
```

---

# Controlling Sessions Per User

Prevent multiple logins.

Example:

```java
.sessionManagement(session ->
 session.maximumSessions(1)
 .maxSessionsPreventsLogin(true)
)
```

Meaning:

```
Only 1 active session per user
```

Second login attempt → blocked.

---

# Session Creation Policies

Spring Security provides 4 session policies.

### IF_REQUIRED (Default)

```
Session created only when required
```

### ALWAYS

```
Session always created
```

### NEVER

```
Do not create session but use existing session if available
```

### STATELESS

```
No session used
```

Used for:

```
JWT Authentication
```

---

# Disadvantages of Form Login Authentication

### 1. Vulnerable to CSRF

Session-based authentication can be attacked via CSRF.

### 2. Session Hijacking

If attacker steals the session ID:

```
Attacker gains full access
```

### 3. Session Management Overhead

Large applications require complex session management.

### 4. Scalability Issues

In distributed systems:

```
Session must be stored in DB or Cache
```

### 5. Database Load

Every request requires session validation.

---

# Summary

Form Login Authentication works as follows:

```
User Login
     |
Create Authentication Object
     |
Authentication Manager
     |
Validate Credentials
     |
Create Security Context
     |
Create HTTP Session
     |
Store Session ID in Cookie
     |
Subsequent Requests use Session ID
```

---

# Key Points

- Default authentication method in Spring Security
- Stateful authentication
- Uses HTTP session
- Stores session ID in cookies
- Works automatically with Spring Security filters

---
