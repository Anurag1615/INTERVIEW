# Spring Boot Security — Annotation Based Authorization

This is the **final part of Spring Boot Security** where we implement **authorization using annotations** instead of configuring everything inside the security filter chain.

Previously we implemented authorization like this:

```java
.requestMatchers("/api/orders")
.hasRole("USER")
```

This configuration is done in **SecurityConfig**.

---

# Why Annotation-Based Authorization?

In large applications:

```
100+
200+
500+ APIs
```

Managing authorization rules in **SecurityConfig** becomes difficult.

Example problem:

```
SecurityConfig contains authorization rules for hundreds of APIs
```

This becomes:

```
Hard to maintain
Hard to scale
Hard to read
```

Better solution:

```
Define authorization where API is defined
```

That means **inside the controller layer**.

---

# Annotation-Based Authorization

Spring Security provides annotations for this.

Main annotations:

```
@PreAuthorize
@PostAuthorize
```

---

# PreAuthorize vs PostAuthorize

## @PreAuthorize

Authorization happens **before method execution**.

Flow:

```
Request
↓
Authorization check
↓
Controller method executes
↓
Response
```

---

## @PostAuthorize

Authorization happens **after method execution**.

Flow:

```
Request
↓
Controller logic executes
↓
Authorization check
↓
Response returned
```

This is useful when **authorization depends on returned data**.

---

# Authentication Object Flow

When a user is authenticated, Spring Security creates an **Authentication Object**.

This object contains:

```
Username
Authorities
Roles
Permissions
```

Example:

```
Authentication
 ├── username
 ├── role_user
 ├── order_read
 └── sales_create
```

This **same authentication object reaches the controller layer**.

Annotations like `@PreAuthorize` use this object for authorization.

---

# Database Structure

To implement role + permission based authorization we create two tables.

## User Table

```
user_login
```

Fields:

```
id
username
password
role
```

Example:

| id | username | role |
|----|----------|------|
| 1 | shreyansh | ROLE_USER |

---

## Permission Table

```
user_permission
```

Fields:

```
id
name
```

Example permissions:

```
order_read
order_delete
sales_create
```

---

# Relationship

One user can have multiple permissions.

```
User (1) ------ (Many) Permissions
```

Example:

```
ROLE_USER
  ├── order_read
  ├── order_delete
  └── sales_create
```

---

# Implementing UserDetailsService

Spring Security requires implementing:

```
UserDetailsService
```

Example:

```java
@Override
public Collection<? extends GrantedAuthority> getAuthorities() {

List<GrantedAuthority> authorities = new ArrayList<>();

authorities.add(new SimpleGrantedAuthority(role));

permissions.forEach(p ->
    authorities.add(new SimpleGrantedAuthority(p.getName()))
);

return authorities;
}
```

This fills:

```
Authentication.grantedAuthorities
```

Example list:

```
ROLE_USER
order_read
sales_create
```

---

# Enable Method Security

To enable annotation-based authorization we must enable method security.

Add this in **SecurityConfig**.

```java
@EnableMethodSecurity(prePostEnabled = true)
```

Without this:

```
@PreAuthorize
@PostAuthorize
```

will not work.

---

# Example Controller

## Order Controller

```java
@GetMapping("/api/orders")
@PreAuthorize("hasRole('USER') and hasAuthority('order_read')")
public String getOrders() {
    return "All orders fetched successfully";
}
```

Requirements:

```
ROLE_USER
order_read permission
```

---

## Sales Controller

```java
@GetMapping("/api/sales")
@PreAuthorize("hasAuthority('sales_read')")
public String getSales() {
    return "Sales data fetched";
}
```

Requirement:

```
sales_read permission
```

---

# Testing Example

User created:

```
username: shreyansh
password: 123
role: ROLE_USER
permission: order_read
```

---

## Request 1

```
GET /api/orders
```

User has:

```
ROLE_USER
order_read
```

Result:

```
SUCCESS
```

---

## Request 2

```
GET /api/sales
```

User lacks:

```
sales_read
```

Result:

```
403 Forbidden
```

---

# hasRole vs hasAuthority

Important interview question.

## hasRole

Automatically adds prefix:

```
ROLE_
```

Example:

```java
hasRole("USER")
```

Internally becomes:

```
ROLE_USER
```

---

## hasAuthority

No prefix added.

Example:

```java
hasAuthority("order_read")
```

Checks exactly:

```
order_read
```

---

# Internally Both Are Same

Both ultimately check inside:

```
Authentication.getAuthorities()
```

Example list:

```
ROLE_USER
order_read
sales_create
```

Spring Security simply checks:

```
Is required value present in authorities list?
```

---

# How @PreAuthorize Works Internally

`@PreAuthorize` is intercepted by:

```
AuthorizationManagerBeforeMethodInterceptor
```

Flow:

```
Controller Method
↓
Interceptor triggered
↓
Authorization expression evaluated
↓
True → execute method
False → access denied
```

---

# Spring Expression Language (SpEL)

Authorization expressions are written using **SpEL**.

Example:

```java
@PreAuthorize("hasRole('USER') and hasAuthority('order_read')")
```

This string is parsed using:

```
SpelExpressionParser
```

Converted into:

```
Abstract Syntax Tree (AST)
```

Example tree:

```
AND
 ├── hasRole('USER')
 └── hasAuthority('order_read')
```

Spring evaluates this recursively.

---

# Additional Operators

SpEL supports logical operators.

### AND

```
hasRole('USER') and hasAuthority('order_read')
```

---

### OR

```
hasRole('ADMIN') or hasRole('USER')
```

---

### NOT

```
!hasRole('ADMIN')
```

---

# Accessing Method Parameters

You can use method parameters in authorization.

Example:

```java
@GetMapping("/users/{id}")
@PreAuthorize("#id == authentication.principal.id")
```

Explanation:

```
User can only access their own data
```

Example:

```
Authenticated User ID = 2
Request: /users/3
```

Result:

```
403 Forbidden
```

---

# PostAuthorize Example

`@PostAuthorize` runs **after method execution**.

Example:

```java
@PostAuthorize("returnObject.userId == authentication.principal.id")
```

---

# Example Scenario

Database users:

| id | username |
|----|---------|
| 1 | A_user |
| 2 | B_user |

Controller:

```java
@GetMapping("/api/orders")
@PostAuthorize("returnObject.userId == authentication.principal.id")
```

Controller returns:

```
OrderDTO
 ├── orderId
 └── userId
```

---

# Case 1

User:

```
A_user (id=1)
```

Returned order:

```
userId = 1
```

Authorization check:

```
1 == 1
```

Result:

```
Allowed
```

---

# Case 2

User:

```
B_user (id=2)
```

Returned order:

```
userId = 1
```

Authorization check:

```
1 == 2
```

Result:

```
Forbidden
```

---

# How PostAuthorize Works

`@PostAuthorize` is intercepted by:

```
AuthorizationManagerAfterMethodInterceptor
```

Flow:

```
Controller method executes
↓
Return object captured
↓
Authorization evaluated
↓
Allowed or Forbidden
```

---

# Complete Authorization Flow

```
Request
↓
Authentication Filter
↓
SecurityContextHolder
↓
Controller Method
↓
@PreAuthorize evaluated
↓
Business Logic
↓
@PostAuthorize evaluated
↓
Response returned
```

---

# Summary

Spring Security authorization can be implemented at:

```
1. Security Filter Layer
2. Controller Annotation Layer
```

Annotations provide:

```
Cleaner code
Better scalability
Fine-grained control
```

Main annotations:

```
@PreAuthorize
@PostAuthorize
```

Key concepts:

```
Roles
Authorities
Permissions
SpEL expressions
Authentication object
Method interceptors
```

---

# Interview Key Points

Common interview questions:

```
Difference between hasRole and hasAuthority
Difference between PreAuthorize and PostAuthorize
How Spring Security evaluates authorization expressions
How authentication object reaches controller layer
```

Understanding this flow is essential for **enterprise-level Spring Security applications**.
