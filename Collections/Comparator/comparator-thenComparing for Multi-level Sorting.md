# Java’s `thenComparing` for Multi-level Sorting

Sorting in Java becomes powerful when you can **compare multiple fields in sequence**, just like SQL’s `ORDER BY` clause. This is where `Comparator` and its method `thenComparing` shine.

In this blog, we’ll explore:

* What `thenComparing` does
* All input types it accepts
* Practical examples
* Tips for multi-field sorting

---

## What `thenComparing` Does

`thenComparing` is used **after a primary comparator**.

* If the **primary comparator returns 0** (tie), the next comparator is used.
* You can chain **multiple `thenComparing` calls** for multi-level sorting.

Example:

```java
Comparator<Employee> cmp = Comparator.comparing(Employee::getName)
                                     .thenComparing(Employee::getSalary)
                                     .thenComparing(Employee::getDepartment);
```

This will sort by **name**, then **salary**, then **department** if the previous fields are equal.

---

## Input Types for `thenComparing`

`thenComparing` is **overloaded**, meaning it accepts multiple input types:

### 1. Comparator

```java
employees.sort(
    Comparator.comparing(Employee::getName)
              .thenComparing(Comparator.comparing(Employee::getSalary).reversed())
);
```

* Input: `Comparator<? super T>`
* Use case: complex logic, descending, or custom comparison.

---

### 2. Key Extractor (Function)

```java
employees.sort(
    Comparator.comparing(Employee::getName)
              .thenComparing(Employee::getDepartment)
);
```

* Input: a function `Function<? super T, ? extends U>`
* Sorts **naturally** based on the field’s `Comparable` implementation.

---

### 3. Key Extractor + Comparator

```java
employees.sort(
    Comparator.comparing(Employee::getName)
              .thenComparing(Employee::getSalary, Comparator.reverseOrder())
);
```

* Useful for descending or custom logic on one field.

---

### 4. Primitive Comparators

Java provides specialized versions for efficiency:

```java
employees.sort(
    Comparator.comparingInt(Employee::getId)
              .thenComparingDouble(Employee::getSalary)
);
```

* `comparingInt`, `comparingDouble`, `comparingLong` for primitive types.

---

### 5. Lambdas

You can also use lambdas for more control:

```java
employees.sort(
    Comparator.comparing(Employee::getName)
              .thenComparing(e -> e.getSalary() % 1000) // custom logic
);
```

---

## Example: Multi-field Sorting

```java
employees.sort(
    Comparator.comparing(Employee::getName)                        // ASC
              .thenComparing(Comparator.comparingDouble(Employee::getSalary).reversed()) // DESC
              .thenComparing(Employee::getDepartment)              // ASC
              .thenComparing(Comparator.comparing(Employee::getRole).reversed())        // DESC
);
```

* Name ascending
* Salary descending
* Department ascending
* Role descending

---

## Key Notes

1. **Chaining order matters** — the first comparator is the most important.
2. Each `thenComparing` is used **only if all previous comparisons return 0**.
3. You can chain **any number** of comparators.
4. Combine **Comparator objects**, **method references**, **lambdas**, and **primitive comparators**.
5. `thenComparing` **does not mutate** the original comparator — it returns a new one.

---

## Summary Table

| Method Signature                                                         | Input Type              | Use Case                       |
| ------------------------------------------------------------------------ | ----------------------- | ------------------------------ |
| `thenComparing(Comparator<? super T>)`                                   | Comparator object       | Custom or complex comparison   |
| `thenComparing(Function<? super T, ? extends U>)`                        | Key extractor           | Natural ordering of a field    |
| `thenComparing(Function<? super T, ? extends U>, Comparator<? super U>)` | Key + custom comparator | Custom ordering for one field  |
| `thenComparingInt(ToIntFunction<? super T>)`                             | int field               | Efficient primitive comparison |
| `thenComparingDouble(ToDoubleFunction<? super T>)`                       | double field            | Efficient primitive comparison |
| `thenComparingLong(ToLongFunction<? super T>)`                           | long field              | Efficient primitive comparison |

---
