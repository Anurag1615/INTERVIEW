# Builder Design Pattern

## 🔥 Definition
> Builder Design Pattern is a creational design pattern used to construct complex objects step by step, allowing different representations of the same object while keeping the construction process separate from its representation.

---

# 1. Problem First (Without Builder)

## Scenario
We have a class with many parameters.

---

## Example

```java
class User {

    private String name;
    private int age;
    private String email;
    private String phone;

    public User(String name, int age, String email, String phone) {
        this.name = name;
        this.age = age;
        this.email = email;
        this.phone = phone;
    }
}
```

---

## Usage

```java
User user = new User("Anurag", 22, "abc@gmail.com", "9999999999");
```

---

## ❌ Problems

### ❌ 1. Too Many Parameters
Hard to read and remember order

```java
new User("Anurag", 22, "abc@gmail.com", "9999999999");
```

---

### ❌ 2. Optional Fields Issue

What if phone is optional?

```java
new User("Anurag", 22, "abc@gmail.com", null);
```

👉 Ugly and error-prone ❌

---

### ❌ 3. Constructor Explosion

Multiple constructors needed:

```java
User(String name)
User(String name, int age)
User(String name, int age, String email)
...
```

👉 Not scalable ❌

---

# 2. Solution → Builder Pattern

## Idea

👉 Construct object step by step  
👉 Provide readable and flexible object creation  

```
UserBuilder → build() → User Object
```

---


```# 🧩 UML Diagram (Modern Builder Pattern)

```
        +------------------+
        |      Client      |
        +------------------+
                 |
                 v
        +----------------------+
        |      Builder         |
        |  (Static Inner Class)|
        +----------------------+
        | setName()            |
        | setAge()             |
        | setEmail()           |
        | setPhone()           |
        | build()              |
        +----------------------+
                 |
                 v
        +----------------------+
        |       Product        |
        |       (User)         |
        +----------------------+
        | name                 |
        | age                  |
        | email                |
        | phone                |
        +----------------------+



---

# 3. Implementation

## Step 1: Create Builder Class

```java
class User {

    private String name;
    private int age;
    private String email;
    private String phone;

    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
        this.phone = builder.phone;
    }

    public static class Builder {

        private String name;
        private int age;
        private String email;
        private String phone;

        public Builder setName(String name) {
            this.name = name;
            return this;
        }

        public Builder setAge(int age) {
            this.age = age;
            return this;
        }

        public Builder setEmail(String email) {
            this.email = email;
            return this;
        }

        public Builder setPhone(String phone) {
            this.phone = phone;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}
```

---

## Step 2: Usage

```java
public class Main {

    public static void main(String[] args) {

        User user = new User.Builder()
                .setName("Anurag")
                .setAge(22)
                .setEmail("abc@gmail.com")
                .build();

        System.out.println(user);
    }
}
```

---

## ✅ Benefits in Usage

✔ No need to remember parameter order  
✔ Optional fields handled easily  
✔ Readable code  

---

# 4. Flow

```
Client → Builder → build() → Object
```

---

# 5. Key Concepts (WHY)

✔ **Private Constructor**  
→ Prevents direct object creation  

✔ **Static Inner Builder Class**  
→ Helps in step-by-step construction  

✔ **Method Chaining (return this)**  
→ Improves readability  

✔ **build() Method**  
→ Final object creation  

---

# 6. Advantages

✔ Readable and maintainable  
✔ Handles optional parameters  
✔ Avoids constructor explosion  
✔ Immutable object creation possible  

---

# 7. Disadvantages

❌ More code (extra class)  
❌ Slight complexity  

---

# 8. When to Use

✔ Object has many parameters  
✔ Some parameters are optional  
✔ Need readable object creation  

---

# 9. Real-World Examples

- StringBuilder in Java  
- Lombok @Builder  
- HTTP request builders  
- Configuration objects  

---

# 10. Interview One-Liner

> Builder pattern constructs complex objects step by step and provides a readable way to create objects with many optional parameters.

---

# 11. Pro Tip

> Use Builder when constructor becomes complex or hard to read due to multiple parameters.
