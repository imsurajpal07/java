# Java Stream Collectors: The Complete Reference Guide

---

## Introduction to Collectors

Java Stream Collectors are specialized reduction operations that accumulate stream elements into collections, summarize data, or perform complex transformations. Introduced in Java 8, they provide a declarative way to process data streams efficiently.

### Why Collectors Matter

- **Type Safety**: Compile-time guarantees for collection types
- **Performance**: Optimized implementations for common operations
- **Readability**: Clear, expressive code that reads like natural language
- **Composability**: Can be combined for complex data transformations
- **Immutability**: Often produce immutable results by default

### Basic Collector Anatomy

```java
Stream<T> stream = ...;
R result = stream.collect(Collector<T, A, R> collector);
```

Where:
- `T`: Type of stream elements
- `A`: Intermediate accumulation type
- `R`: Final result type

---

## Core Collection Collectors

### `toList()` - Collecting to ArrayList

**Purpose**: Accumulates stream elements into a mutable `ArrayList`.

```java
import java.util.stream.Collectors;
import java.util.List;

List<String> fruits = Stream.of("apple", "banana", "cherry")
    .collect(Collectors.toList());

System.out.println(fruits); 
// Output: [apple, banana, cherry]
// Type: ArrayList<String>
```

### `toSet()` - Collecting to HashSet

**Purpose**: Accumulates stream elements into a mutable `HashSet`, removing duplicates.

```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3);
Set<Integer> uniqueNumbers = numbers.stream()
    .collect(Collectors.toSet());

System.out.println(uniqueNumbers); 
// Output: [1, 2, 3] (order may vary)
// Type: HashSet<Integer>
```

### `toCollection(Supplier)` - Custom Collection Types

**Purpose**: Accumulates elements into a specified collection type.

```java
// Collect to LinkedList for insertion-order preservation
LinkedList<String> linkedList = Stream.of("first", "second", "third")
    .collect(Collectors.toCollection(LinkedList::new));

// Collect to TreeSet for sorted unique elements
TreeSet<Integer> sortedSet = Stream.of(5, 1, 3, 1, 4)
    .collect(Collectors.toCollection(TreeSet::new));

System.out.println(sortedSet); 
// Output: [1, 3, 4, 5]
```

### `toMap()` - Creating Maps

**Purpose**: Creates maps from stream elements using key and value extractors.

```java
// Simple key-value mapping
Map<String, Integer> lengthMap = Stream.of("cat", "elephant", "dog")
    .collect(Collectors.toMap(
        Function.identity(),    // key: the string itself
        String::length         // value: string length
    ));

System.out.println(lengthMap);
// Output: {cat=3, elephant=8, dog=3}

// Handling duplicate keys with merge function
List<String> words = Arrays.asList("apple", "apricot", "banana");
Map<Character, String> firstLetterMap = words.stream()
    .collect(Collectors.toMap(
        word -> word.charAt(0),           // key: first character
        Function.identity(),              // value: the word
        (existing, replacement) -> existing + ", " + replacement  // merge duplicates
    ));

System.out.println(firstLetterMap);
// Output: {a=apple, apricot, b=banana}
```

---

## Counting and Summarizing Collectors 

### `counting()` - Element Count

**Purpose**: Counts the number of elements in a stream.

```java
long count = Stream.of("a", "b", "c", "d")
    .collect(Collectors.counting());

System.out.println(count); 
// Output: 4
```

### `summingInt()` / `summingLong()` / `summingDouble()` - Numeric Summation

**Purpose**: Sums numeric values extracted from stream elements.

```java
class Product {
    private String name;
    private double price;
    
    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
    
    public double getPrice() { return price; }
    public String getName() { return name; }
}

List<Product> products = Arrays.asList(
    new Product("Laptop", 999.99),
    new Product("Mouse", 25.50),
    new Product("Keyboard", 75.00)
);

double totalPrice = products.stream()
    .collect(Collectors.summingDouble(Product::getPrice));

System.out.println("Total: $" + totalPrice);
// Output: Total: $1100.49
```

### `averagingInt()` / `averagingLong()` / `averagingDouble()` - Average Calculation

**Purpose**: Calculates the arithmetic mean of numeric values.

```java
OptionalDouble average = Stream.of(1, 2, 3, 4, 5)
    .collect(Collectors.averagingInt(Integer::intValue));

System.out.println("Average: " + average);
// Output: Average: 3.0
```

### `summarizingInt()` / `summarizingLong()` / `summarizingDouble()` - Complete Statistics

**Purpose**: Provides comprehensive statistics (count, sum, min, max, average).

```java
IntSummaryStatistics stats = products.stream()
    .collect(Collectors.summarizingDouble(Product::getPrice))
    .intValue(); // Note: This is for demonstration, normally use DoubleSummaryStatistics

// Better example:
DoubleSummaryStatistics priceStats = products.stream()
    .collect(Collectors.summarizingDouble(Product::getPrice));

System.out.println("Count: " + priceStats.getCount());
System.out.println("Sum: $" + priceStats.getSum());
System.out.println("Average: $" + priceStats.getAverage());
System.out.println("Min: $" + priceStats.getMin());
System.out.println("Max: $" + priceStats.getMax());

// Output:
// Count: 3
// Sum: $1100.49
// Average: $366.83
// Min: $25.5
// Max: $999.99
```

---

## Joining and String Collectors 

### `joining()` - String Concatenation

**Purpose**: Concatenates stream elements into a single string.

```java
// Simple joining
String result = Stream.of("Java", "is", "awesome")
    .collect(Collectors.joining());

System.out.println(result);
// Output: Javaisawesome

// Joining with delimiter
String withSpaces = Stream.of("Java", "is", "awesome")
    .collect(Collectors.joining(" "));

System.out.println(withSpaces);
// Output: Java is awesome

// Joining with delimiter, prefix, and suffix
String formatted = Stream.of("apple", "banana", "cherry")
    .collect(Collectors.joining(", ", "[", "]"));

System.out.println(formatted);
// Output: [apple, banana, cherry]
```

---

## Grouping and Partitioning Collectors 

### `groupingBy()` - Grouping Elements

**Purpose**: Groups stream elements by a classifier function.

```java
class Person {
    private String name;
    private String city;
    private int age;
    
    public Person(String name, String city, int age) {
        this.name = name;
        this.city = city;
        this.age = age;
    }
    
    // Getters
    public String getName() { return name; }
    public String getCity() { return city; }
    public int getAge() { return age; }
    
    @Override
    public String toString() {
        return name + "(" + age + ")";
    }
}

List<Person> people = Arrays.asList(
    new Person("Alice", "New York", 25),
    new Person("Bob", "New York", 30),
    new Person("Charlie", "London", 35),
    new Person("David", "London", 28)
);

// Simple grouping
Map<String, List<Person>> peopleByCity = people.stream()
    .collect(Collectors.groupingBy(Person::getCity));

System.out.println(peopleByCity);
// Output: {New York=[Alice(25), Bob(30)], London=[Charlie(35), David(28)]}

// Grouping with downstream collector
Map<String, Long> countByCity = people.stream()
    .collect(Collectors.groupingBy(
        Person::getCity,
        Collectors.counting()
    ));

System.out.println(countByCity);
// Output: {New York=2, London=2}

// Multi-level grouping
Map<String, Map<String, List<Person>>> grouped = people.stream()
    .collect(Collectors.groupingBy(
        Person::getCity,
        Collectors.groupingBy(person -> person.getAge() >= 30 ? "Senior" : "Junior")
    ));

System.out.println(grouped);
// Output: {New York={Junior=[Alice(25)], Senior=[Bob(30)]}, London={Junior=[David(28)], Senior=[Charlie(35)]}}
```

### `partitioningBy()` - Binary Classification

**Purpose**: Partitions stream elements into two groups based on a predicate.

```java
Map<Boolean, List<Person>> partitionedByAge = people.stream()
    .collect(Collectors.partitioningBy(person -> person.getAge() >= 30));

System.out.println("Adults (30+): " + partitionedByAge.get(true));
System.out.println("Young adults (<30): " + partitionedByAge.get(false));

// Output:
// Adults (30+): [Bob(30), Charlie(35)]
// Young adults (<30): [Alice(25), David(28)]

// Partitioning with downstream collector
Map<Boolean, Long> agePartitionCount = people.stream()
    .collect(Collectors.partitioningBy(
        person -> person.getAge() >= 30,
        Collectors.counting()
    ));

System.out.println(agePartitionCount);
// Output: {false=2, true=2}
```

---

## Advanced Mapping Collectors

### `mapping()` - Transform Then Collect

**Purpose**: Transforms elements before collecting with another collector.

```java
// Extract names from people grouped by city
Map<String, List<String>> namesByCity = people.stream()
    .collect(Collectors.groupingBy(
        Person::getCity,
        Collectors.mapping(Person::getName, Collectors.toList())
    ));

System.out.println(namesByCity);
// Output: {New York=[Alice, Bob], London=[Charlie, David]}

// Get unique ages per city
Map<String, Set<Integer>> agesByCity = people.stream()
    .collect(Collectors.groupingBy(
        Person::getCity,
        Collectors.mapping(Person::getAge, Collectors.toSet())
    ));

System.out.println(agesByCity);
// Output: {New York=[25, 30], London=[35, 28]}
```

---

## Reduction and Transformation Collectors

### `reducing()` - General Purpose Reduction

**Purpose**: Performs a reduction using a binary operator.

```java
// Find the person with maximum age
Optional<Person> oldestPerson = people.stream()
    .collect(Collectors.reducing((p1, p2) -> p1.getAge() > p2.getAge() ? p1 : p2));

oldestPerson.ifPresent(p -> System.out.println("Oldest: " + p));
// Output: Oldest: Charlie(35)

// Sum ages with identity and mapper
Integer totalAge = people.stream()
    .collect(Collectors.reducing(
        0,                    // identity
        Person::getAge,       // mapper
        Integer::sum          // reducer
    ));

System.out.println("Total age: " + totalAge);
// Output: Total age: 118
```

### `collectingAndThen()` - Transform Final Result

**Purpose**: Applies a transformation to the final result of another collector.

```java
// Collect to list and make it immutable
List<String> immutableNames = people.stream()
    .map(Person::getName)
    .collect(Collectors.collectingAndThen(
        Collectors.toList(),
        Collections::unmodifiableList
    ));

// Count and convert to string
String countAsString = people.stream()
    .collect(Collectors.collectingAndThen(
        Collectors.counting(),
        count -> "Total people: " + count
    ));

System.out.println(countAsString);
// Output: Total people: 4
```

---

## Modern Collectors (Java 9-12) 

### `filtering()` - Filter Before Collecting (Java 9)

**Purpose**: Filters elements before applying another collector.

```java
// Group people by city, but only include adults
Map<String, List<Person>> adultsByCity = people.stream()
    .collect(Collectors.groupingBy(
        Person::getCity,
        Collectors.filtering(
            person -> person.getAge() >= 30,
            Collectors.toList()
        )
    ));

System.out.println(adultsByCity);
// Output: {New York=[Bob(30)], London=[Charlie(35)]}
```

### `flatMapping()` - Flatten Before Collecting (Java 9)

**Purpose**: Flattens elements before applying another collector.

```java
class Department {
    private String name;
    private List<String> employees;
    
    public Department(String name, List<String> employees) {
        this.name = name;
        this.employees = employees;
    }
    
    public String getName() { return name; }
    public List<String> getEmployees() { return employees; }
}

List<Department> departments = Arrays.asList(
    new Department("IT", Arrays.asList("Alice", "Bob")),
    new Department("HR", Arrays.asList("Charlie", "David", "Eve"))
);

// Collect all employee names
List<String> allEmployees = departments.stream()
    .collect(Collectors.flatMapping(
        dept -> dept.getEmployees().stream(),
        Collectors.toList()
    ));

System.out.println(allEmployees);
// Output: [Alice, Bob, Charlie, David, Eve]
```

### `teeing()` - Combine Two Collectors (Java 12)

**Purpose**: Applies two collectors to the same stream and combines their results.

```java
class Statistics {
    private final long count;
    private final double average;
    
    public Statistics(long count, double average) {
        this.count = count;
        this.average = average;
    }
    
    @Override
    public String toString() {
        return String.format("Count: %d, Average: %.2f", count, average);
    }
}

Statistics ageStats = people.stream()
    .collect(Collectors.teeing(
        Collectors.counting(),
        Collectors.averagingInt(Person::getAge),
        Statistics::new
    ));

System.out.println(ageStats);
// Output: Count: 4, Average: 29.50
```

---

## Version History Reference

| Collector Method | Since Version | Purpose |
|------------------|---------------|---------|
| `toList()` | Java 8 | Collect to ArrayList |
| `toSet()` | Java 8 | Collect to HashSet |
| `toCollection()` | Java 8 | Collect to specified collection |
| `toMap()` | Java 8 | Create Map from key-value pairs |
| `toConcurrentMap()` | Java 8 | Create ConcurrentMap |
| `counting()` | Java 8 | Count elements |
| `summingInt/Long/Double()` | Java 8 | Sum numeric values |
| `averagingInt/Long/Double()` | Java 8 | Calculate average |
| `summarizingInt/Long/Double()` | Java 8 | Complete statistics |
| `joining()` | Java 8 | Concatenate strings |
| `groupingBy()` | Java 8 | Group by classifier |
| `groupingByConcurrent()` | Java 8 | Concurrent grouping |
| `partitioningBy()` | Java 8 | Binary partition |
| `mapping()` | Java 8 | Transform then collect |
| `reducing()` | Java 8 | General reduction |
| `collectingAndThen()` | Java 8 | Transform final result |
| `filtering()` | Java 9 | Filter before collecting |
| `flatMapping()` | Java 9 | Flatten before collecting |
| `teeing()` | Java 12 | Combine two collectors |

---

## Real-World Case Study: Employee Analytics 

Let's build a comprehensive employee analytics system that demonstrates advanced Collector usage patterns.

### Employee Data Model

```java
import java.time.LocalDate;
import java.util.*;
import java.util.stream.Collectors;

class Employee {
    private final String name;
    private final String department;
    private final String role;
    private final double salary;
    private final LocalDate birthDate;
    private final boolean isManager;
    
    public Employee(String name, String department, String role, 
                   double salary, LocalDate birthDate, boolean isManager) {
        this.name = name;
        this.department = department;
        this.role = role;
        this.salary = salary;
        this.birthDate = birthDate;
        this.isManager = isManager;
    }
    
    // Getters
    public String getName() { return name; }
    public String getDepartment() { return department; }
    public String getRole() { return role; }
    public double getSalary() { return salary; }
    public LocalDate getBirthDate() { return birthDate; }
    public boolean isManager() { return isManager; }
    
    public int getAge() {
        return LocalDate.now().getYear() - birthDate.getYear();
    }
    
    @Override
    public String toString() {
        return String.format("%s (%s, $%.0f, %d years old)", 
                           name, role, salary, getAge());
    }
}
```

### Sample Dataset

```java
List<Employee> employees = Arrays.asList(
    // IT Department
    new Employee("Alice Johnson", "IT", "Senior Developer", 95000, LocalDate.of(1988, 3, 15), false),
    new Employee("Bob Smith", "IT", "DevOps Engineer", 88000, LocalDate.of(1990, 7, 22), false),
    new Employee("Carol Williams", "IT", "Tech Lead", 110000, LocalDate.of(1985, 11, 8), true),
    new Employee("David Brown", "IT", "Junior Developer", 65000, LocalDate.of(1995, 5, 12), false),
    new Employee("Eve Davis", "IT", "System Architect", 125000, LocalDate.of(1982, 9, 30), true),
    
    // HR Department
    new Employee("Frank Wilson", "HR", "HR Manager", 82000, LocalDate.of(1987, 1, 18), true),
    new Employee("Grace Miller", "HR", "Recruiter", 58000, LocalDate.of(1992, 4, 25), false),
    new Employee("Henry Taylor", "HR", "HR Specialist", 62000, LocalDate.of(1989, 8, 14), false),
    
    // Finance Department
    new Employee("Ivy Anderson", "Finance", "Financial Analyst", 72000, LocalDate.of(1991, 6, 3), false),
    new Employee("Jack Thomas", "Finance", "Accountant", 68000, LocalDate.of(1993, 12, 20), false),
    new Employee("Kate Jackson", "Finance", "Finance Manager", 95000, LocalDate.of(1986, 10, 7), true),
    
    // Marketing Department
    new Employee("Liam White", "Marketing", "Marketing Manager", 85000, LocalDate.of(1988, 2, 28), true),
    new Employee("Mia Harris", "Marketing", "Content Specialist", 55000, LocalDate.of(1994, 9, 16), false),
    new Employee("Noah Clark", "Marketing", "Digital Marketer", 63000, LocalDate.of(1992, 11, 5), false),
    new Employee("Olivia Lewis", "Marketing", "Brand Strategist", 78000, LocalDate.of(1989, 7, 11), false),
    
    // Sales: Avg: $65000, Total: $260000

6. YOUNGEST EMPLOYEE BY DEPARTMENT:
Finance: Jack Thomas (Accountant, $68000, 30 years old)
HR: Grace Miller (Recruiter, $58000, 31 years old)
IT: David Brown (Junior Developer, $65000, 28 years old)
Marketing: Mia Harris (Content Specialist, $55000, 29 years old)
Sales: Quinn Hall (Sales Rep, $48000, 27 years old)
```

---

## Key Takeaways and Best Practices 

### Performance Considerations
- **Parallel Streams**: Most collectors work seamlessly with parallel streams
- **Custom Collectors**: Create custom collectors for specialized use cases using `Collector.of()`
- **Memory Efficiency**: Use appropriate collection types (e.g., `toSet()` for uniqueness)

### Common Patterns

1. **Chaining Collectors**: Combine multiple collectors for complex transformations
```java
// Get department names of employees earning > $80k
Set<String> highPayingDepartments = employees.stream()
    .filter(emp -> emp.getSalary() > 80000)
    .map(Employee::getDepartment)
    .collect(Collectors.toSet());
```

2. **Conditional Collection**: Use filtering and mapping for selective processing
```java
// Collect email domains of managers (hypothetical email field)
List<String> managerDomains = employees.stream()
    .filter(Employee::isManager)
    .map(emp -> emp.getEmail().substring(emp.getEmail().indexOf("@") + 1))
    .distinct()
    .collect(Collectors.toList());
```

3. **Hierarchical Grouping**: Build complex data structures with nested collectors
```java
// Group by department, then by salary range
Map<String, Map<String, List<Employee>>> hierarchical = employees.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        Collectors.groupingBy(emp -> emp.getSalary() > 80000 ? "High" : "Standard")
    ));
```

### Error Handling and Edge Cases

```java
// Handle empty streams safely
Optional<Employee> oldestEmployee = employees.stream()
    .collect(Collectors.reducing((e1, e2) -> 
        e1.getAge() > e2.getAge() ? e1 : e2));

// Provide default values for empty groups
Map<String, String> departmentSummary = employees.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        Collectors.collectingAndThen(
            Collectors.counting(),
            count -> count > 0 ? count + " employees" : "No employees"
        )
    ));
```

---

## Advanced Custom Collector Example 

Here's how to create a custom collector that finds the top N employees by salary in each department:

```java
public static <T> Collector<T, ?, List<T>> topN(int n, Comparator<T> comparator) {
    return Collector.of(
        () -> new PriorityQueue<>(comparator.reversed()),
        (queue, item) -> {
            queue.offer(item);
            if (queue.size() > n) {
                queue.poll();
            }
        },
        (queue1, queue2) -> {
            queue2.forEach(queue1::offer);
            while (queue1.size() > n) {
                queue1.poll();
            }
            return queue1;
        },
        queue -> queue.stream()
            .sorted(comparator.reversed())
            .collect(Collectors.toList())
    );
}

// Usage: Top 2 earners per department
Map<String, List<Employee>> top2ByDepartment = employees.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        topN(2, Comparator.comparing(Employee::getSalary))
    ));
```

---

## Conclusion 

Java Stream Collectors provide a powerful, expressive way to transform and aggregate data. From simple operations like `toList()` and `counting()` to complex multi-level groupings and custom collectors, mastering these tools will significantly improve your data processing capabilities.

**Key Benefits Recap:**
- ✅ **Readability**: Code that clearly expresses intent
- ✅ **Performance**: Optimized implementations for common operations
- ✅ **Composability**: Mix and match collectors for complex transformations
- ✅ **Type Safety**: Compile-time guarantees for collection operations
- ✅ **Parallel Processing**: Seamless parallel execution support

---

### Further Reading 📖

- [Oracle Java Documentation: Stream Collectors](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)
- [Java 8 Stream Tutorial](https://www.oracle.com/technical-resources/articles/java/ma14-java-se-8-streams.html)
- [Parallel Processing with Streams](https://docs.oracle.com/javase/tutorial/collections/streams/parallelism.html) Department
    new Employee("Paul Walker", "Sales", "Sales Manager", 92000, LocalDate.of(1984, 4, 2), true),
    new Employee("Quinn Hall", "Sales", "Sales Rep", 48000, LocalDate.of(1996, 1, 9), false),
    new Employee("Ruby Young", "Sales", "Senior Sales Rep", 68000, LocalDate.of(1990, 3, 27), false),
    new Employee("Sam King", "Sales", "Sales Rep", 52000, LocalDate.of(1995, 8, 19), false)
);
```

### Analytics Implementation

```java
public class EmployeeAnalytics {
    
    // 1. Group employees by department
    public static Map<String, List<Employee>> groupByDepartment(List<Employee> employees) {
        return employees.stream()
            .collect(Collectors.groupingBy(Employee::getDepartment));
    }
    
    // 2. Count employees per department
    public static Map<String, Long> countByDepartment(List<Employee> employees) {
        return employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.counting()
            ));
    }
    
    // 3. Find highest paid employee per department
    public static Map<String, Optional<Employee>> highestPaidByDepartment(List<Employee> employees) {
        return employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.reducing((e1, e2) -> 
                    e1.getSalary() > e2.getSalary() ? e1 : e2)
            ));
    }
    
    // 4. Filter managers only per department
    public static Map<String, List<Employee>> managersByDepartment(List<Employee> employees) {
        return employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.filtering(
                    Employee::isManager,
                    Collectors.toList()
                )
            ));
    }
    
    // 5. Department salary statistics using teeing()
    public static Map<String, DepartmentStats> departmentSalaryStats(List<Employee> employees) {
        return employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.teeing(
                    Collectors.averagingDouble(Employee::getSalary),
                    Collectors.summingDouble(Employee::getSalary),
                    DepartmentStats::new
                )
            ));
    }
    
    // 6. Find youngest employee per department with alphabetical tie-breaking
    public static Map<String, Optional<Employee>> youngestByDepartment(List<Employee> employees) {
        return employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.reducing((e1, e2) -> {
                    int ageComparison = Integer.compare(e2.getAge(), e1.getAge()); // Younger first
                    if (ageComparison == 0) {
                        return e1.getName().compareTo(e2.getName()) <= 0 ? e1 : e2; // Alphabetical
                    }
                    return ageComparison < 0 ? e1 : e2;
                })
            ));
    }
    
    // Supporting class for department statistics
    static class DepartmentStats {
        private final double averageSalary;
        private final double totalSalary;
        
        public DepartmentStats(double averageSalary, double totalSalary) {
            this.averageSalary = averageSalary;
            this.totalSalary = totalSalary;
        }
        
        @Override
        public String toString() {
            return String.format("Avg: $%.0f, Total: $%.0f", averageSalary, totalSalary);
        }
    }
}
```

### Running the Analytics

```java
public class EmployeeAnalyticsDemo {
    public static void main(String[] args) {
        List<Employee> employees = createEmployeeDataset(); // As defined above
        
        System.out.println("=== EMPLOYEE ANALYTICS REPORT ===\n");
        
        // 1. Employees by Department
        System.out.println("1. EMPLOYEES BY DEPARTMENT:");
        Map<String, List<Employee>> byDept = EmployeeAnalytics.groupByDepartment(employees);
        byDept.forEach((dept, empList) -> {
            System.out.println(dept + " (" + empList.size() + "):");
            empList.forEach(emp -> System.out.println("  - " + emp));
            System.out.println();
        });
        
        // 2. Employee Count per Department
        System.out.println("2. EMPLOYEE COUNT BY DEPARTMENT:");
        Map<String, Long> counts = EmployeeAnalytics.countByDepartment(employees);
        counts.entrySet().stream()
            .sorted(Map.Entry.<String, Long>comparingByValue().reversed())
            .forEach(entry -> System.out.println(entry.getKey() + ": " + entry.getValue()));
        System.out.println();
        
        // 3. Highest Paid Employee per Department
        System.out.println("3. HIGHEST PAID BY DEPARTMENT:");
        Map<String, Optional<Employee>> highestPaid = EmployeeAnalytics.highestPaidByDepartment(employees);
        highestPaid.forEach((dept, emp) -> 
            emp.ifPresent(e -> System.out.println(dept + ": " + e)));
        System.out.println();
        
        // 4. Managers per Department
        System.out.println("4. MANAGERS BY DEPARTMENT:");
        Map<String, List<Employee>> managers = EmployeeAnalytics.managersByDepartment(employees);
        managers.forEach((dept, mgrs) -> {
            if (!mgrs.isEmpty()) {
                System.out.println(dept + ":");
                mgrs.forEach(mgr -> System.out.println("  - " + mgr));
            }
        });
        System.out.println();
        
        // 5. Department Salary Statistics
        System.out.println("5. SALARY STATISTICS BY DEPARTMENT:");
        Map<String, EmployeeAnalytics.DepartmentStats> stats = 
            EmployeeAnalytics.departmentSalaryStats(employees);
        stats.entrySet().stream()
            .sorted(Map.Entry.<String, EmployeeAnalytics.DepartmentStats>comparingByKey())
            .forEach(entry -> System.out.println(entry.getKey() + ": " + entry.getValue()));
        System.out.println();
        
        // 6. Youngest Employee per Department
        System.out.println("6. YOUNGEST EMPLOYEE BY DEPARTMENT:");
        Map<String, Optional<Employee>> youngest = EmployeeAnalytics.youngestByDepartment(employees);
        youngest.forEach((dept, emp) -> 
            emp.ifPresent(e -> System.out.println(dept + ": " + e)));
    }
}
