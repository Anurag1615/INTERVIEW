# Spring Security – JWT (JSON Web Token)

## Introduction

JWT stands for **JSON Web Token**.

JWT provides a **secure way to transmit information between two parties using JSON objects**.

The information inside JWT is **digitally signed** using cryptographic algorithms such as:

- HMAC
- RSA

Because it is digitally signed, the receiver can verify that:

```
Data was not modified
Token was issued by trusted authority
```

---

# Where JWT is Used

JWT is widely used in:

```
Authentication
Authorization
Single Sign-On (SSO)
```

### Authentication

Verify the identity of a user.

Example:

```
User logs in → Token generated → Token proves identity
```

---

### Authorization

Check whether a user has permission to access resources.

Example:

```
User authenticated
Token contains roles
Server checks roles for permission
```

---

### Single Sign-On (SSO)

User logs in once and accesses multiple applications.

Example:

```
Login → App1
      → App2
      → App3
```

All applications trust the same **JWT token**.

---

# JWT Authentication Flow

```
Client
   |
   | Username + Password
   v
Authentication Server
   |
   | Generate JWT Token
   v
Client
   |
   | Request Resource + JWT
   v
Resource Server
   |
   | Validate Token
   v
Access Granted
```

### Steps

1. Client sends **username and password**.
2. Authentication server verifies credentials.
3. Authentication server generates **JWT token**.
4. Token is returned to client.
5. Client sends token in every request.
6. Server validates token before giving access.

---

# Authorization Header with JWT

JWT tokens are sent inside the **Authorization header**.

Example:

```
Authorization: Bearer <JWT_TOKEN>
```

Example request:

```
GET /api/users

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Important:

```
Bearer → indicates token authentication
Basic → indicates username/password authentication
```

---

# Before JWT – Session Based Authentication

Previously authentication worked using **Session IDs**.

### Session Authentication Flow

```
Client → Login (username/password)

Server:
   Create Session ID
   Store session in DB

Client:
   Sends session ID in cookie

Server:
   Fetch session from DB
   Validate session
```

---

# Problems with Session Authentication

### 1. Stateful

Server must store session data.

```
Session ID
User data
Roles
Expiry
```

---

### 2. Database Lookup

Every request requires database lookup.

```
SessionID → Query DB → Fetch data
```

---

### 3. Scalability Problems

In distributed systems:

```
Load Balancer
     |
Server1  Server2
```

Sessions must be synchronized across servers.

---

# JWT Solves These Problems

JWT is **stateless**.

Server does not store session.

All required information is inside the **token itself**.

Example data stored in token:

```
User ID
Roles
Expiry
Issuer
Audience
```

---

# JWT Structure

JWT consists of **three parts**.

```
HEADER.PAYLOAD.SIGNATURE
```

Example:

```
xxxxx.yyyyy.zzzzz
```

---

# JWT Components

## 1. Header

Contains metadata about the token.

Example:

```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```

Fields:

```
typ → token type
alg → signing algorithm
```

Common algorithms:

```
HS256 (HMAC)
RS256 (RSA)
```

---

## 2. Payload

Payload contains **claims**.

Claims are the data stored inside the token.

Example payload:

```json
{
 "sub": "123456789",
 "name": "Shreyansh",
 "email": "user@email.com",
 "role": "ADMIN",
 "exp": 1710000000
}
```

---

# Types of Claims

Claims are categorized into three types.

---

## 1. Registered Claims

Standard claims defined by JWT specification.

Examples:

```
iss → Issuer
sub → Subject (User ID)
aud → Audience
exp → Expiration Time
nbf → Not Before
iat → Issued At
jti → JWT ID
```

Example:

```
exp → Token expiration time
iss → Who issued the token
sub → User identifier
```

---

## 2. Public Claims

Custom claims used by applications.

Examples:

```
email
country
role
city
```

Example:

```json
{
 "email": "user@email.com",
 "country": "India"
}
```

These claims can be understood by multiple services.

---

## 3. Private Claims

Claims used internally by a specific system.

Example:

```
iam
internal_id
service_flag
```

These claims are **not standardized** and only understood by the issuing server.

---

# 3. Signature

Signature ensures that the token was **not tampered with**.

Signature creation process:

```
Base64Encode(Header)
+
Base64Encode(Payload)
+
Secret Key
```

Example formula:

```
Signature =
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```

Final token format:

```
Base64(header).Base64(payload).Base64(signature)
```

Example:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJzdWIiOiIxMjM0NTY3ODkwIiwiZXhwIjoxNzEwMDAwMDAwfQ
.
abc123signature
```

---

# Why JWT is Stateless

All information is stored inside the token.

Server does not store anything.

```
Token → Contains user data
Token → Contains expiry
Token → Contains signature
```

Server only needs to **verify signature**.

---

# Advantages of JWT

### Compact

JWT is small enough to send in HTTP headers.

---

### Stateless

No session storage required.

---

### Self-contained

Token contains all required user information.

---

### Secure

Digital signature ensures token integrity.

---

### Built-in Expiration

JWT supports expiration using `exp` claim.

---

### Custom Data

Developers can add custom claims.

---

# JWT and JWS

Technically:

```
JWT → JSON Web Token
JWS → JSON Web Signature
```

Most JWT implementations actually use **JWS** because tokens include signatures.

Thus both terms are often used interchangeably.

---

# JWT Challenges

Despite its advantages, JWT has some challenges.

---

# 1. Token Invalidation Problem

JWT is stateless.

Server does not store tokens.

Problem:

```
User becomes fraudulent
Token still valid
Server cannot revoke token immediately
```

Example:

```
Token expiry = 24 hours
User blocked after 2 hours
Token still valid
```

---

# Solutions for Token Invalidation

### 1. Blacklist Tokens

Maintain a blacklist.

```
Blacklist DB / Cache
```

Check blacklist before accepting token.

Disadvantage:

```
Requires DB lookup
Breaks stateless nature
```

---

### 2. Rotate Secret Keys

Change signing keys.

Problem:

```
All existing tokens become invalid
Including valid users
```

---

### 3. Short-lived Tokens

Tokens expire quickly.

Example:

```
Token expiry = 5 minutes
```

Most popular approach.

---

### 4. One-time Tokens

Token usable only once.

Requires storing token IDs.

---

# 2. JWT Payload Is Encoded Not Encrypted

Payload is **Base64 encoded**.

Anyone can decode it.

Example:

```
Base64Decode(payload) → visible data
```

Never store sensitive data such as:

```
password
credit card number
secret keys
```

---

# JWE – JSON Web Encryption

To encrypt JWT payload, use **JWE**.

Difference:

```
JWT → payload encoded
JWE → payload encrypted
```

Encryption ensures payload cannot be read.

---

# 3. Unsecured JWT

If algorithm is set to:

```
alg = none
```

Then token has **no signature**.

Example header:

```json
{
 "alg": "none",
 "typ": "JWT"
}
```

This creates **unsecured JWT**.

Such tokens must always be **rejected**.

---

# 4. JWK Exploit

JWT header may include:

```
jwk → JSON Web Key
```

It contains public key used for verification.

Attack risk:

```
Attacker modifies payload
Attacker signs token with own key
Attacker includes own public key in header
```

If server trusts this key → attack succeeds.

---

# Secure Solution – Use Trusted Key Store

Instead of trusting header keys:

Use trusted public keys from authentication server.

Example endpoint:

```
/.well-known/jwks.json
```

This contains trusted public keys.

Example:

```json
{
 "keys": [
  {
   "kid": "abc123",
   "kty": "RSA",
   "n": "...",
   "e": "AQAB"
  }
 ]
}
```

---

# Key ID (kid)

JWT header may include:

```
kid → Key ID
```

Server uses this ID to find the correct public key.

Example:

```
kid = abc123
```

Server fetches key from trusted JWKS list.

---

# JWT Single Sign-On Example

```
User → Login once
     |
     | Receive JWT
     v
App1 → verify token
App2 → verify token
App3 → verify token
```

User does not log in again.

JWT enables **Single Sign-On**.

---

# Summary

JWT is a **stateless authentication mechanism**.

Structure:

```
HEADER.PAYLOAD.SIGNATURE
```

Advantages:

```
Stateless
Fast
Self-contained
Secure with signatures
Supports SSO
```

Challenges:

```
Token revocation
Payload visibility
Key management
```

Despite challenges, JWT is widely used in **modern microservices authentication systems**.
