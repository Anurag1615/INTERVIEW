# JVM, JRE, JDK, Class Loading & Memory Areas

---

## 1️⃣ JVM vs JRE vs JDK

```
JDK  =  JRE + Development Tools (compiler javac, debugger, etc.)
JRE  =  JVM + Core Libraries (rt.jar / modules)
JVM  =  Just the runtime engine that executes bytecode
```

| Component | Full Form | Contains | Purpose |
|-----------|-----------|----------|---------|
| **JVM** | Java Virtual Machine | Class loader, runtime memory areas, execution engine | Executes `.class` bytecode |
| **JRE** | Java Runtime Environment | JVM + standard libraries | Required to **run** Java programs |
| **JDK** | Java Development Kit | JRE + compiler (`javac`), tools (`javadoc`, `jar`, debugger) | Required to **develop** Java programs |

👉 JVM is platform-dependent (different implementation per OS), which is why Java is "**Write Once, Run Anywhere**" — the same bytecode runs on any JVM.

---

## 2️⃣ Java Execution Flow

```
.java file
    ↓  (compiled by javac)
.class file (bytecode)
    ↓  (loaded by Class Loader)
JVM (Runtime Data Areas)
    ↓  (executed by Execution Engine)
Machine Code (via Interpreter / JIT Compiler)
```

1. `javac` compiles `.java` → `.class` (platform-independent bytecode).
2. JVM's **Class Loader** loads `.class` files into memory.
3. **Bytecode Verifier** checks the bytecode for security/correctness.
4. **Execution Engine** interprets or JIT-compiles bytecode into native machine code.

---

## 3️⃣ JVM Architecture — Major Components

```
┌─────────────────────────────────────────┐
│              Class Loader                │
│  (Loading → Linking → Initialization)    │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│           Runtime Data Areas             │
│  Method Area | Heap | Stack (per thread) │
│  PC Register | Native Method Stack       │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│            Execution Engine              │
│   Interpreter | JIT Compiler | GC        │
└─────────────────────────────────────────┘
```

---

## 4️⃣ Class Loading — 3 Phases

### a) Loading

- Reads `.class` file bytes and creates a `Class` object in memory.
- Done by 3 built-in class loaders, in a **parent-delegation hierarchy**:

```
Bootstrap ClassLoader   → loads core JDK classes (java.lang.*, rt.jar)
        ↓
Extension/Platform ClassLoader → loads extension libraries
        ↓
Application/System ClassLoader → loads classes from classpath (your code)
```

**Parent Delegation Model**: A class loader first asks its **parent** to load the class before trying itself. This prevents duplicate loading of core classes and protects against overriding trusted classes like `java.lang.String`.

### b) Linking

1. **Verification** — bytecode verifier checks the class file format and correctness (no illegal jumps, correct types, etc.).
2. **Preparation** — memory is allocated for static variables, initialized with **default** values (`0`, `null`, `false`).
3. **Resolution** — symbolic references (class/method/field names) are resolved to actual memory references.

### c) Initialization

- Static variables get their **actual assigned values**.
- Static blocks execute, in the order they appear.

```java
class Test {
    static int x = 10; // assigned during Initialization
    static {
        System.out.println("Static block");
    }
}
```

---

## 5️⃣ Runtime Data Areas (Memory Areas)

| Area | Shared/Per-Thread | Stores |
|------|---------------------|--------|
| **Method Area (Metaspace since Java 8)** | Shared | Class metadata, static variables, constant pool |
| **Heap** | Shared | All objects and instance variables |
| **Stack** | Per-thread | Method call frames, local variables, partial results |
| **PC Register** | Per-thread | Address of the currently executing instruction |
| **Native Method Stack** | Per-thread | Native (non-Java, e.g. C/C++) method calls |

---

## 6️⃣ Stack vs Heap

| Aspect | Stack | Heap |
|--------|-------|------|
| Stores | Local variables, method call frames, references | Objects, instance variables |
| Memory allocation | Fast (LIFO push/pop) | Slower (managed by Garbage Collector) |
| Scope | Per-thread (each thread has its own stack) | Shared across all threads |
| Lifetime | Cleared when method returns | Lives until no longer referenced (GC eligible) |
| Size | Smaller, can cause `StackOverflowError` | Larger, can cause `OutOfMemoryError` |
| Example | `int x = 5;` inside a method | `new Employee()` |

```java
void method() {
    int x = 10;              // x lives on the Stack
    Employee e = new Employee(); // reference 'e' on Stack, actual object on Heap
}
```

```
Stack Frame (method())
┌──────────────┐
│ x = 10       │
│ e = 0x1A2B  ─┼────► Heap: Employee object { ... }
└──────────────┘
```

---

## 🔥 Interview Questions

1. What is the difference between JVM, JRE, and JDK?
2. Explain the class loading process — Loading, Linking, Initialization.
3. What is the Parent Delegation Model and why is it important?
4. What causes `StackOverflowError` vs `OutOfMemoryError`?
5. Where are static variables stored — Stack or Heap? (Method Area / Metaspace)
6. Why is Java called "platform independent" despite the JVM itself being platform-dependent?

---

## 🎯 Summary

✔ JDK ⊃ JRE ⊃ JVM  
✔ Class loading: Loading → Linking (Verify, Prepare, Resolve) → Initialization  
✔ Parent Delegation Model avoids duplicate/unsafe class loading  
✔ Stack → method frames & locals (per-thread); Heap → objects (shared)
