# Annotations & Reflection

---

## 1️⃣ Annotations

## 🔹 Definition

An annotation is **metadata** attached to code (classes, methods, fields, parameters) that provides information to the compiler, tools, or runtime — without directly changing program logic itself.

```java
@Override
void method() { }

@Deprecated
void oldMethod() { }

@SuppressWarnings("unchecked")
void method2() { }
```

### Built-in Annotations

| Annotation | Purpose |
|------------|---------|
| `@Override` | Tells compiler the method overrides a parent method (compile-time check) |
| `@Deprecated` | Marks an element as outdated / discouraged |
| `@SuppressWarnings` | Instructs compiler to ignore specific warnings |
| `@FunctionalInterface` | Ensures interface has exactly one abstract method |
| `@SafeVarargs` | Suppresses unchecked warnings for varargs of generic type |

---

## 🔹 Custom (Meta) Annotations

You can create your own annotation type using `@interface`.

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME) // available at runtime via reflection
@Target(ElementType.METHOD)         // can only be applied to methods
public @interface LogExecutionTime {
}
```

Usage:

```java
class Service {
    @LogExecutionTime
    void process() {
        System.out.println("Processing...");
    }
}
```

### Meta-Annotations (Annotations on Annotations)

| Meta-Annotation | Purpose |
|-------------------|---------|
| `@Retention` | How long the annotation is retained: `SOURCE`, `CLASS`, or `RUNTIME` |
| `@Target` | Where it can be applied: `TYPE`, `METHOD`, `FIELD`, `PARAMETER`, etc. |
| `@Inherited` | Allows subclasses to inherit the annotation from a parent class |
| `@Documented` | Includes annotation info in generated Javadoc |

### Retention Policies

| Policy | Availability |
|--------|---------------|
| `SOURCE` | Discarded by compiler — only available in source code (e.g., `@Override`) |
| `CLASS` | Stored in `.class` file, but not available at runtime (default) |
| `RUNTIME` | Available at runtime via **Reflection** — needed for frameworks like Spring |

---

## 2️⃣ Reflection

## 🔹 Definition

Reflection is the ability of a Java program to **inspect and manipulate classes, methods, fields, and constructors at runtime**, even if their names/types weren't known at compile time.

Package: `java.lang.reflect`

### Getting Class Metadata

```java
class Employee {
    private int id;
    public String name;

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    private void privateMethod() {
        System.out.println("private method called");
    }
}

Class<?> clazz = Employee.class;
// or: Class<?> clazz = Class.forName("Employee");
// or: Class<?> clazz = employeeObj.getClass();

System.out.println(clazz.getName());          // Employee
System.out.println(clazz.getFields().length); // public fields only
```

### Accessing Private Fields/Methods (Breaking Encapsulation!)

```java
Employee emp = new Employee(1, "Raj");

Field field = Employee.class.getDeclaredField("id");
field.setAccessible(true); // bypass private access check
int id = (int) field.get(emp);
System.out.println(id); // 1

Method method = Employee.class.getDeclaredMethod("privateMethod");
method.setAccessible(true);
method.invoke(emp); // calls the private method
```

### Creating Objects Dynamically

```java
Constructor<?> constructor = Employee.class.getConstructor(int.class, String.class);
Employee emp2 = (Employee) constructor.newInstance(2, "Priya");
```

### Reading Annotations at Runtime

```java
Method m = Service.class.getMethod("process");
if (m.isAnnotationPresent(LogExecutionTime.class)) {
    System.out.println("This method should be timed!");
}
```

---

## 🔹 Where Is Reflection Used in Real Projects?

- **Frameworks**: Spring (Dependency Injection, `@Autowired`), Hibernate (mapping entities to tables), JUnit (discovering `@Test` methods).
- **Serialization libraries**: Jackson/Gson use reflection to read/write object fields as JSON.
- **Dependency injection containers** need to inspect constructors/fields at runtime.

---

## 🔹 Downsides of Reflection

- **Performance overhead** — slower than direct method calls (no JIT optimization for reflective calls in the same way).
- **Breaks encapsulation** — `setAccessible(true)` can access private members, violating OOP principles.
- **No compile-time safety** — errors surface only at runtime (`NoSuchMethodException`, `IllegalAccessException`).

---

## 🔥 Interview Questions

1. What is the difference between `RetentionPolicy.SOURCE`, `CLASS`, and `RUNTIME`?
2. How does Spring use annotations and reflection together (e.g., `@Autowired`)?
3. How can you access a `private` field of a class using reflection?
4. What are the performance/security downsides of using Reflection?
5. How would you create a custom annotation and read it at runtime?

---

## 🎯 Summary

✔ Annotations = metadata for compiler/tools/runtime, defined via `@interface`  
✔ `@Retention(RUNTIME)` is required for an annotation to be readable via Reflection  
✔ Reflection inspects/manipulates classes, fields, methods at runtime  
✔ Powers frameworks like Spring, Hibernate, JUnit, Jackson — but comes with performance and encapsulation trade-offs
