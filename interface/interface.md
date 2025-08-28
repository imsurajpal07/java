# Mastering Java Interfaces: Core Concepts, Functional Interfaces, Usage, and Best Practices

Java interfaces have evolved significantly since the language's inception, transforming from simple contracts for abstraction into powerful tools that enable functional programming paradigms. In this comprehensive guide, we'll explore the journey from traditional interfaces to modern functional interfaces, examining their role in contemporary Java development and how they contribute to writing clean, maintainable, and expressive code.

Whether you're working with legacy systems or building cutting-edge applications, understanding interfaces in their various forms is crucial for effective Java development. Let's dive deep into the world of Java interfaces and discover how they can elevate your programming skills.

## What is a Java Interface?

A Java interface is a reference type that defines a contract of methods that implementing classes must fulfill. Think of an interface as a blueprint or specification that describes what a class should do, without dictating how it should do it. Unlike classes, interfaces cannot be instantiated directly and primarily serve as a mechanism for achieving abstraction and multiple inheritance of type.

### Interface vs. Class: Key Differences

| Aspect | Interface | Class |
|--------|-----------|-------|
| Instantiation | Cannot be instantiated | Can be instantiated |
| Methods | Abstract by default (before Java 8) | Can have concrete implementations |
| Inheritance | Multiple inheritance supported | Single inheritance only |
| Variables | Public, static, final by default | Can have instance variables |
| Access Modifiers | Public or package-private only | All access modifiers supported |

### Real-World Analogy

Consider a car interface in the real world. Different car manufacturers (BMW, Toyota, Honda) implement the same basic interface: they all have steering wheels, brakes, accelerators, and gear systems. However, each manufacturer implements these features differently. The interface ensures that anyone who knows how to drive one car can drive another, despite the internal differences.

```java
// The "driving interface" that all cars must implement
public interface Driveable {
    void start();
    void stop();
    void accelerate();
    void brake();
}
```

### Uses and Importance

Interfaces are fundamental for:
- **Abstraction**: Hiding implementation details while exposing essential functionality
- **Decoupling**: Reducing dependencies between different parts of your application
- **Polymorphism**: Allowing objects of different types to be treated uniformly
- **Testing**: Enabling easy mocking and stubbing for unit tests
- **API Design**: Creating stable contracts between different layers of your application

## Declaring and Implementing Interfaces

### Basic Interface Declaration

```java
// Basic interface declaration
public interface PaymentProcessor {
    // Abstract method (implicitly public and abstract)
    boolean processPayment(double amount);
    
    // Constant (implicitly public, static, and final)
    double MAX_TRANSACTION_AMOUNT = 10000.0;
}

// Implementation
public class CreditCardProcessor implements PaymentProcessor {
    @Override
    public boolean processPayment(double amount) {
        if (amount > MAX_TRANSACTION_AMOUNT) {
            throw new IllegalArgumentException("Amount exceeds maximum limit");
        }
        // Credit card processing logic
        System.out.println("Processing credit card payment: $" + amount);
        return true;
    }
}

public class PayPalProcessor implements PaymentProcessor {
    @Override
    public boolean processPayment(double amount) {
        // PayPal processing logic
        System.out.println("Processing PayPal payment: $" + amount);
        return true;
    }
}
```

### Usage Example

```java
public class PaymentService {
    public void executePayment(PaymentProcessor processor, double amount) {
        if (processor.processPayment(amount)) {
            System.out.println("Payment successful!");
        } else {
            System.out.println("Payment failed!");
        }
    }
    
    public static void main(String[] args) {
        PaymentService service = new PaymentService();
        
        // Polymorphism in action
        service.executePayment(new CreditCardProcessor(), 150.0);
        service.executePayment(new PayPalProcessor(), 75.0);
    }
}
```

## Key Features of Java Interfaces

### Abstract Methods

By default, all methods in an interface are public and abstract (before Java 8). Implementing classes must provide concrete implementations for all abstract methods.

```java
public interface DatabaseConnection {
    void connect();    // implicitly public abstract
    void disconnect(); // implicitly public abstract
    boolean isConnected();
}
```

### Default Methods (Java 8+)

Default methods allow you to add new methods to interfaces without breaking existing implementations.

```java
public interface Logger {
    void log(String message);
    
    // Default method with implementation
    default void logWithTimestamp(String message) {
        System.out.println("[" + java.time.LocalDateTime.now() + "] " + message);
    }
    
    default void logError(String error) {
        log("ERROR: " + error);
    }
}

public class ConsoleLogger implements Logger {
    @Override
    public void log(String message) {
        System.out.println(message);
    }
    // Can use default methods without overriding
}
```

### Static Methods (Java 8+)

Static methods in interfaces belong to the interface itself and can be called without an instance.

```java
public interface MathUtils {
    static double calculateCircleArea(double radius) {
        return Math.PI * radius * radius;
    }
    
    static boolean isPrime(int number) {
        if (number <= 1) return false;
        for (int i = 2; i <= Math.sqrt(number); i++) {
            if (number % i == 0) return false;
        }
        return true;
    }
}

// Usage
double area = MathUtils.calculateCircleArea(5.0);
boolean prime = MathUtils.isPrime(17);
```

### Private Methods (Java 9+)

Private methods help reduce code duplication within interfaces by allowing shared logic between default methods.

```java
public interface ValidationUtils {
    default boolean isValidEmail(String email) {
        return isNotEmpty(email) && email.contains("@") && email.contains(".");
    }
    
    default boolean isValidUsername(String username) {
        return isNotEmpty(username) && username.length() >= 3;
    }
    
    // Private helper method
    private boolean isNotEmpty(String str) {
        return str != null && !str.trim().isEmpty();
    }
}
```

### Constants

All variables in interfaces are implicitly public, static, and final.

```java
public interface HttpStatus {
    int OK = 200;
    int NOT_FOUND = 404;
    int INTERNAL_SERVER_ERROR = 500;
    
    // Equivalent to:
    // public static final int OK = 200;
}
```

### Interface Inheritance and Multiple Inheritance

Interfaces can extend other interfaces, and a class can implement multiple interfaces.

```java
public interface Readable {
    String read();
}

public interface Writable {
    void write(String data);
}

// Interface extending another interface
public interface ReadWritable extends Readable, Writable {
    void copy(String source, String destination);
}

// Class implementing multiple interfaces
public class FileHandler implements ReadWritable {
    @Override
    public String read() {
        return "Reading file content";
    }
    
    @Override
    public void write(String data) {
        System.out.println("Writing: " + data);
    }
    
    @Override
    public void copy(String source, String destination) {
        String content = read();
        write(content);
    }
}
```

## Nested Interfaces

Nested interfaces are interfaces declared within another class or interface. They provide better encapsulation and organization of related functionality.

### Syntax and Basic Usage

```java
public class DatabaseManager {
    
    // Nested interface
    public interface ConnectionPool {
        Connection getConnection();
        void releaseConnection(Connection conn);
        int getActiveConnections();
    }
    
    // Static nested interface
    public static interface QueryBuilder {
        QueryBuilder select(String... columns);
        QueryBuilder from(String table);
        QueryBuilder where(String condition);
        String build();
    }
    
    // Implementation of nested interface
    private static class SimpleConnectionPool implements ConnectionPool {
        private List<Connection> connections = new ArrayList<>();
        
        @Override
        public Connection getConnection() {
            // Implementation logic
            return null; // Simplified
        }
        
        @Override
        public void releaseConnection(Connection conn) {
            // Implementation logic
        }
        
        @Override
        public int getActiveConnections() {
            return connections.size();
        }
    }
}
```

### Use Cases and Benefits

1. **Encapsulation**: Groups related functionality within a specific context
2. **Namespace Management**: Avoids naming conflicts
3. **Logical Organization**: Makes code more readable and maintainable

```java
public class EventSystem {
    
    // Nested interface for event handlers
    public interface EventHandler<T> {
        void handle(T event);
        default boolean canHandle(Class<?> eventType) {
            return true;
        }
    }
    
    // Nested interface for event publishers
    public interface EventPublisher {
        void publish(Object event);
        void subscribe(Class<?> eventType, EventHandler<?> handler);
    }
    
    // Implementation
    public static class SimpleEventPublisher implements EventPublisher {
        private Map<Class<?>, List<EventHandler<Object>>> handlers = new HashMap<>();
        
        @Override
        public void publish(Object event) {
            List<EventHandler<Object>> eventHandlers = handlers.get(event.getClass());
            if (eventHandlers != null) {
                eventHandlers.forEach(handler -> handler.handle(event));
            }
        }
        
        @Override
        @SuppressWarnings("unchecked")
        public void subscribe(Class<?> eventType, EventHandler<?> handler) {
            handlers.computeIfAbsent(eventType, k -> new ArrayList<>())
                    .add((EventHandler<Object>) handler);
        }
    }
}
```

## What is a Functional Interface?

A functional interface is an interface that contains exactly one abstract method, also known as a Single Abstract Method (SAM) interface. Functional interfaces are the cornerstone of Java's functional programming capabilities introduced in Java 8.

### The @FunctionalInterface Annotation

While not mandatory, the `@FunctionalInterface` annotation provides compile-time safety and documentation.

```java
@FunctionalInterface
public interface Calculator {
    double calculate(double a, double b);
    
    // Default and static methods don't count toward the single abstract method rule
    default void printResult(double result) {
        System.out.println("Result: " + result);
    }
    
    static void printWelcome() {
        System.out.println("Welcome to Calculator!");
    }
}
```

### Predefined Functional Interfaces

Java provides several built-in functional interfaces:

```java
// Runnable - executes code without parameters or return value
Runnable task = () -> System.out.println("Running task");

// Callable - executes code and returns a value
Callable<String> callable = () -> "Hello World";

// Comparator - compares two objects
Comparator<String> lengthComparator = (s1, s2) -> s1.length() - s2.length();

// Example usage
List<String> names = Arrays.asList("John", "Jane", "Bob", "Alice");
names.sort(lengthComparator);
System.out.println(names); // [Bob, John, Jane, Alice]
```

### Custom Functional Interface Example

```java
@FunctionalInterface
public interface StringProcessor {
    String process(String input);
}

public class StringProcessorDemo {
    public static void main(String[] args) {
        // Using lambda expressions
        StringProcessor toUpperCase = text -> text.toUpperCase();
        StringProcessor reverse = text -> new StringBuilder(text).reverse().toString();
        StringProcessor addPrefix = text -> "Processed: " + text;
        
        String input = "hello world";
        
        // Using the functional interfaces
        System.out.println(toUpperCase.process(input));  // HELLO WORLD
        System.out.println(reverse.process(input));      // dlrow olleh
        System.out.println(addPrefix.process(input));    // Processed: hello world
        
        // Method reference example
        StringProcessor trim = String::trim;
        System.out.println(trim.process("  spaced  "));  // spaced
    }
}
```

### Core Functional Interfaces from java.util.function

#### Consumer<T> - Accepts input, returns nothing

```java
import java.util.function.Consumer;

Consumer<String> printer = message -> System.out.println("Message: " + message);
Consumer<List<String>> listPrinter = list -> list.forEach(System.out::println);

// Usage
printer.accept("Hello World");

List<String> items = Arrays.asList("apple", "banana", "cherry");
listPrinter.accept(items);

// Chaining consumers
Consumer<String> upperCasePrinter = message -> System.out.println(message.toUpperCase());
Consumer<String> combined = printer.andThen(upperCasePrinter);
combined.accept("hello"); // Prints both normal and uppercase versions
```

#### Predicate<T> - Accepts input, returns boolean

```java
import java.util.function.Predicate;

Predicate<Integer> isEven = number -> number % 2 == 0;
Predicate<String> isLongString = text -> text.length() > 5;
Predicate<String> startsWithA = text -> text.toLowerCase().startsWith("a");

// Usage
System.out.println(isEven.test(4));        // true
System.out.println(isLongString.test("hi")); // false

// Combining predicates
Predicate<String> complexPredicate = isLongString.and(startsWithA);
System.out.println(complexPredicate.test("apple"));  // false (length <= 5)
System.out.println(complexPredicate.test("amazing")); // true

// Filtering with predicates
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
List<Integer> evenNumbers = numbers.stream()
    .filter(isEven)
    .collect(Collectors.toList());
System.out.println(evenNumbers); // [2, 4, 6, 8]
```

#### Function<T, R> - Accepts input, returns output

```java
import java.util.function.Function;

Function<String, Integer> stringLength = text -> text.length();
Function<Integer, String> intToString = number -> "Number: " + number;
Function<String, String> toUpperCase = String::toUpperCase;

// Usage
System.out.println(stringLength.apply("hello"));     // 5
System.out.println(intToString.apply(42));          // Number: 42

// Composing functions
Function<String, String> lengthAndUpperCase = stringLength.andThen(intToString).andThen(toUpperCase);
System.out.println(lengthAndUpperCase.apply("test")); // NUMBER: 4

// Transforming collections
List<String> words = Arrays.asList("java", "python", "javascript");
List<Integer> lengths = words.stream()
    .map(stringLength)
    .collect(Collectors.toList());
System.out.println(lengths); // [4, 6, 10]
```

#### Supplier<T> - Returns output without input

```java
import java.util.function.Supplier;

Supplier<Double> randomValue = Math::random;
Supplier<String> currentTime = () -> LocalDateTime.now().toString();
Supplier<List<String>> defaultList = () -> Arrays.asList("default", "values");

// Usage
System.out.println(randomValue.get());  // Random double between 0.0 and 1.0
System.out.println(currentTime.get());  // Current timestamp

// Lazy evaluation example
Supplier<String> expensiveOperation = () -> {
    System.out.println("Performing expensive operation...");
    return "Expensive result";
};

// Only executed when needed
if (someCondition) {
    String result = expensiveOperation.get();
}
```

### Advanced Functional Interface Example

```java
@FunctionalInterface
public interface TriFunction<T, U, V, R> {
    R apply(T t, U u, V v);
    
    default <W> TriFunction<T, U, V, W> andThen(Function<? super R, ? extends W> after) {
        Objects.requireNonNull(after);
        return (T t, U u, V v) -> after.apply(apply(t, u, v));
    }
}

// Usage example
TriFunction<Integer, Integer, Integer, Integer> sum = (a, b, c) -> a + b + c;
TriFunction<Integer, Integer, Integer, String> sumAsString = sum.andThen(Object::toString);

System.out.println(sum.apply(1, 2, 3));           // 6
System.out.println(sumAsString.apply(1, 2, 3));   // "6"
```

## The Diamond Problem and Java's Solution

The diamond problem occurs in multiple inheritance when a class inherits from two classes that have a common base class, creating ambiguity about which method implementation to use.

### Traditional Diamond Problem

```java
// This would cause issues in languages with multiple class inheritance
class A {
    void method() { System.out.println("A"); }
}

class B extends A {
    void method() { System.out.println("B"); }
}

class C extends A {
    void method() { System.out.println("C"); }
}

// This would be problematic: class D extends B, C { }
```

### Java's Interface Solution

Java resolves this through interfaces and provides clear rules for method resolution:

```java
interface Flyable {
    default void move() {
        System.out.println("Flying");
    }
}

interface Swimable {
    default void move() {
        System.out.println("Swimming");
    }
}

// Diamond problem with interfaces
class Duck implements Flyable, Swimable {
    // Must override to resolve ambiguity
    @Override
    public void move() {
        // Choose one explicitly
        Flyable.super.move();
        // Or provide custom implementation
        // System.out.println("Duck moves by flying or swimming");
    }
}

// More complex scenario
interface Vehicle {
    default void start() {
        System.out.println("Vehicle starting");
    }
}

interface Car extends Vehicle {
    default void start() {
        System.out.println("Car starting with ignition");
    }
}

interface ElectricVehicle extends Vehicle {
    default void start() {
        System.out.println("Electric vehicle starting silently");
    }
}

class Tesla implements Car, ElectricVehicle {
    @Override
    public void start() {
        // Custom implementation combining both behaviors
        System.out.println("Tesla starting with electric ignition");
        Car.super.start();        // Call Car's implementation
        ElectricVehicle.super.start(); // Call ElectricVehicle's implementation
    }
}
```

### Resolution Rules

Java follows these rules for method resolution:
1. **Class methods win**: Class methods take precedence over interface default methods
2. **More specific interfaces win**: If a sub-interface overrides a default method, it takes precedence
3. **Explicit resolution required**: When ambiguity exists, the implementing class must override the method

```java
interface A {
    default void method() { System.out.println("A"); }
}

interface B extends A {
    default void method() { System.out.println("B"); }
}

interface C extends A {
    default void method() { System.out.println("C"); }
}

class Implementation implements B, C {
    // Must override due to ambiguity between B and C
    @Override
    public void method() {
        B.super.method(); // Explicitly choose B's implementation
    }
}
```

## Best Practices

### Naming Conventions

Follow these established naming patterns for interfaces:

```java
// Capability/behavior interfaces - use adjectives ending in -able
public interface Comparable<T> { }
public interface Serializable { }
public interface Runnable { }

// Service interfaces - use nouns describing the service
public interface UserService { }
public interface PaymentProcessor { }
public interface DataRepository { }

// Functional interfaces - use descriptive names for the operation
public interface StringValidator { }
public interface NumberCalculator { }
public interface EventHandler<T> { }

// Avoid generic names like IInterface or AbstractInterface
// Bad examples:
// public interface IUserService { }  // Hungarian notation
// public interface AbstractCalculator { }  // Confusing with abstract classes
```

### When to Use Interfaces vs Abstract Classes

| Use Interface When | Use Abstract Class When |
|-------------------|-------------------------|
| Multiple inheritance needed | Sharing code among related classes |
| Defining contracts for unrelated classes | Common base functionality exists |
| Enabling functional programming | Need constructor logic |
| API design and loose coupling | Gradual implementation over versions |
| Testing with mocks | Need protected/private methods |

```java
// Good interface usage - unrelated classes can implement
public interface Drawable {
    void draw();
    default void highlight() {
        System.out.println("Highlighting drawable object");
    }
}

class Circle implements Drawable {
    @Override
    public void draw() { System.out.println("Drawing circle"); }
}

class Button implements Drawable {
    @Override
    public void draw() { System.out.println("Drawing button"); }
}

// Good abstract class usage - shared functionality
public abstract class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    // Common implementation
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    // Abstract method for subclasses
    public abstract void makeSound();
}
```

### Designing Functional Interfaces

#### Keep It Simple and Focused

```java
// Good - single, clear responsibility
@FunctionalInterface
public interface EmailValidator {
    boolean isValid(String email);
}

// Avoid - too many concerns mixed together
@FunctionalInterface
public interface UserProcessor {
    // This interface tries to do too much
    User processUserData(String email, String name, int age, boolean active);
}

// Better - separate concerns
@FunctionalInterface
public interface UserFactory {
    User createUser(String email, String name);
}

@FunctionalInterface
public interface UserValidator {
    boolean isValid(User user);
}
```

#### Provide Meaningful Default Methods

```java
@FunctionalInterface
public interface DataProcessor<T> {
    List<T> process(List<T> data);
    
    // Useful default methods
    default T processSingle(T item) {
        return process(Arrays.asList(item)).get(0);
    }
    
    default DataProcessor<T> andThen(DataProcessor<T> next) {
        return data -> next.process(this.process(data));
    }
    
    default DataProcessor<T> compose(DataProcessor<T> before) {
        return data -> this.process(before.process(data));
    }
}

// Usage
DataProcessor<String> trimmer = list -> list.stream()
    .map(String::trim)
    .collect(Collectors.toList());

DataProcessor<String> upperCaser = list -> list.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

DataProcessor<String> combined = trimmer.andThen(upperCaser);
```

#### Use Generic Types Appropriately

```java
// Good - flexible and type-safe
@FunctionalInterface
public interface Transformer<T, R> {
    R transform(T input);
}

// Usage with different types
Transformer<String, Integer> stringToLength = String::length;
Transformer<User, String> userToName = User::getName;
Transformer<Order, BigDecimal> orderToTotal = Order::getTotal;

// Bounded generics when appropriate
@FunctionalInterface
public interface NumberProcessor<T extends Number> {
    double process(T number);
}
```

### General Interface Design Tips

#### 1. Keep Interfaces Cohesive

```java
// Good - cohesive interface focused on file operations
public interface FileProcessor {
    String readFile(String path);
    void writeFile(String path, String content);
    boolean fileExists(String path);
    long getFileSize(String path);
}

// Avoid - mixing unrelated responsibilities
public interface UtilityInterface {
    String readFile(String path);    // File operation
    void sendEmail(String to);       // Email operation
    double calculateTax(double amount); // Math operation
}
```

#### 2. Design for Extension

```java
public interface CacheManager {
    void put(String key, Object value);
    Object get(String key);
    void remove(String key);
    
    // Extension points
    default void putWithExpiry(String key, Object value, Duration expiry) {
        put(key, value);
        // Default implementation, can be overridden
    }
    
    default boolean containsKey(String key) {
        return get(key) != null;
    }
    
    default void clear() {
        // Default implementation - remove all keys
        // Subclasses can provide more efficient implementations
    }
}
```

#### 3. Use Composition Over Complex Inheritance

```java
// Instead of complex interface hierarchies
public interface DatabaseOperations {
    default UserRepository users() {
        return new UserRepositoryImpl(this);
    }
    
    default OrderRepository orders() {
        return new OrderRepositoryImpl(this);
    }
    
    // Core database operations
    <T> T execute(String query, Class<T> resultType);
    void executeUpdate(String query);
}
```

## Frequently Asked Questions (FAQ)

### Can interfaces have constructors or instance variables?

**No, interfaces cannot have constructors or instance variables.**

```java
public interface InvalidInterface {
    // This will cause compilation error
    // private String instanceVar; // ❌ No instance variables
    
    // This will cause compilation error
    // public InvalidInterface() { } // ❌ No constructors
    
    // This is valid - constants (implicitly static final)
    String CONSTANT = "This is fine"; // ✅ Constants are allowed
    
    // This is valid - static methods can be used for factory patterns
    static InvalidInterface createInstance() { // ✅ Static methods allowed
        return new InvalidInterfaceImpl();
    }
}
```

### Can interfaces be private or protected?

**Top-level interfaces can only be public or package-private (default). Nested interfaces can use all access modifiers.**

```java
// Top-level interfaces
public interface PublicInterface { }        // ✅ Valid
interface PackagePrivateInterface { }       // ✅ Valid (default access)

// private interface TopLevelPrivate { }    // ❌ Invalid
// protected interface TopLevelProtected { } // ❌ Invalid

// Nested interfaces can have any access modifier
public class OuterClass {
    public interface PublicNestedInterface { }        // ✅ Valid
    protected interface ProtectedNestedInterface { }  // ✅ Valid
    interface PackageNestedInterface { }              // ✅ Valid
    private interface PrivateNestedInterface { }      // ✅ Valid
}
```

### How do functional interfaces differ from regular interfaces?

**Functional interfaces have exactly one abstract method and enable lambda expressions.**

```java
// Regular interface - multiple abstract methods
public interface RegularInterface {
    void method1();
    void method2();
    int method3(String param);
    // Cannot use lambda expressions with this
}

// Functional interface - single abstract method
@FunctionalInterface
public interface FunctionalInterface {
    void singleMethod(String param);
    
    // These don't count toward the "single method" rule
    default void defaultMethod() { }
    static void staticMethod() { }
}

// Usage comparison
public class InterfaceComparison {
    public static void main(String[] args) {
        // Regular interface - must use anonymous class or separate implementation
        RegularInterface regular = new RegularInterface() {
            @Override
            public void method1() { System.out.println("Method 1"); }
            
            @Override
            public void method2() { System.out.println("Method 2"); }
            
            @Override
            public int method3(String param) { return param.length(); }
        };
        
        // Functional interface - can use lambda expressions
        FunctionalInterface functional = param -> System.out.println("Processing: " + param);
        
        // Or method references
        FunctionalInterface functional2 = System.out::println;
    }
}
```

### How to use lambdas with functional interfaces?

**Lambda expressions provide a concise way to implement functional interfaces.**

```java
@FunctionalInterface
public interface Calculator {
    double calculate(double a, double b);
}

public class LambdaExamples {
    public static void main(String[] args) {
        // Lambda expressions
        Calculator addition = (a, b) -> a + b;
        Calculator multiplication = (a, b) -> a * b;
        Calculator division = (a, b) -> {
            if (b == 0) throw new IllegalArgumentException("Division by zero");
            return a / b;
        };
        
        // Method references
        Calculator max = Math::max;
        Calculator min = Math::min;
        
        // Using the functional interfaces
        System.out.println(addition.calculate(5, 3));      // 8.0
        System.out.println(multiplication.calculate(4, 7)); // 28.0
        System.out.println(max.calculate(10, 15));         // 15.0
        
        // With collections
        List<Double> numbers = Arrays.asList(1.0, 2.0, 3.0, 4.0);
        
        // Using Function<T, R>
        List<String> formatted = numbers.stream()
            .map(num -> String.format("%.2f", num))
            .collect(Collectors.toList());
        
        // Using Predicate<T>
        List<Double> filtered = numbers.stream()
            .filter(num -> num > 2.0)
            .collect(Collectors.toList());
        
        // Using Consumer<T>
        numbers.forEach(num -> System.out.println("Number: " + num));
        
        // Method reference version
        numbers.forEach(System.out::println);
    }
}
```

### What happens if a functional interface has multiple abstract methods?

**It's no longer a functional interface and cannot be used with lambda expressions.**

```java
// This annotation will cause a compilation error
// @FunctionalInterface  // ❌ Compilation error: not a functional interface
public interface NotFunctional {
    void method1();
    void method2(); // Second abstract method makes it non-functional
}

// You'd have to use anonymous classes
NotFunctional notFunctional = new NotFunctional() {
    @Override
    public void method1() {
        System.out.println("Method 1");
    }
    
    @Override
    public void method2() {
        System.out.println("Method 2");
    }
};

// Cannot do this:
// NotFunctional lambda = () -> { }; // ❌ Won't compile
```

### Can I have multiple functional interfaces in one class?

**Yes, a class can implement multiple functional interfaces.**

```java
@FunctionalInterface
interface Processor {
    String process(String input);
}

@FunctionalInterface
interface Validator {
    boolean validate(String input);
}

public class TextHandler implements Processor, Validator {
    @Override
    public String process(String input) {
        return input.toUpperCase().trim();
    }
    
    @Override
    public boolean validate(String input) {
        return input != null && !input.isEmpty();
    }
    
    public static void main(String[] args) {
        TextHandler handler = new TextHandler();
        
        // Can be used as either interface type
        Processor processor = handler;
        Validator validator = handler;
        
        String text = "  hello world  ";
        if (validator.validate(text)) {
            String result = processor.process(text);
            System.out.println(result); // HELLO WORLD
        }
    }
}
```

### How do default methods work in inheritance hierarchies?

**Default methods follow inheritance rules with class methods taking precedence.**

```java
interface A {
    default void method() {
        System.out.println("A's default method");
    }
}

interface B extends A {
    default void method() {
        System.out.println("B's default method");
    }
}

class C implements A {
    // Uses A's default method
}

class D implements B {
    // Uses B's default method (more specific)
}

class E implements A {
    @Override
    public void method() {
        System.out.println("E's overridden method");
        A.super.method(); // Can still call the default method
    }
}

public class DefaultMethodExample {
    public static void main(String[] args) {
        new C().method(); // A's default method
        new D().method(); // B's default method
        new E().method(); // E's overridden method, then A's default method
    }
}
```

## Conclusion

Java interfaces have evolved from simple contracts into powerful tools that enable both object-oriented and functional programming paradigms. Understanding the full spectrum of interface capabilities—from traditional abstract methods to modern functional interfaces—is essential for writing clean, maintainable, and expressive Java code.
