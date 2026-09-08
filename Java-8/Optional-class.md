# Optional Class (Java 8)

Introduced to avoid NullPointerException.

It is a container object which may or may not contain value.

Package:
java.util.Optional

---

## Problem Before Java 8

```java
String name = getName();
System.out.println(name.length());
```

If name = null → NullPointerException ❌

---

## Using Optional

```java
Optional<String> name = Optional.ofNullable(getName());

System.out.println(name.orElse("No Name"));
```

Now safe ✔

---

## Creating Optional Object

### 1. of()

Used when value definitely NOT null

```java
Optional<String> op = Optional.of("Durga");
```

If null → NullPointerException

---

### 2. ofNullable()

Used when value may be null

```java
Optional<String> op = Optional.ofNullable(null);
```

Safe ✔

---

### 3. empty()

Creates empty Optional

```java
Optional<String> op = Optional.empty();
```

---

## Important Methods

### isPresent()

```java
if(op.isPresent()){
    System.out.println(op.get());
}
```

---

### get()

Returns value if present  
Otherwise → NoSuchElementException

---

### orElse()

Return value if present  
Otherwise return default value

```java
System.out.println(op.orElse("Guest"));
```

---

### orElseGet()

Takes Supplier

```java
System.out.println(op.orElseGet(() -> "Default Name"));
```

---

### orElseThrow()

Throws exception if value absent

```java
op.orElseThrow(() -> new RuntimeException("Value missing"));
```

---

### ifPresent()

Executes Consumer if value exists

```java
op.ifPresent(System.out::println);
```

---

## Important Interview Question

Difference between orElse and orElseGet

| orElse | orElseGet |
|------|------|
| always creates object | creates only if needed |
| slower | faster |

---

## Key Point

Optional is NOT for fields and parameters  
It is mainly for return types.
