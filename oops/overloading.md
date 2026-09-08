# 🔹 Method Overloading

## 🔹 Definition

Method Overloading occurs when multiple methods in the same class have:

- Same method name
- Different parameter list

Return type is NOT considered.

---

## 🔹 Overloading = Compile-Time Polymorphism

Method call is resolved at compile time based on reference type and arguments.

---

# 🔹 Case Study 1️⃣ – Exact Match

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

### Output:
```
int version
```

### Reason:
Exact match available → int version selected.

---

# 🔹 Case Study 2️⃣ – Widening

```java
class Test {

    void m1(long a) {
        System.out.println("long version");
    }

    public static void main(String[] args) {

        Test t = new Test();
        t.m1(10);
    }
}
```

### Output:
```
long version
```

### Reason:
int → long (widening allowed)

---

# 🔹 Case Study 3️⃣ – Widening vs Autoboxing

```java
class Test {

    void m1(Integer a) {
        System.out.println("Integer version");
    }

    void m1(long a) {
        System.out.println("long version");
    }

    public static void main(String[] args) {

        Test t = new Test();
        t.m1(10);
    }
}
```

### Output:
```
long version
```

### Reason:
Priority: Widening > Autoboxing  
So int → long selected.

---

# 🔹 Case Study 4️⃣ – Autoboxing

```java
class Test {

    void m1(Integer a) {
        System.out.println("Integer version");
    }

    public static void main(String[] args) {

        Test t = new Test();
        t.m1(10);
    }
}
```

### Output:
```
Integer version
```

### Reason:
int → Integer (Autoboxing)

---

# 🔹 Case Study 5️⃣ – Var-args

```java
class Test {

    void m1(int... a) {
        System.out.println("var-args version");
    }

    public static void main(String[] args) {

        Test t = new Test();
        t.m1(10);
    }
}
```

### Output:
```
var-args version
```

### Reason:
Var-args used when no better match available.

---

# 🔹 Case Study 6️⃣ – Ambiguity

```java
class Test {

    void m1(Integer a) {
        System.out.println("Integer");
    }

    void m1(String s) {
        System.out.println("String");
    }

    public static void main(String[] args) {

        Test t = new Test();
        t.m1(null);
    }
}
```

### Output:
```
Compile Time Error (Ambiguous)
```

### Reason:
null matches both Integer and String.
Compiler cannot decide.

---

# 🔹 Case Study 7️⃣ – Most Specific Method

```java
class Test {

    void m1(Object o) {
        System.out.println("Object version");
    }

    void m1(String s) {
        System.out.println("String version");
    }

    public static void main(String[] args) {

        Test t = new Test();
        t.m1("Durga");
    }
}
```

### Output:
```
String version
```

### Reason:
String is child of Object → Most specific method selected.

---

# 🔹 Overloading Resolution Priority

```
1. Exact Match
2. Widening
3. Autoboxing
4. Var-args
```

---

# 🔹 Resolution Flow Diagram

```
Method Call
     │
     ▼
Check Exact Match
     │
     ▼
If not found → Check Widening
     │
     ▼
If not found → Check Autoboxing
     │
     ▼
If not found → Check Var-args
```

---

# 🔹 Important Interview Points

- Overloading is compile-time polymorphism.
- Return type does not matter.
- Access modifiers may change.
- Static methods can be overloaded.
- Constructors can be overloaded.
- main() method can be overloaded.
- Method selection is based on reference type, not object type.
