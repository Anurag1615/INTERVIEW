# OAuth 2.0 – Complete Notes

## What is OAuth 2.0?

OAuth stands for **Open Authorization**.

It is an **authorization framework** that enables **secure third-party access to protected user data** without sharing credentials.

Example:

```
User → Login to Instagram using Gmail
```

Instead of giving Gmail password to Instagram, the user **authorizes Gmail to share limited data**.

---

# Example Scenario

User wants to login to a third-party website.

```
User → Instagram
      → Login using Gmail
```

Flow:

```
User already logged into Gmail
↓
Instagram requests permission
↓
User allows access
↓
Gmail shares limited data
↓
User logs into Instagram
```

---

# OAuth 2.0 Actors (Important)

OAuth involves **4 main actors**.

## 1. Resource Owner

The **user who owns the data**.

Example:

```
User (Shreyansh)
```

Owns:

```
Email
Name
DOB
Profile
```

---

## 2. Client

Application requesting access.

Example:

```
Instagram
```

Client wants access to:

```
User email
User profile
```

---

## 3. Authorization Server

Server responsible for:

```
Authenticating user
Issuing access tokens
```

Example:

```
Gmail Authorization Server
```

---

## 4. Resource Server

Server storing user data.

Example:

```
Gmail server storing user data
```

---

# OAuth Actors Architecture

```
+-------------------+
|   Resource Owner  |
|      (User)       |
+---------+---------+
          |
          v
+-------------------+
|       Client      |
|    (Instagram)    |
+---------+---------+
          |
          v
+-------------------+
| Authorization     |
| Server (Gmail)    |
+---------+---------+
          |
          v
+-------------------+
| Resource Server   |
| (User Data Host)  |
+-------------------+
```

---

# OAuth Grant Types

Grant types define **how a client obtains an access token**.

Main types:

```
1. Authorization Code Grant
2. Implicit Grant
3. Resource Owner Password Grant
4. Client Credentials Grant
5. Refresh Token Grant
```

Most commonly used:

```
Authorization Code Grant
```

---

# Authorization Code Grant Flow

Used when:

```
User logs into third-party apps using Google / Facebook
```

Example:

```
Login with Google
```

---

# Step 1 — Client Registration

Client must register with Authorization Server.

Example request:

```
POST /register
```

Request:

```
client_name = Instagram
redirect_uri = https://instagram.com/callback
```

Response:

```
client_id
client_secret
```

Important:

```
client_secret must remain confidential
```

---

# Step 2 — Authorization Request

User clicks:

```
Sign in with Gmail
```

Client redirects user to authorization server.

Example request:

```
GET /authorize
```

Parameters:

```
response_type = code
client_id = CLIENT_ID
redirect_uri = CALLBACK_URL
scope = email profile
state = random_string
```

Explanation:

| Parameter | Purpose |
|----------|--------|
| response_type | expected response |
| client_id | identifies client |
| redirect_uri | callback URL |
| scope | requested permissions |
| state | CSRF protection |

---

# Step 3 — User Authentication

User authenticates with Gmail.

Example:

```
Enter Gmail username/password
```

Then Gmail asks for consent.

Example message:

```
Instagram wants access to:
✓ Email
✓ Profile
```

User clicks:

```
Allow
```

---

# Step 4 — Authorization Code Returned

Authorization server redirects user back.

Example:

```
https://instagram.com/callback?code=AUTH_CODE&state=XYZ
```

Response contains:

```
authorization_code
state
```

---

# Why State Parameter is Important

State protects against **CSRF attacks**.

Example:

```
Client sends state = ABC123
```

Authorization server returns:

```
state = ABC123
```

Client verifies:

```
if response_state != request_state
    reject request
```

This prevents attackers from injecting authorization codes.

---

# CSRF Attack Example

Without state validation:

```
Attacker obtains authorization code
↓
Attacker injects code into victim request
↓
Client exchanges token
↓
Victim logged into attacker's account
```

State validation prevents this.

---

# Step 5 — Exchange Authorization Code for Token

Client requests access token.

Example request:

```
POST /token
```

Parameters:

```
grant_type = authorization_code
code = AUTH_CODE
redirect_uri = CALLBACK_URL
client_id = CLIENT_ID
client_secret = CLIENT_SECRET
```

---

# Token Response

Example response:

```
{
 access_token: "abc123",
 token_type: "Bearer",
 expires_in: 3600,
 refresh_token: "xyz456"
}
```

Meaning:

| Field | Description |
|------|-------------|
| access_token | used to access resources |
| token_type | bearer token |
| expires_in | expiration time |
| refresh_token | obtain new token |

---

# Step 6 — Access Protected Resource

Client uses access token.

Example request:

```
GET /userinfo
Authorization: Bearer ACCESS_TOKEN
```

Flow:

```
Client → Resource Server
Resource Server → Authorization Server (validate token)
Authorization Server → Valid
Resource Server → Returns data
```

Returned data:

```
email
name
profile
```

---

# Step 7 — Token Expiration

Access tokens are **short-lived**.

Example:

```
15 minutes
30 minutes
1 hour
```

After expiration client must refresh token.

---

# Refresh Token Grant

Used to generate new access token.

Request:

```
POST /token
```

Parameters:

```
grant_type = refresh_token
refresh_token = REFRESH_TOKEN
client_id = CLIENT_ID
client_secret = CLIENT_SECRET
```

Response:

```
new_access_token
new_refresh_token
```

---

# Implicit Grant

Not recommended anymore.

Difference:

```
Authorization Code Grant
Client receives authorization code
then exchanges code for token
```

Implicit Grant:

```
Token returned directly
```

Request:

```
GET /authorize
```

Parameters:

```
response_type = token
client_id
redirect_uri
scope
state
```

Response:

```
access_token returned directly
```

No refresh token.

Security risk → deprecated.

---

# Resource Owner Password Credentials Grant

Client directly receives **username/password**.

Request:

```
POST /token
```

Parameters:

```
grant_type = password
username = user@gmail.com
password = user_password
client_id
client_secret
scope
```

Response:

```
access_token
refresh_token
```

This method is discouraged.

Reason:

```
Client gets user password
```

---

# Client Credentials Grant

Used when **client itself is resource owner**.

Example:

```
Backend service accessing another service
```

Request:

```
POST /token
```

Parameters:

```
grant_type = client_credentials
client_id
client_secret
scope
```

Response:

```
access_token
```

No refresh token.

If expired:

```
request new token
```

---

# OAuth Flow Summary

```
User → Client
Client → Authorization Server
Authorization Server → User authentication
User → Consent
Authorization Server → Authorization Code
Client → Token Request
Authorization Server → Access Token
Client → Resource Server
Resource Server → Return Data
```

---

# OAuth Authorization Code Architecture

```
User
  |
  v
Client (Instagram)
  |
  v
Authorization Server (Google)
  |
  v
Resource Server (User Data)
```

---

# Key Interview Points

OAuth is:

```
Authorization framework
```

Not:

```
Authentication protocol
```

Access token:

```
Short-lived
```

Refresh token:

```
Long-lived
```

Most secure grant type:

```
Authorization Code Grant
```

Implicit Grant:

```
Deprecated
```

---

# Real-World Examples

OAuth is used in:

```
Login with Google
Login with Facebook
Login with GitHub
Login with Apple
```

---

# Final Summary

OAuth allows:

```
Secure third-party authorization
```

Without exposing:

```
User passwords
```

Most common flow:

```
Authorization Code Grant
```

Provides:

```
Security
Scalability
Token based access
```
