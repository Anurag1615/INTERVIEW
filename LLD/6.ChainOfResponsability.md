# 🔗 Chain of Responsibility Design Pattern

Chain of Responsibility is a **behavioral design pattern** where a request is passed through a chain of handlers until one of them handles it.

---

# 🧠 Core Idea

```
Request → Handler1 → Handler2 → Handler3 → ... → HandlerN
```

Each handler:

- Either **handles the request**
- Or **passes it to the next handler**

---

# 🔥 Problem (Without Pattern)

Imagine handling requests like:

```java
if (role.equals("ADMIN")) {
    // admin logic
} else if (role.equals("MANAGER")) {
    // manager logic
} else if (role.equals("USER")) {
    // user logic
}
```

### ❌ Issues:

- Big if-else chains
- Hard to extend
- Violates Open/Closed Principle
- Tight coupling

---

# ✅ Solution (With Chain of Responsibility)

```
Request → AdminHandler → ManagerHandler → UserHandler
```

Each handler:

- Decides **can I handle this?**
- If not → passes to next

---

# 🧩 Real-World Analogy

## 🏢 Customer Support Escalation

```
Level 1 Support → Level 2 → Manager → Director
```

- If Level 1 can't solve → escalate
- Request flows through chain

---

# 🏦 Backend Example (Important 🚀)

## Use Case: Request Authorization

```
User Request → AuthHandler → RoleHandler → PermissionHandler
```

---

# 🧱 Structure

## Components:

1. **Handler Interface / Abstract Class**
2. **Concrete Handlers**
3. **Client**

---

# 💻 Java Implementation (Interview Ready)

---

## 1️⃣ Handler Abstract Class

```java
public abstract class Handler {

    protected Handler next;

    public void setNext(Handler next) {
        this.next = next;
    }

    public abstract void handleRequest(String role);
}
```

---

## 2️⃣ Concrete Handlers

### AdminHandler

```java
public class AdminHandler extends Handler {

    @Override
    public void handleRequest(String role) {

        if (role.equals("ADMIN")) {
            System.out.println("Handled by Admin");
        } else if (next != null) {
            next.handleRequest(role);
        }
    }
}
```

---

### ManagerHandler

```java
public class ManagerHandler extends Handler {

    @Override
    public void handleRequest(String role) {

        if (role.equals("MANAGER")) {
            System.out.println("Handled by Manager");
        } else if (next != null) {
            next.handleRequest(role);
        }
    }
}
```

---

### UserHandler

```java
public class UserHandler extends Handler {

    @Override
    public void handleRequest(String role) {

        if (role.equals("USER")) {
            System.out.println("Handled by User");
        } else if (next != null) {
            next.handleRequest(role);
        }
    }
}
```

---

## 3️⃣ Client Code

```java
public class Main {

    public static void main(String[] args) {

        Handler admin = new AdminHandler();
        Handler manager = new ManagerHandler();
        Handler user = new UserHandler();

        admin.setNext(manager);
        manager.setNext(user);

        admin.handleRequest("USER");
    }
}
```

---

# 🎯 Output

```
Handled by User
```

---

# ⚖️ When To Use

## ✅ Use When:

- Multiple objects can handle a request
- You want to avoid large if-else chains
- You want flexible request processing

---

## ❌ Avoid When:

- Chain becomes too long (performance issue)
- Hard to debug flow

---

# 🔥 Chain vs Mediator vs State

| Pattern                     | Key Idea |
|---------------------------|---------|
| **Chain of Responsibility**| Pass request along chain |
| **Mediator**               | Central communication hub |
| **State**                  | Behavior changes based on state |

---

# 🧠 Interview One-Liner

> Chain of Responsibility allows multiple handlers to process a request sequentially, where each handler decides whether to handle or pass it further.

---

# 💣 Pro Tip (Real Systems 🚀)

Chain of Responsibility is heavily used in:

```
✔ Spring Security Filters
✔ Servlet Filters
✔ Logging frameworks
✔ Middleware pipelines
```

---

## 🌍 Real Example (Spring Security)

```
Request
 ↓
Authentication Filter
 ↓
Authorization Filter
 ↓
Exception Filter
 ↓
Controller
```

👉 Each filter decides:

```
Handle OR Pass forward
```

---

# 🧩 Key Takeaways

- Removes large conditional logic
- Promotes loose coupling
- Easy to extend handlers
- Follows Open/Closed Principle

---

# 📌 Final Thought

Chain of Responsibility is the backbone of:

```
✔ Middleware pipelines
✔ Security filters
✔ Request processing systems
```

---
