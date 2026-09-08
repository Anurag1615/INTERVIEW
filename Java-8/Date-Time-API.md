# Date & Time API (java.time) — Java 8

---

## 🔹 Why a New Date/Time API?

The old `java.util.Date` and `java.util.Calendar` classes had several well-known problems:

- **Mutable** — not thread-safe, dangerous to share across threads.
- Confusing APIs — months are 0-indexed (`Calendar.JANUARY == 0`), years are offset by 1900.
- No clear separation between date, time, and date-time concepts.

Java 8 introduced `java.time` package (inspired by the Joda-Time library), providing an **immutable, thread-safe** date/time API.

---

## 🔹 Core Classes

| Class | Represents |
|-------|-------------|
| `LocalDate` | Date only (no time), e.g., `2026-09-08` |
| `LocalTime` | Time only (no date), e.g., `14:30:00` |
| `LocalDateTime` | Date + Time, no timezone |
| `ZonedDateTime` | Date + Time + Timezone |
| `Instant` | A point in time on the UTC timeline (machine timestamp) |
| `Duration` | Time-based amount (hours, minutes, seconds) |
| `Period` | Date-based amount (years, months, days) |

---

## 🔹 `LocalDate`

```java
LocalDate today = LocalDate.now();
LocalDate specificDate = LocalDate.of(2026, 9, 8);

LocalDate nextWeek = today.plusWeeks(1);
LocalDate lastMonth = today.minusMonths(1);

System.out.println(today.getDayOfWeek()); // e.g., TUESDAY
System.out.println(today.isLeapYear());   // true/false
```

---

## 🔹 `LocalTime`

```java
LocalTime now = LocalTime.now();
LocalTime specificTime = LocalTime.of(14, 30, 0);

LocalTime later = now.plusHours(2);
```

---

## 🔹 `LocalDateTime`

```java
LocalDateTime dateTime = LocalDateTime.now();
LocalDateTime specific = LocalDateTime.of(2026, 9, 8, 14, 30);

LocalDateTime combined = LocalDate.now().atTime(LocalTime.now());
```

---

## 🔹 `ZonedDateTime`

```java
ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("Asia/Kolkata"));
System.out.println(zdt); // 2026-09-08T14:30:00+05:30[Asia/Kolkata]

ZonedDateTime nyTime = zdt.withZoneSameInstant(ZoneId.of("America/New_York"));
```

---

## 🔹 `Instant` (Machine Timestamp)

```java
Instant now = Instant.now(); // point on UTC timeline, useful for logging/measuring elapsed time
Instant later = now.plusSeconds(3600);

long epochMillis = now.toEpochMilli();
```

---

## 🔹 `Duration` vs `Period`

| Aspect | `Duration` | `Period` |
|--------|-------------|----------|
| Represents | Time-based amount (seconds, nanos) | Date-based amount (years, months, days) |
| Used with | `LocalTime`, `Instant`, `LocalDateTime` | `LocalDate` |
| Example | `Duration.ofHours(2)` | `Period.ofDays(10)` |

```java
Duration duration = Duration.between(LocalTime.of(9, 0), LocalTime.of(17, 0));
System.out.println(duration.toHours()); // 8

Period period = Period.between(LocalDate.of(2026, 1, 1), LocalDate.of(2026, 9, 8));
System.out.println(period.getMonths()); // 8
```

---

## 🔹 Formatting & Parsing

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd-MM-yyyy");

LocalDate date = LocalDate.now();
String formatted = date.format(formatter); // "08-09-2026"

LocalDate parsed = LocalDate.parse("08-09-2026", formatter);
```

---

## 🔹 Immutability — Every "Modification" Returns a New Object

```java
LocalDate date = LocalDate.of(2026, 1, 1);
date.plusDays(10); // ❌ this line does NOTHING useful — return value is discarded!

LocalDate newDate = date.plusDays(10); // ✔ correct — capture the new object
System.out.println(date);    // 2026-01-01 (unchanged)
System.out.println(newDate); // 2026-01-11
```

This is the same immutability principle used by `String` — every "modifier" method actually returns a brand new object.

---

## 🔹 Old API vs New API

| Aspect | `java.util.Date` / `Calendar` (Old) | `java.time` (New, Java 8+) |
|--------|----------------------------------------|------------------------------|
| Mutability | Mutable | Immutable |
| Thread Safety | Not thread-safe | Thread-safe |
| Month indexing | 0-based (confusing) | 1-based (`JANUARY = 1`) |
| API clarity | Poor, error-prone | Clear, fluent, readable |
| Timezone handling | `Calendar` clunky | `ZonedDateTime` explicit and clean |

---

## 🔥 Interview Questions

1. Why did Java 8 introduce a new Date-Time API instead of fixing `java.util.Date`?
2. What's the difference between `LocalDateTime` and `ZonedDateTime`?
3. What's the difference between `Duration` and `Period`?
4. Why is `Instant` useful compared to `LocalDateTime`?
5. Why must you always capture the return value of methods like `plusDays()`?

---

## 🎯 Summary

✔ `java.time` API is immutable and thread-safe (unlike old `Date`/`Calendar`)  
✔ `LocalDate`/`LocalTime`/`LocalDateTime` → no timezone; `ZonedDateTime` → with timezone  
✔ `Instant` → machine timestamp on UTC timeline  
✔ `Duration` → time-based amount; `Period` → date-based amount
