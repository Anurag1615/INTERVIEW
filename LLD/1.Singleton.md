# Singleton Design Pattern

## 🔥 Definition
> Singleton Design Pattern ensures that a class has only one instance throughout the application and provides a controlled global access point to it.

---

# 1. Problem First (Without Singleton)

## Scenario
We are creating a **Database Connection class**.

## Implementation

```java
class DatabaseConnection {

    public DatabaseConnection() {
        System.out.println("New DB Connection Created");
    }
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        DatabaseConnection db1 = new DatabaseConnection();
        DatabaseConnection db2 = new DatabaseConnection();
    }
}
```

## ❌ Output

```
New DB Connection Created
New DB Connection Created
```

---

# 2. Problems in This Design

## ❌ Multiple Instances
- Each `new` creates a new object  
- Wastes memory and resources  

## ❌ No Control Over Object Creation

```java
new DatabaseConnection();
```

## ❌ Inconsistent State
- Multiple objects → different states → bugs  

---

# 3. Solution → Singleton Pattern

## Idea
- Only **one instance**
- Global access via method

---

# 4. Key Rules (With WHY)

✔ **Private Constructor**  
→ Prevents object creation using `new` keyword  
→ Ensures no external class can create instances  

✔ **Private Static Instance Variable**  
→ Stores single instance at class level  
→ Prevents external modification or reassignment  

✔ **Public Static Method (`getInstance()`)**  
→ Provides global access  
→ Controls object creation  
→ Always returns same instance  

---

# 5. Why NOT make instance public?

```java
public static Singleton instance;
```

## ❌ Problems

```java
Singleton.instance = null;
Singleton.instance = Singleton.getInstance();
```

👉 Breaks Singleton control

---

# 6. Why Constructor Alone is NOT Enough

✔ Prevents:
```
new Singleton();
```

❌ Does NOT prevent:
```
Singleton.instance = null;
```

👉 Need both constructor + private instance

---

# 7. Lazy Initialization (Basic Singleton)

```java
class Singleton {

    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {

        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Singleton obj1 = Singleton.getInstance();
        Singleton obj2 = Singleton.getInstance();

        System.out.println(obj1 == obj2); // true
    }
}
```

## ✅ Output

```
true
```

---

# 8. Internal Flow

```
First Call → instance == null → create object
Next Call  → return same object
```

---

# 9. Problem → Not Thread Safe

```
Thread 1 → instance == null
Thread 2 → instance == null
```

👉 Multiple objects created ❌

---

# 10. Thread-Safe Singleton (Synchronized)

```java
class Singleton {

    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {

        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

## ❌ Drawback
- Slow performance

---

# 11. Double Checked Locking (Best Practical Approach)

```java
class Singleton {

    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {

        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## ✅ Why `volatile` is required in Double Checked Locking?

Without `volatile`, JVM may reorder instructions while creating an object.

### Object Creation is NOT a single step

When we write:

```java
instance = new Singleton();
```

It actually happens in 3 steps:

```
1. Allocate memory
2. Assign reference to instance
3. Initialize object (constructor runs)
```

---

### ❌ Problem: Instruction Reordering

JVM can reorder steps for optimization:

```
1. Allocate memory
2. Assign reference
3. Initialize object
```

may become:

```
1. Allocate memory
2. Assign reference   ← reference is visible now
3. Initialize object  ← object not ready yet
```

---

### 🚨 What goes wrong?

Consider multithreading:

```
Thread 1:
→ starts creating object
→ assigns reference before initialization completes

Thread 2:
→ sees instance != null
→ returns partially constructed object ❌
```

👉 This leads to:
- Unexpected behavior
- Runtime bugs
- Hard-to-debug issues

---

### ✅ How `volatile` fixes it

```java
private static volatile Singleton instance;
```

`volatile` ensures:

✔ No instruction reordering  
✔ Visibility across threads  
✔ Object is fully constructed before being accessed  

---

### 🎯 Final Understanding

> `volatile` guarantees that any thread accessing the instance will always see a fully initialized object, preventing subtle concurrency bugs caused by instruction reordering.

---
## Why it is called Double Checked Locking?

Because we check the condition **twice** before creating the object.

---

### First Check (Without Lock)

```java
if (instance == null)
```

👉 This avoids locking when object is already created  
👉 Improves performance  

---

### Second Check (Inside Lock)

```java
synchronized (Singleton.class) {
    if (instance == null)
```

👉 Ensures only one thread creates the object  
👉 Prevents multiple object creation  

👉 **Uses class-level lock** (not object-level) because `instance` is static  

---

### 🔁 So "Double Check" = 2 checks

1. Check before locking (for performance)
2. Check after locking (for safety)

---

### 🎯 Why both checks?

#### ❌ If only first check:
Multiple threads can enter and create multiple objects  

#### ❌ If only synchronized:
Every call will acquire lock → slow performance  

#### ✅ With Double Check:
- First check → avoids unnecessary locking  
- Second check → ensures thread safety  

---

### 🧠 One Line

> It is called Double Checked Locking because we check the instance twice — once outside the synchronized block for performance and once inside for thread safety, using a class-level lock to handle static instance safely.


## Lock Type in Double Checked Locking

### ✅ Answer (Simple)

👉 In the code:

```java
synchronized (Singleton.class)
```

👉 This is a **class-level lock** ✅

---

### 🧠 Why Class-Level?

- `instance` is **static**
- Static data belongs to the **class**, not object  

👉 So locking must also be at **class level**

---

### 🔍 What does it mean?

```java
synchronized (Singleton.class)
```

👉 Only **one thread in the entire JVM** can enter this block for this class at a time  

👉 Ensures only one object is created globally  

---

### ❌ What if we use object-level lock?

```java
synchronized (this)
```

👉 Problems:

- `this` requires an object  
- But object is not created yet (`instance == null`)  

👉 So it is **not applicable / incorrect here** ❌  

---

### 💡 Key Rule

```
Static data   → Class-level lock
Instance data → Object-level lock
```

---

### 🔥 Interview Answer

> Since the instance variable is static, we use class-level locking (`Singleton.class`) to ensure only one thread can create the object across the entire application.

# 12. Can we make instance `final`?

## ❌ Lazy Initialization

```java
private static final Singleton instance;
```

👉 Compile error (must initialize immediately)

---

## ✅ Eager Initialization

```java
class Singleton {

    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

---

# 13. Best in Java → Enum Singleton

```java
enum Singleton {
    INSTANCE;

    public void show() {
        System.out.println("Singleton using Enum");
    }
}
```

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Singleton obj = Singleton.INSTANCE;
        obj.show();
    }
}
```

---

# 14. Flow Summary

```
Client → getInstance() → Same Object
```

---

# 15. Real-World Examples

- Logger  
- Database Connection Pool  
- Configuration Manager  
- Cache Manager  

---

# 16. Advantages

✔ Only one instance  
✔ Saves memory  
✔ Consistent state  
✔ Global access  

---

# 17. Disadvantages

❌ Hard to test  
❌ Tight coupling  
❌ Hidden dependencies  

---

# 18. When to Use

✔ Only one instance required  
✔ Shared resource  
✔ Expensive object  

---

# 19. When NOT to Use

❌ Multiple instances needed  
❌ Stateless objects  
❌ High testability required  

---

# 20. Interview One-Liner

> Private constructor prevents creation, private instance ensures control, and static method provides global access — together enforcing Singleton.

---

# 21. Pro Tip

> Singleton is not just about one object, it's about **controlled access to that object**.
