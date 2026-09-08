# Java 8 Streams – Interview Coding Questions

---

## 1. Even Numbers from List

```java
List<Integer> list = Arrays.asList(10,15,20,25,30,35);

List<Integer> even = list.stream()
                         .filter(i -> i%2==0)
                         .collect(Collectors.toList());

System.out.println(even);
```

---

## 2. Square each number

```java
List<Integer> square = list.stream()
                           .map(i -> i*i)
                           .collect(Collectors.toList());
```

---

## 3. Count even numbers

```java
long count = list.stream()
                 .filter(i -> i%2==0)
                 .count();
```

---

## 4. Find Max number

```java
Integer max = list.stream()
                  .max(Integer::compare)
                  .get();
```

---

## 5. Find Min number

```java
Integer min = list.stream()
                  .min(Integer::compare)
                  .get();
```

---

## 6. Sort ascending

```java
list.stream()
    .sorted()
    .forEach(System.out::println);
```

---

## 7. Sort descending

```java
list.stream()
    .sorted((a,b)->b-a)
    .forEach(System.out::println);
```

---

## 8. Remove duplicates

```java
list.stream()
    .distinct()
    .forEach(System.out::println);
```

---

## 9. Sum of all numbers

```java
int sum = list.stream()
              .reduce(0,(a,b)->a+b);
```

---

## 10. Average of numbers

```java
double avg = list.stream()
                 .mapToInt(i->i)
                 .average()
                 .getAsDouble();
```

---

## 11. Convert List<Integer> → List<String>

```java
List<String> s = list.stream()
                     .map(String::valueOf)
                     .collect(Collectors.toList());
```

---

## 12. Filter strings starting with A

```java
List<String> names = Arrays.asList("Anurag","Durga","Amit","Rahul");

names.stream()
     .filter(s->s.startsWith("A"))
     .forEach(System.out::println);
```

---

## 13. Find length of each string

```java
names.stream()
     .map(String::length)
     .forEach(System.out::println);
```

---

## 14. Join strings

```java
String result = names.stream()
                     .collect(Collectors.joining(","));
```

---

## 15. Find first element

```java
names.stream()
     .findFirst()
     .ifPresent(System.out::println);
```

---

## 16. Any match

```java
boolean exists = names.stream()
                      .anyMatch(s->s.equals("Durga"));
```

---

## 17. All match

```java
boolean all = names.stream()
                   .allMatch(s->s.length()>2);
```

---

## 18. None match

```java
boolean none = names.stream()
                    .noneMatch(s->s.startsWith("Z"));
```

---

## 19. Group numbers (Even/Odd)

```java
Map<Boolean,List<Integer>> map =
list.stream()
    .collect(Collectors.partitioningBy(i->i%2==0));
```

---

## 20. Frequency of characters in string

```java
String str="aabbccc";

Map<Character,Long> freq =
str.chars()
   .mapToObj(c->(char)c)
   .collect(Collectors.groupingBy(c->c,Collectors.counting()));
```

---

## 21. Highest salary employee

```java
Employee e = employees.stream()
                      .max(Comparator.comparing(Employee::getSalary))
                      .get();
```

---

## 22. Second highest number

```java
Integer secondHighest =
list.stream()
    .sorted(Comparator.reverseOrder())
    .skip(1)
    .findFirst()
    .get();
```

---

## 23. Flatten list of lists

```java
List<List<Integer>> list2 = Arrays.asList(
        Arrays.asList(1,2),
        Arrays.asList(3,4),
        Arrays.asList(5,6)
);

List<Integer> flat =
list2.stream()
     .flatMap(Collection::stream)
     .collect(Collectors.toList());
```

---

## 24. Find duplicate elements

```java
Set<Integer> dup = list.stream()
    .filter(i->Collections.frequency(list,i)>1)
    .collect(Collectors.toSet());
```

---

## 25. First non-repeated character

```java
String str="aabbcddee";

Character ch = str.chars()
    .mapToObj(c->(char)c)
    .collect(Collectors.groupingBy(c->c,LinkedHashMap::new,Collectors.counting()))
    .entrySet()
    .stream()
    .filter(e->e.getValue()==1)
    .map(Map.Entry::getKey)
    .findFirst()
    .get();
```

# Employee Based Stream Problems (Very Important)

> Dataset: List<Employe> employees = Employe.employeeData();

---

## 1. Minimum Salary Employee

```java
Employe minSalaryEmp =
employees.stream()
         .min(Comparator.comparingDouble(e -> e.salary))
         .orElse(null);

System.out.println(minSalaryEmp);
```

---

## 2. Maximum Salary Employee

```java
Employe max =
employees.stream()
         .max(Comparator.comparingDouble(e -> e.salary))
         .orElse(null);

System.out.println(max);
```

---

## 3. Separate Even and Odd Employee Id

```java
System.out.println("Even Id Employees");
employees.stream()
         .filter(e -> e.empId % 2 == 0)
         .forEach(System.out::println);

System.out.println("Odd Id Employees");
employees.stream()
         .filter(e -> e.empId % 2 != 0)
         .forEach(System.out::println);
```

---

## 4. Department Wise Count

```java
employees.stream()
         .collect(Collectors.groupingBy(
                 e -> e.dept,
                 Collectors.counting()))
         .forEach((k,v)->System.out.println(k+" "+v));
```

---

## 5. Company Wise Count

```java
employees.stream()
         .collect(Collectors.groupingBy(
                 e -> e.company,
                 Collectors.counting()))
         .forEach((k,v)->System.out.println("company : "+k+" employees : "+v));
```

---

## 6. Highest Paying Company

```java
String company =
employees.stream()
         .max(Comparator.comparingDouble(e -> e.salary))
         .get().company;

System.out.println(company);
```

---

## 7. Average Salary Company Wise

```java
employees.stream()
         .collect(Collectors.groupingBy(
                 e -> e.company,
                 Collectors.averagingDouble(Employe::getSalary)))
         .entrySet().stream()
         .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
         .forEach(e->System.out.println(e.getKey()+" "+e.getValue()));
```

---

## 8. Department With Maximum Salary Employee

```java
employees.stream()
.collect(Collectors.groupingBy(
        e -> e.dept,
        Collectors.maxBy(Comparator.comparingDouble(i->i.salary))))
.entrySet().stream()
.max(Comparator.comparingDouble(e->e.getValue().get().salary))
.ifPresent(System.out::println);
```

---

## 9. Salary Sum Department Wise

```java
employees.stream()
.collect(Collectors.groupingBy(
        Employe::getDept,
        Collectors.summingDouble(Employe::getSalary)))
.forEach((k,v)->System.out.println(k+" "+v));
```

---

## 10. Average Salary Department Wise

```java
employees.stream()
.collect(Collectors.groupingBy(
        Employe::getDept,
        Collectors.averagingDouble(Employe::getSalary)))
.entrySet().stream()
.sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
.forEach(e->System.out.println(e.getKey()+" "+e.getValue()));
```

---

## 11. Max Salary Location Wise

```java
employees.stream()
.collect(Collectors.groupingBy(
        Employe::getLocation,
        Collectors.maxBy(Comparator.comparingDouble(Employe::getSalary))))
.forEach((k,v)->System.out.println(k+" "+v));
```

---

## 12. Group Employees By Location

```java
employees.stream()
.collect(Collectors.groupingBy(Employe::getLocation))
.forEach((k,v)->System.out.println(k+" "+v));
```

