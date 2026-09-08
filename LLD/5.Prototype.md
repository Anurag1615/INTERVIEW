# Prototype Design Pattern

## 🔥 Definition
> Prototype Design Pattern is a creational design pattern that creates new objects by copying an existing object (prototype) instead of creating from scratch.

---

# 1. Problem First (Without Prototype)

## Scenario
Suppose creating an object is **expensive** (DB call, heavy computation, complex setup).

---

## Example

```java
class Student {

    private String name;

    public Student(String name) {
        // Simulate heavy operation
        System.out.println("Loading data from DB...");
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

---

## Usage

```java
public class Main {

    public static void main(String[] args) {

        Student s1 = new Student("Anurag");
        Student s2 = new Student("Anurag");
    }
}
```

---

## ❌ Output

```
Loading data from DB...
Loading data from DB...
```

---

# 2. Problems in This Design

## ❌ Expensive Object Creation
- Repeated DB calls  
- Heavy processing every time  

## ❌ Performance Issues
- Slow system  

## ❌ Duplicate Logic
- Same object created again and again  

---

# 3. Solution → Prototype Pattern

## Idea

👉 Instead of creating new object  
👉 **Clone existing object**

```
Original Object → Clone → New Object
```

---

# 4. Implementation

## Step 1: Implement Cloneable

```java
class Student implements Cloneable {

    private String name;

    public Student(String name) {
        System.out.println("Loading data from DB...");
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

---

## Step 2: Use Clone

```java
public class Main {

    public static void main(String[] args) throws Exception {

        Student s1 = new Student("Anurag");

        Student s2 = (Student) s1.clone(); // no DB call

        System.out.println(s1.getName());
        System.out.println(s2.getName());
    }
}
```

---

## ✅ Output

```
Loading data from DB...
Anurag
Anurag
```

👉 DB call happens only once ✅

---

# 5. Shallow Copy vs Deep Copy

## 🔹 Shallow Copy

 The shallow copy only creates a new reference and points to the same object

```java
return super.clone();
```

- Copies object  
- References are shared  

---

## 🔹 Deep Copy

In a deep copy, we create a new object and copy the old object value to the new object.

```java
@Override
protected Object clone() throws CloneNotSupportedException {
    Student copy = (Student) super.clone();
    copy.name = new String(this.name);
    return copy;
}
```

- Fully independent copy  
- No shared references  

---

# 6. When to Use

✔ Object creation is expensive  
✔ Many similar objects needed  
✔ Avoid repeated initialization  

---

# 7. Advantages

✔ Improves performance  
✔ Reduces object creation cost  
✔ Avoids duplicate initialization logic  

---

# 8. Disadvantages

❌ Cloning can be complex  
❌ Deep copy is difficult  
❌ Requires careful handling of references  

---

# 9. Real-World Examples

- Object caching  
- Game character cloning  
- Spring Bean scopes (prototype scope)  
- Document templates  

---

# 10. Flow

```
Client → Existing Object → Clone → New Object
```

---

# 11. Interview One-Liner

> Prototype pattern creates new objects by copying an existing object, reducing the cost of object creation.

---

# 12. Important Insight

> Prototype avoids costly object creation by reusing existing instances through cloning.

---

# 13. Pro Tip

> Use Prototype when object creation is expensive and cloning is cheaper than creating from scratch.
