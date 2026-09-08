# Streams API (Java 8)

Stream is used to process objects from collection.

Important:
Stream does NOT store data.
Stream only processes data.

Collection → stores data
Stream → processes data

---

## Why Streams?

To process collections in easy and readable way.

Before Java 8:

```java
ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
list.add(25);
list.add(5);
list.add(30);

ArrayList<Integer> l2 = new ArrayList<>();

for(Integer i : list){
    if(i % 2 == 0){
        l2.add(i);
    }
}

System.out.println(l2);
```

Too much code 😑

---

## After Java 8

```java
ArrayList<Integer> list = new ArrayList<>();

list.add(10);
list.add(20);
list.add(25);
list.add(5);
list.add(30);

List<Integer> l2 = list.stream()
                       .filter(i -> i%2==0)
                       .collect(Collectors.toList());

System.out.println(l2);
```

Less code ✔  
Readable ✔

---

## What is Stream?

Stream is a sequence of elements supporting functional operations.

---

## How to get Stream?

```java
list.stream();
```

---

## Stream Pipeline

Stream operations are divided into 3 parts:

1. Source
2. Intermediate Operation
3. Terminal Operation

Example:

list.stream().filter(i->i%2==0).collect(Collectors.toList());

| Part | Meaning |
|----|----|
| list | Source |
| filter | Intermediate |
| collect | Terminal |

---

## Important Point

Intermediate methods return Stream → Lazy execution  
Terminal method returns result → Execution starts

So processing starts only after terminal operation.



# Streams Operations (filter, map, collect, count)

---

## filter()

Used to filter elements based on condition.

Returns Stream.

### Example — Even numbers

```java
ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.add(15);
list.add(20);
list.add(25);
list.add(30);

List<Integer> l = list.stream()
                      .filter(i -> i%2==0)
                      .collect(Collectors.toList());

System.out.println(l);
```

Output:
[10, 20, 30]

---

## map()

Used to perform operation on each element.

### Example — Square each number

```java
List<Integer> l = list.stream()
                      .map(i -> i*i)
                      .collect(Collectors.toList());

System.out.println(l);
```

---

### Example — Add 5 to each element

```java
List<Integer> l = list.stream()
                      .map(i -> i+5)
                      .collect(Collectors.toList());
```

---

## count()

Returns number of elements.

```java
long count = list.stream()
                 .filter(i -> i%2==0)
                 .count();

System.out.println(count);
```

---

## collect()

Used to convert Stream → Collection

Most common:

```java
Collectors.toList()
Collectors.toSet()
```

---

## Chaining Operations

```java
List<Integer> l = list.stream()
                      .filter(i -> i%2==0)
                      .map(i -> i*i)
                      .collect(Collectors.toList());
```

Step-by-step:

1. Filter even numbers
2. Square them
3. Store in list

---

## Important Interview Point

filter → condition check  
map → transformation




# Streams Sorting, Min, Max, forEach, toArray

---

## sorted()

Used to sort elements.

### Natural Sorting (Ascending)

```java
List<Integer> l = list.stream()
                      .sorted()
                      .collect(Collectors.toList());

System.out.println(l);
```

---

### Custom Sorting (Descending)

```java
List<Integer> l = list.stream()
                      .sorted((i1,i2) -> i2.compareTo(i1))
                      .collect(Collectors.toList());

System.out.println(l);
```

---

## min()

Returns minimum element.

```java
Integer min = list.stream()
                  .min((i1,i2)-> i1.compareTo(i2))
                  .get();

System.out.println(min);
```

---

## max()

Returns maximum element.

```java
Integer max = list.stream()
                  .max((i1,i2)-> i1.compareTo(i2))
                  .get();

System.out.println(max);
```

---

## forEach()

Used to process each element.

```java
list.stream().forEach(i -> System.out.println(i));
```

---

## toArray()

Convert stream to array.

```java
Integer[] arr = list.stream().toArray(Integer[]::new);
```

---

## Important Notes

sorted() → intermediate method  
min(), max(), forEach(), toArray() → terminal methods



# Streams reduce(), Parallel Stream & Performance

---

## reduce()

Used to combine all elements and produce single result.

General form:

result = list[0] op list[1] op list[2] op list[3] ...

---

### Example — Sum of elements

```java
ArrayList<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
list.add(30);
list.add(40);

Integer sum = list.stream()
                  .reduce(0, (a,b) -> a+b);

System.out.println(sum);
```

---

### Example — Multiplication

```java
Integer product = list.stream()
                      .reduce(1, (a,b) -> a*b);

System.out.println(product);
```

---

## reduce vs collect

| reduce | collect |
|------|------|
| returns single value | returns collection |
| used for calculation | used for grouping |

---

## Parallel Stream

Stream normally runs on single thread.

Parallel stream runs on multiple threads.

```java
list.parallelStream().forEach(System.out::println);
```

---

## When to use parallel stream?

Use when:
- large data
- independent operations

Do NOT use when:
- order important
- small data

---

## Important Interview Question

Difference between stream() and parallelStream()

| stream() | parallelStream() |
|------|------|
| single thread | multiple threads |
| predictable order | unpredictable order |
| safe | may cause race conditions |

---

## Performance Concept

Parallel stream uses ForkJoinPool internally.

So multiple CPU cores used.

---

## Key Point

Parallel stream is not always faster.

For small data → normal stream faster
For big data → parallel faster
