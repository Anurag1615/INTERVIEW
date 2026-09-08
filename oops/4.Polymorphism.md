
# 🔹 Polymorphism

---

## 🔹 Meaning

- Poly → Many  
- Morph → Forms  

👉 Polymorphism means **Many Forms**.

---

# 🔹 Real Life Example (As Per Notes)

A Boy starts love with the word **FRIENDSHIP**.

But a Girl ends love with the same word **FRIENDSHIP**.

Word is same  
Behaviour is different  
Meaning is different  

Same word → Different behaviour  

This is nothing but the beautiful concept of OOPS called:

👉 **Polymorphism**

---

# 🔹 Concept Structure

```
                Polymorphism
                      |
          --------------------------------
          |                              |
   Compile Time                    Runtime
 (Static / Early Binding)     (Dynamic / Late Binding)
          |                              |
    Method Overloading            Method Overriding
```

---

# 🔹 Types of Polymorphism

## 1️⃣ Compile Time Polymorphism

Also called:

- Static Polymorphism  
- Early Binding  

### ✔ Based On:
Reference Type  

### ✔ Example:
Method Overloading

```java
class Test {

    void m1(int a) {
        System.out.println("int version");
    }

    void m1(double a) {
        System.out.println("double version");
    }

    public static void main(String[] args) {

        Test t = new Test();
        t.m1(10);
    }
}
```

✔ Method resolved at compile time.

---

## 2️⃣ Runtime Polymorphism

Also called:

- Dynamic Polymorphism  
- Late Binding  

### ✔ Based On:
Object Type  

### ✔ Example:
Method Overriding

```java
class P {
    public void m1() {
        System.out.println("Parent");
    }
}

class C extends P {
    public void m1() {
        System.out.println("Child");
    }
}

class Test {
    public static void main(String[] args) {

        P p = new C();
        p.m1();
    }
}
```

### Output:
```
Child
```

✔ JVM decides at runtime based on object.

---

# 🔹 Important Point (Very Important)

In this line:

```java
P p = new C();
```

Two things are involved:

- Reference Type → P  
- Object Type → C  

---

# 🔹 Compile Time Checking – 1

Compiler checks relationship between reference and object.

Object must be:

- Same type OR
- Child type

Otherwise → Compile Time Error

---

# 🔹 Compile Time Checking – 2

Method must be present in reference type.

Example:

```java
class P {
    public void m1() {}
}

class C extends P {
    public void m2() {}
}

class Test {
    public static void main(String[] args) {

        P p = new C();
        p.m2();   // Compile Time Error
    }
}
```

Because compiler checks reference type only.

---

# 🔹 Method Resolution Summary

```
Compile Time:
   Based on Reference Type

Runtime:
   Based on Object Type
```

---

# 🔹 Static Methods and Polymorphism

Static methods:

- Resolved at compile time
- Based on reference
- No runtime polymorphism

Example:

```java
class P {
    public static void m1() {
        System.out.println("Parent");
    }
}

class C extends P {
    public static void m1() {
        System.out.println("Child");
    }
}

class Test {
    public static void main(String[] args) {

        P p = new C();
        p.m1();
    }
}
```

Output:
```
Parent
```

Because static binding.

---

# 🔹 Variables and Polymorphism

Variables are resolved at compile time.

Example:

```java
class P {
    int x = 10;
}

class C extends P {
    int x = 20;
}

class Test {
    public static void main(String[] args) {

        P p = new C();
        System.out.println(p.x);
    }
}
```

Output:
```
10
```

Variable resolution based on reference.

---

# 🔥 Final Summary Table

| Concept | Based On | Binding | Type |
|----------|----------|----------|------|
| Overloading | Reference | Compile Time | Static Poly |
| Overriding | Object | Runtime | Dynamic Poly |
| Static Method | Reference | Compile Time | Method Hiding |
| Variable | Reference | Compile Time | Variable Hiding |

---

# 🔥 Interview One Line

Polymorphism means:

👉 One interface, multiple implementations.

or

👉 Ability of one object to take many forms.
