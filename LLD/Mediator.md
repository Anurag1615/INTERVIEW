# 🧠 Mediator Design Pattern

Mediator Pattern is a **behavioral design pattern** that reduces direct communication between objects by introducing a **central mediator** that manages all interactions.

---

# 🧠 Mediator Design Pattern (Explained with Chat App Example)

---

## 🚀 Why You Should Care

When multiple objects communicate directly, the system becomes:

- ❌ Tightly coupled  
- ❌ Hard to maintain  
- ❌ Difficult to extend  

👉 Mediator Pattern solves this problem by introducing a **central communication object**

---

## 🤯 Problem (Without Mediator)

Imagine a **chat application** with 3 users:

- A
- B
- C

### Communication Flow:

```
A → B
A → C
B → C
```

👉 Every user needs to know every other user  

### ❌ Problems:

- Too many connections  
- Code becomes messy  
- Adding a new user = modify existing code  

---

## 💡 Solution (Mediator Pattern)

Introduce a **ChatRoom (Mediator)**

```
A → ChatRoom ← B
        ↑
        C
```

👉 Users don’t communicate directly  
👉 All communication happens via **ChatRoom**

---

## ✈️ Real Life Analogy

- Planes don’t talk to each other  
- They communicate via **Air Traffic Control (Mediator)**  

👉 Same concept applies here

---

## 🧩 Structure

- **Mediator (Interface)** → Defines communication method  
- **Concrete Mediator (ChatRoom)** → Handles communication  
- **Colleagues (Users)** → Communicate via mediator  

---

## 🧑‍💻 Code Implementation

### 1️⃣ Mediator Interface

```java
interface ChatMediator {
    void sendMessage(String msg, User sender);
}
```

---

### 2️⃣ Concrete Mediator

```java
import java.util.*;

class ChatRoom implements ChatMediator {

    private List<User> users = new ArrayList<>();

    public void addUser(User user) {
        users.add(user);
    }

    @Override
    public void sendMessage(String msg, User sender) {
        for (User user : users) {
            if (user != sender) {
                user.receive(msg);
            }
        }
    }
}
```

---

### 3️⃣ User Class (Colleague)

```java
class User {

    private String name;
    private ChatMediator mediator;

    public User(String name, ChatMediator mediator) {
        this.name = name;
        this.mediator = mediator;
    }

    public void send(String msg) {
        System.out.println(this.name + " sends: " + msg);
        mediator.sendMessage(msg, this);
    }

    public void receive(String msg) {
        System.out.println(this.name + " received: " + msg);
    }
}
```

---

### 4️⃣ Main Class

```java
public class Main {
    public static void main(String[] args) {

        ChatRoom chatRoom = new ChatRoom();

        User A = new User("A", chatRoom);
        User B = new User("B", chatRoom);
        User C = new User("C", chatRoom);

        chatRoom.addUser(A);
        chatRoom.addUser(B);
        chatRoom.addUser(C);

        A.send("Hello everyone!");
    }
}
```

---

## 🔄 Execution Flow

```
A.send("Hello")
      ↓
ChatRoom (Mediator)
      ↓
B.receive("Hello")
C.receive("Hello")
```

---

## 🎯 Key Idea

> Objects don’t talk to each other directly.  
> They communicate through a mediator.

---

## ✅ Advantages

- Loose coupling ✔️  
- Centralized control ✔️  
- Easy to extend ✔️  
- Cleaner code ✔️  

---

## ❌ Disadvantages

- Mediator can become complex ⚠️  
- Risk of "God Class" if too much logic is added  

---

## 🧠 When to Use

- Many-to-many object communication  
- Complex interaction logic  
- Want to reduce dependencies  

---

## 🆚 Mediator vs Direct Communication

| Feature | Without Mediator | With Mediator |
|--------|----------------|--------------|
| Coupling | High ❌ | Low ✅ |
| Maintainability | Hard ❌ | Easy ✅ |
| Scalability | Poor ❌ | Good ✅ |

---

## 💥 Important Note

👉 This pattern is used **inside a single application (same JVM)**  
👉 It is **NOT for microservices communication**

---

## 🧠 Interview One-Liner

> “Mediator pattern introduces a central object to handle communication between multiple objects, reducing direct dependencies and improving maintainability.”

---

## 🚀 Pro Tip

👉 Best way to remember:

**“Chat Room Example = Mediator Pattern”**

---
