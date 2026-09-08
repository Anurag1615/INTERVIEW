# Spring Boot Security — Part 2 (User Creation)

## Overview

Before implementing authentication methods such as:

- Form Login
- Basic Authentication
- JWT
- OAuth

we must first **create users**.

Authentication and authorization can only occur **after users exist in the system**.

```
User Creation → Authentication → Authorization
```

---

# 1. Adding Spring Security Dependency

```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

When the application starts, Spring Boot automatically creates a default user.

Example log:

```
Using generated security password: 8d1a3c67-6c2c-4b8c-bc16-4c58d2c8d1ab
```

Default credentials:

```
Username : user
Password : randomly generated password
```

---

# 2. Default User Creation Flow

Internally Spring Security performs the following steps:

```
SecurityProperties.User
        |
        v
UserDetailsServiceAutoConfiguration
        |
        v
InMemoryUserDetailsManager
        |
        v
User stored in memory
```

User data is stored in an in-memory map.

Example structure:

```
Map<String, UserDetails> users
```

---

# 3. Custom User Using application.properties

For development purposes we can override default credentials.

```
spring.security.user.name=my_username
spring.security.user.password=my_password
spring.security.user.roles=USER
```

Spring Boot internally overrides default values using reflection.

⚠️ This approach is **not recommended for production**.

Limitations:

- Only one user
- Password stored in configuration
- Not scalable

---

# 4. Creating Multiple Users (In-Memory)

We can create a custom `UserDetailsService` bean.

```java
@Bean
public UserDetailsService userDetailsService() {

 UserDetails user1 =
     User.withUsername("user1")
     .password("{noop}1234")
     .roles("USER")
     .build();

 UserDetails user2 =
     User.withUsername("user2")
     .password("{noop}1234")
     .roles("ADMIN")
     .build();

 return new InMemoryUserDetailsManager(user1, user2);
}
```

---

# 5. Password Storage Format

Spring Security stores passwords using this format:

```
{id}encodedPassword
```

Examples:

```
{noop}1234
{bcrypt}$2a$10$ksdfkjsdf....
{sha256}ksdf987sd9f8....
```

---

# 6. Password Verification Flow

Example stored password:

```
{bcrypt}$2a$10$abcd12345
```

Authentication steps:

```
1. User enters password: 1234
2. System reads encoder id: bcrypt
3. Password encoded again using bcrypt
4. New hash compared with stored hash
```

Flow diagram:

```
User Password
      |
      v
DelegatingPasswordEncoder
      |
      v
Specific Encoder (bcrypt)
      |
      v
Password Comparison
```

---

# 7. Default Password Encoder

Spring Security uses:

```
DelegatingPasswordEncoder
```

Responsibilities:

```
1. Read password id
2. Choose correct encoder
3. Delegate encoding
```

Example mapping:

```
{bcrypt} → BCryptPasswordEncoder
{noop} → NoOpPasswordEncoder
```

---

# 8. Custom Password Encoder

Instead of writing `{bcrypt}` manually, we can configure encoder globally.

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

Then password creation becomes:

```java
.password(passwordEncoder.encode("1234"))
```

---

# 9. Why In-Memory Users Are Not Suitable For Production

Problems:

- Data lost after application restart
- Not scalable
- Users cannot be created dynamically

Production systems require **database-based user storage**.

---

# 10. Production Architecture

```
Client
  |
  v
Auth Controller
  |
  v
User Service
  |
  v
User Repository
  |
  v
Database
```

---

# 11. User Entity

Example entity:

```java
@Entity
@Table(name = "user_auth")
public class UserAuthEntity implements UserDetails {

 @Id
 @GeneratedValue
 private Long id;

 private String username;
 private String password;
 private String roles;
}
```

---

# 12. Why Implement UserDetails

Spring Security authentication expects return type:

```
UserDetails
```

So our entity implements:

```
UserDetails
```

Benefits:

```
UserAuthEntity directly returned
No mapping required
```

Without this we would need conversion:

```
UserEntity → UserDetails
```

---

# 13. Repository

```java
@Repository
public interface UserAuthRepository extends JpaRepository<UserAuthEntity, Long> {

 Optional<UserAuthEntity> findByUsername(String username);

}
```

Spring Boot automatically generates query:

```
SELECT * FROM user_auth WHERE username = ?
```

---

# 14. Custom UserDetailsService

Spring Security loads users using `UserDetailsService`.

```java
@Service
public class UserAuthService implements UserDetailsService {

 @Autowired
 private UserAuthRepository repository;

 @Override
 public UserDetails loadUserByUsername(String username)
         throws UsernameNotFoundException {

     return repository.findByUsername(username)
             .orElseThrow(() -> new UsernameNotFoundException("User not found"));
 }
}
```

---

# 15. Registration API

Users register dynamically.

```java
@PostMapping("/auth/register")
public String register(@RequestBody UserAuthEntity user) {

 user.setPassword(passwordEncoder.encode(user.getPassword()));

 repository.save(user);

 return "User Registered Successfully";
}
```

Password stored in database:

```
$2a$10$abc123hashedpassword
```

---

# 16. Allow Register API Without Authentication

By default all APIs require authentication.

We must bypass authentication for registration API.

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

 http
   .authorizeHttpRequests(auth -> auth
      .requestMatchers("/auth/register").permitAll()
      .anyRequest().authenticated()
   );

 return http.build();
}
```

---

# 17. Complete Production Flow

```
User Register
      |
      v
Password Hashed (BCrypt)
      |
      v
User Stored in Database
      |
      v
Authentication Request
      |
      v
UserDetailsService
      |
      v
Fetch User From Database
      |
      v
Password Encoder Compare
      |
      v
Authentication Success
```

---

# Key Takeaways

1. Spring Security creates default user automatically.
2. `InMemoryUserDetailsManager` is useful for testing.
3. Passwords should always be hashed.
4. `DelegatingPasswordEncoder` chooses encoding algorithm.
5. Production systems store users in database.
6. Implement `UserDetails` for direct integration with Spring Security.
7. `UserDetailsService` loads users during authentication.

---
