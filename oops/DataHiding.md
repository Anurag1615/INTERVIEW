# 🔹 Data Hiding

## 🔹 Definition

Data hiding is the process of restricting direct access to the internal data of a class.

The internal representation of data should not be exposed directly to the outside world.

---

## 🔹 Why Data Hiding?

- To protect data from unauthorized access
- To improve security
- To maintain control over internal state

---

## 🔹 How to Achieve Data Hiding?

By declaring instance variables as `private`.

```java
class Student {

    private int marks;
}
```

Now direct access is not allowed:

```java
Student s = new Student();
s.marks = 90;   // Compile Time Error
```

---

## 🔹 Proper Access Using Getter and Setter

```java
class Student {

    private int marks;

    public void setMarks(int m) {
        marks = m;
    }

    public int getMarks() {
        return marks;
    }
}
```

Data is accessed in a controlled way.

---

## 🔹 Important Point

Data hiding focuses only on:

Protecting internal data.

It does not focus on implementation details.
