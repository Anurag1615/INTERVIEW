# 🧠 Observer Design Pattern (Easy + Interview Ready)

---

## 🚀 Why You Should Care

When one object changes state, multiple objects may need to be notified.

👉 Without proper design:

- ❌ Tight coupling  
- ❌ Hard to maintain  
- ❌ Difficult to scale  

👉 Observer Pattern solves this problem

---

## 🎯 Definition (Simple)

> Observer Pattern defines a one-to-many dependency where one object (Subject) notifies multiple objects (Observers) automatically when its state changes.

---

## 🤯 Problem (Without Observer)

### Example: YouTube Channel

```java
class YouTubeChannel {

    void uploadVideo() {
        System.out.println("New Video Uploaded");

        // manually notifying
        user1.notifyUser();
        user2.notifyUser();
        user3.notifyUser();
    }
}
```

### ❌ Problems

- Hardcoded users ❌  
- Adding/removing users is difficult ❌  
- Tight coupling ❌  

---

## 💡 Solution (Observer Pattern)

👉 Maintain a **list of observers (subscribers)**

```text
Channel → Notify → Subscribers
```

👉 Automatically notify all observers

---

## 🧩 Structure

- **Subject** → maintains list of observers  
- **Observer** → interface for update  
- **Concrete Subject** → actual implementation  
- **Concrete Observer** → actual subscribers  

---

## 🧑‍💻 Code Implementation

---

### 1️⃣ Observer Interface

```java
interface Observer {
    void update(String message);
}
```

---

### 2️⃣ Subject Interface

```java
interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers(String message);
}
```

---

### 3️⃣ Concrete Subject (YouTube Channel)

```java
import java.util.*;

class YouTubeChannel implements Subject {

    private List<Observer> observers = new ArrayList<>();

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void uploadVideo(String title) {
        System.out.println("New video uploaded: " + title);
        notifyObservers(title);
    }
}
```

---

### 4️⃣ Concrete Observer (Subscriber)

```java
class Subscriber implements Observer {

    private String name;

    public Subscriber(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " got notification: " + message);
    }
}
```

---

### 5️⃣ Client Code

```java
public class Main {
    public static void main(String[] args) {

        YouTubeChannel channel = new YouTubeChannel();

        Subscriber user1 = new Subscriber("A");
        Subscriber user2 = new Subscriber("B");
        Subscriber user3 = new Subscriber("C");

        channel.addObserver(user1);
        channel.addObserver(user2);
        channel.addObserver(user3);

        channel.uploadVideo("Observer Pattern Explained");
    }
}
```

---

## 🔄 Execution Flow

```text
Channel uploads video
        ↓
notifyObservers()
        ↓
All subscribers receive update
```

---

## 🎯 Key Idea

> Subject does not know details of observers  
> It just notifies them

---

## ✅ Advantages

- Loose coupling ✔️  
- Easy to add/remove observers ✔️  
- Scalable ✔️  
- Automatic updates ✔️  

---

## ❌ Disadvantages

- Too many notifications ⚠️  
- Hard to debug sometimes ⚠️  

---

## 🧠 When to Use

- Event systems  
- Notification systems  
- Real-time updates  

---

## 🆚 Observer vs Mediator

| Feature | Observer | Mediator |
|--------|---------|---------|
| Purpose | Notify changes | Manage communication |
| Relation | One-to-many | Many-to-many |
| Example | YouTube subscribers | Chat room |

---

## 💥 Real World Examples

- YouTube subscriptions  
- Stock price updates  
- Event listeners (Java, Spring)  

---

## 🧠 Interview One-Liner

> “Observer pattern defines a one-to-many relationship where multiple observers are notified automatically when the subject changes state.”

---

## 🚀 Easy Memory Trick

👉 **YouTube Channel = Subject**  
👉 **Subscribers = Observers**

---
