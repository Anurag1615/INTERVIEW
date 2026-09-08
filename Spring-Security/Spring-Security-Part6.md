# Spring Security – JWT Implementation in Spring Boot

## Introduction

JWT (JSON Web Token) is a **stateless authentication mechanism**.

Stateless means:

```
Server does NOT store authentication state
No session is maintained
Each request contains the authentication token
```

JWT has **three parts**:

```
HEADER.PAYLOAD.SIGNATURE
```

Example token:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJzdWIiOiJ1c2VyMSIsImV4cCI6MTcwMDAwMDAwMH0
.
abc123signature
```

---

# JWT Components

## Header

Metadata about the token.

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Fields:

```
alg → algorithm used for signing
typ → token type (JWT)
```

---

## Payload

Payload contains **claims** (data stored in token).

Example:

```json
{
  "sub": "user1",
  "email": "user@email.com",
  "role": "USER",
  "exp": 1710000000
}
```

Common claims:

```
sub → user identifier
exp → expiration time
iss → issuer
aud → audience
```

---

## Signature

Signature ensures that token data is **not tampered**.

Signature formula:

```
HMACSHA256(
 base64UrlEncode(header) +
 "." +
 base64UrlEncode(payload),
 secretKey
)
```

Final token:

```
base64(header).base64(payload).base64(signature)
```

---

# Steps to Implement JWT in Spring Boot

JWT implementation requires these steps:

```
1. User Registration
2. Token Generation
3. Token Validation
4. Refresh Token
```

---

# Step 1 – User Registration

Expose an API to register users.

```
POST /api/user-register
```

Request Body:

```json
{
 "username": "user1",
 "password": "123",
 "role": "ROLE_USER"
}
```

User is stored in database with **hashed password**.

Example DB table:

```
USER_REGISTER
-------------------
ID
USERNAME
PASSWORD
ROLE
```

Password stored using **BCrypt hashing**.

---

# Step 2 – Token Generation

Create API:

```
POST /generate-token
```

Input:

```
username
password
```

Steps:

```
1. Validate username/password
2. Generate JWT token
3. Return token to client
```

---

# JWT Dependencies

Add JWT libraries:

```xml
<dependency>
 <groupId>io.jsonwebtoken</groupId>
 <artifactId>jjwt-api</artifactId>
 <version>0.11.5</version>
</dependency>

<dependency>
 <groupId>io.jsonwebtoken</groupId>
 <artifactId>jjwt-impl</artifactId>
 <version>0.11.5</version>
</dependency>

<dependency>
 <groupId>io.jsonwebtoken</groupId>
 <artifactId>jjwt-jackson</artifactId>
 <version>0.11.5</version>
</dependency>
```

Purpose:

```
jjwt-api → interfaces
jjwt-impl → implementation
jjwt-jackson → JSON parsing
```

---

# JWT Authentication Filter

Create custom filter.

```
JwtAuthenticationFilter
```

Extend:

```
OncePerRequestFilter
```

Purpose:

```
Ensure filter runs once per request
```

Filter logic:

```
if endpoint == /generate-token
    read username/password
    create authentication object
    call AuthenticationManager
    generate JWT token
    return token
else
    pass request to next filter
```

---

# Authentication Object Creation

Create:

```
UsernamePasswordAuthenticationToken
```

Example:

```
new UsernamePasswordAuthenticationToken(
 username,
 password
)
```

This object is passed to:

```
AuthenticationManager
```

---

# Authentication Flow

```
Filter
   |
   | Authentication Token
   v
AuthenticationManager
   |
   v
ProviderManager
   |
   v
DaoAuthenticationProvider
   |
   v
UserDetailsService
   |
   v
PasswordEncoder
```

Steps:

```
1. Password hashed
2. User fetched from DB
3. Password compared
4. Authentication result returned
```

---

# Generating JWT Token

JWT utility class:

```
JwtUtil
```

Example code:

```java
String generateToken(String username) {

 return Jwts.builder()
  .setSubject(username)
  .setIssuedAt(new Date())
  .setExpiration(new Date(System.currentTimeMillis()+900000))
  .signWith(secretKey)
  .compact();
}
```

Token contains:

```
subject → username
issuedAt → creation time
expiration → expiry time
```

---

# Adding Token to Response

Token returned in response header.

Example:

```
Authorization: Bearer <JWT_TOKEN>
```

Example response:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

# Security Configuration

Custom security config required.

Example:

```java
@Bean
SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

 http
   .csrf(csrf -> csrf.disable())
   .sessionManagement(session ->
      session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
   .addFilterBefore(jwtAuthFilter,
      UsernamePasswordAuthenticationFilter.class);

 return http.build();
}
```

Key points:

```
Disable CSRF
Use stateless session policy
Add custom filter
```

---

# Token Validation

User accesses protected API.

Example request:

```
GET /api/users

Authorization: Bearer <token>
```

Steps:

```
1. Extract token from header
2. Validate token signature
3. Check expiration
4. Extract username
5. Load user details
6. Authenticate request
```

---

# JWT Validation Filter

Create filter:

```
JwtValidationFilter
```

Steps:

```
1. Read Authorization header
2. Extract token
3. Create JwtAuthenticationToken
4. Pass to AuthenticationManager
5. Validate token
6. Store authentication in SecurityContextHolder
7. Continue filter chain
```

---

# Custom Authentication Token

Create class:

```
JwtAuthenticationToken
```

Extends:

```
AbstractAuthenticationToken
```

Fields:

```
token
authenticated = false
```

Used to carry token data.

---

# Custom Authentication Provider

Create provider:

```
JwtAuthenticationProvider
```

Implements:

```
AuthenticationProvider
```

Responsibilities:

```
1. Validate JWT token
2. Extract username
3. Load user details
4. Create authenticated object
```

---

# Security Context

After successful validation:

```
SecurityContextHolder.getContext().setAuthentication(auth);
```

Now controller can access authenticated user.

Example:

```
Principal user = SecurityContextHolder.getContext().getAuthentication();
```

---

# Refresh Token Concept

Access tokens are **short-lived**.

Example:

```
Access Token → 15 minutes
Refresh Token → 7 days
```

Purpose:

```
Avoid asking username/password again
```

---

# Generating Refresh Token

During login:

```
Access Token → response header
Refresh Token → cookie
```

Example cookie:

```
refreshToken=abc123
```

Secure cookie settings:

```
HttpOnly = true
Secure = true
Path = /refresh-token
```

Benefits:

```
JavaScript cannot access cookie
Sent only over HTTPS
Used only for refresh endpoint
```

---

# Refresh Token Flow

User calls:

```
POST /refresh-token
```

Steps:

```
1. Extract refresh token from cookie
2. Validate refresh token
3. Generate new access token
4. Return access token
```

No username/password required.

---

# Refresh Token Filter

Create filter:

```
JwtRefreshFilter
```

Logic:

```
if endpoint == /refresh-token
    read refresh token from cookie
    validate token
    generate new access token
    return token
else
    continue filter chain
```

---

# Authorization

Authorization checks **user roles**.

Example:

```java
.requestMatchers("/users")
.hasRole("USER")
```

Example scenario:

```
User role = ADMIN
API requires = USER
```

Result:

```
403 Forbidden
```

Authorization handled by:

```
AuthorizationFilter
```

---

# Complete JWT Flow

```
User Register
     |
Generate Token API
     |
JWT Authentication Filter
     |
DaoAuthenticationProvider
     |
Token Generated
     |
Client sends JWT
     |
JWT Validation Filter
     |
JWT Authentication Provider
     |
SecurityContextHolder
     |
Authorization Filter
     |
Controller
```

---

# Key Points

```
JWT is stateless
No session storage
Custom filters required
Spring Security must be extended
```

---

# Important Interview Points

```
Spring Security does NOT provide default JWT implementation
Developers must implement JWT manually
Custom filter + provider required
JWT uses security filter chain
```

---

# JWT Security Best Practices

```
Use HTTPS
Keep access tokens short-lived
Store refresh tokens securely
Never store sensitive data in payload
Protect secret keys
```

---

# Summary

JWT implementation involves:

```
User Registration
Token Generation
Token Validation
Refresh Token Handling
Authorization Checks
```

JWT integrates with Spring Security using:

```
Custom Filters
Custom Authentication Provider
SecurityContextHolder
```
