# Enums, Records & Sealed Classes

---

## 1️⃣ Enums

## 🔹 Definition

`enum` is a special data type that represents a **fixed set of constants**.

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

Day today = Day.MONDAY;
```

### Key Facts

- Internally, every `enum` implicitly extends `java.lang.Enum` — so it **cannot extend any other class** (Java has no multiple inheritance).
- Each constant is actually a `public static final` instance of the enum type.
- Enums **can** implement interfaces.
- Enums **can** have fields, constructors, and methods.

```java
enum Planet {
    MERCURY(3.3e23, 2.4e6),
    EARTH(5.9e24, 6.4e6);

    private final double mass;
    private final double radius;

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }

    double surfaceGravity() {
        double G = 6.67300E-11;
        return G * mass / (radius * radius);
    }
}

System.out.println(Planet.EARTH.surfaceGravity());
```

### Useful Built-in Methods

```java
Day.values();          // returns array of all constants
Day.valueOf("MONDAY"); // returns MONDAY constant, throws IllegalArgumentException if not found
Day.MONDAY.ordinal();  // position (0-based)
Day.MONDAY.name();     // "MONDAY"
```

### Enum in `switch`

```java
switch (today) {
    case MONDAY -> System.out.println("Start of week");
    case FRIDAY -> System.out.println("Almost weekend");
    default -> System.out.println("Midweek");
}
```

### Why Use Enum Instead of `public static final int` Constants?

- Type-safe (compiler prevents assigning an invalid value).
- Can have behavior (methods) attached.
- Readable, self-documenting (`Day.MONDAY` vs magic number `1`).

---

## 2️⃣ Records (Java 16+)

## 🔹 Definition

A `record` is a compact way to create an **immutable data-carrier class**, auto-generating boilerplate: constructor, `equals()`, `hashCode()`, `toString()`, and accessor methods.

```java
record Point(int x, int y) { }
```

The compiler automatically generates:

```java
// Equivalent hand-written class (simplified)
final class Point {
    private final int x;
    private final int y;

    Point(int x, int y) { this.x = x; this.y = y; }

    int x() { return x; }
    int y() { return y; }

    // equals(), hashCode(), toString() auto-generated
}
```

### Usage

```java
Point p1 = new Point(1, 2);
Point p2 = new Point(1, 2);

System.out.println(p1.x());           // 1 (note: accessor is x(), not getX())
System.out.println(p1.equals(p2));    // true (value-based equality)
System.out.println(p1);               // Point[x=1, y=2]
```

### Custom Validation (Compact Constructor)

```java
record Range(int min, int max) {
    Range {
        if (min > max) {
            throw new IllegalArgumentException("min must be <= max");
        }
    }
}
```

### Key Facts

- Fields are implicitly `private final`.
- Records are implicitly `final` (cannot be extended).
- Can implement interfaces, but cannot extend another class (already extends `java.lang.Record` implicitly).
- Best suited for simple, immutable DTO-style data.

---

## 3️⃣ Sealed Classes (Java 17+)

## 🔹 Definition

Sealed classes **restrict which classes/interfaces are allowed to extend or implement them**, giving more control over class hierarchies than plain `final` or open inheritance.

```java
sealed interface Shape permits Circle, Square, Rectangle { }

final class Circle implements Shape {
    double radius;
}

final class Square implements Shape {
    double side;
}

non-sealed class Rectangle implements Shape {
    double length, width;
    // 'non-sealed' re-opens this branch for further unrestricted extension
}
```

### Rules

- A `permits` clause explicitly lists allowed subclasses.
- Every permitted subclass must be declared as:
  - `final` — cannot be extended further, OR
  - `sealed` — further restricts its own subclasses, OR
  - `non-sealed` — reopens unrestricted extension from this point.

### Why Sealed Classes Matter

- Enables **exhaustive pattern matching** in `switch` — compiler knows all possible subtypes, so `default` case can become optional.

```java
static String describe(Shape shape) {
    return switch (shape) {
        case Circle c -> "Circle";
        case Square s -> "Square";
        case Rectangle r -> "Rectangle";
        // no default needed — compiler knows these are ALL possible subtypes
    };
}
```

- Provides a middle ground between:
  - Fully open inheritance (any class can extend) — hard to reason about all possibilities.
  - Fully closed/`final` (no inheritance at all) — too restrictive.

---

## 🔥 Interview Questions

1. Can an `enum` extend a class? Can it implement an interface?
2. What boilerplate does a `record` eliminate compared to a normal POJO/DTO class?
3. Can a `record` be extended? Why or why not?
4. What problem do sealed classes solve that plain inheritance doesn't?
5. What does `non-sealed` mean in a sealed class hierarchy?

---

## 🎯 Summary

✔ Enum → fixed set of type-safe constants, can have fields/methods, implicitly extends `Enum`  
✔ Record → concise immutable data carrier, auto-generates constructor/equals/hashCode/toString  
✔ Sealed classes → explicitly restrict which classes can extend/implement, enabling exhaustive pattern matching
