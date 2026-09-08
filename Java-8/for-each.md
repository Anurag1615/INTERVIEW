# forEach() Method (Java 8)

forEach() is used to process each element of collection.

Available in:
Iterable interface

So all collections support it.

---

## Traditional Approach

```java
ArrayList<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

for(String s : list){
    System.out.println(s);
}
```

---

## Using forEach() + Lambda

```java
list.forEach(s -> System.out.println(s));
```

---

## Using Method Reference

```java
list.forEach(System.out::println);
```

More readable ✔

---

## Important Point

forEach() takes Consumer functional interface.

Definition:

void accept(T t)

---

## Example — Square numbers

```java
list.forEach(i -> System.out.println(i*i));
```

---

## Difference: stream().forEach vs collection.forEach

| collection.forEach | stream.forEach |
|------|------|
| simple iteration | stream processing |
| sequential | may be parallel |
| no intermediate ops | supports filter/map |
