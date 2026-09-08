# 🧠 Iterator Design Pattern (Easy + Interview Ready)

---

## 🚀 Why You Should Care

When you work with collections (List, Array, Stack, etc.), you often need to:

- Traverse elements  
- Access items one by one  

👉 Without exposing internal structure  

👉 Iterator Pattern solves this problem

---

## 🎯 Definition (Simple)

> Iterator Pattern provides a way to access elements of a collection sequentially without exposing its internal structure.

---

## 🤯 Problem (Without Iterator)

### Example

```java
class MyCollection {
    int[] arr = {1, 2, 3, 4};

    public int[] getItems() {
        return arr;
    }
}
```

### ❌ Usage

```java
MyCollection collection = new MyCollection();
int[] items = collection.getItems();

for (int i = 0; i < items.length; i++) {
    System.out.println(items[i]);
}
```

### ❌ Problems

- Exposes internal structure (array)  
- If structure changes → client breaks  
- Tight coupling  

---

## 💡 Solution (Iterator Pattern)

👉 Hide internal structure  
👉 Provide a separate **Iterator object**

---

## 🧩 Structure

- **Iterator (Interface)** → traversal methods  
- **Concrete Iterator** → actual traversal logic  
- **Aggregate (Collection)** → creates iterator  
- **Concrete Aggregate** → actual collection  

---

## 🧑‍💻 Code Implementation

---

### 1️⃣ Iterator Interface

```java
interface MyIterator {
    boolean hasNext();
    int next();
}
```

---

### 2️⃣ Collection (Aggregate)

```java
class MyCollection {

    private int[] arr = {1, 2, 3, 4};

    public MyIterator createIterator() {
        return new MyIteratorImpl(arr);
    }
}
```

---

### 3️⃣ Concrete Iterator

```java
class MyIteratorImpl implements MyIterator {

    private int[] arr;
    private int index = 0;

    public MyIteratorImpl(int[] arr) {
        this.arr = arr;
    }

    public boolean hasNext() {
        return index < arr.length;
    }

    public int next() {
        return arr[index++];
    }
}
```

---

### 4️⃣ Client Code

```java
public class Main {
    public static void main(String[] args) {

        MyCollection collection = new MyCollection();
        MyIterator iterator = collection.createIterator();

        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

---

## 🔄 Execution Flow

```text
Collection → Iterator → Client
                ↓
          Traverse elements
```

---

## 🎯 Key Idea

> Client does NOT know internal structure  
> It only uses:

```java
iterator.hasNext();
iterator.next();
```

---

## ✅ Advantages

- Encapsulation ✔️  
- Uniform traversal ✔️  
- Works with different data structures ✔️  
- Cleaner code ✔️  

---

## ❌ Disadvantages

- Extra classes 😵  
- Slight overhead  

---

## 🧠 When to Use

- When you need to traverse a collection  
- When you want to hide internal structure  
- When multiple traversal strategies are needed  

---

## 🆚 Iterator vs For Loop

| Feature | For Loop | Iterator |
|--------|---------|----------|
| Encapsulation | ❌ | ✅ |
| Flexibility | Low ❌ | High ✅ |
| Structure dependency | High ❌ | Low ✅ |

---

## 💥 Real World Example

👉 Java Collections

```java
List<Integer> list = Arrays.asList(1, 2, 3);

Iterator<Integer> it = list.iterator();

while (it.hasNext()) {
    System.out.println(it.next());
}
```

👉 This is built-in Iterator Pattern

---

## 🧠 Interview One-Liner

> “Iterator pattern provides a way to traverse a collection without exposing its internal structure.”

---

## 🚀 Easy Memory Trick

👉 **Iterator = Traversal without exposing structure**

---
