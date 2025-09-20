# Java LTS Features Cheat Sheet (Java 8 → 25)

This document summarizes **all major language, library, and JVM features** across **Java 8, 11, 17, 21, and 25**, with rolled-up updates from the non-LTS versions in between.

---

## Java 8 (2014, LTS)

### Language Features
- **Lambda Expressions**
- **Method References**
- **Functional Interfaces (`@FunctionalInterface`)**
- **Default & Static Methods in Interfaces**
- **Type Annotations (JSR 308)**
- **Repeatable Annotations**

### Libraries & APIs
- **Streams API** with `map`, `filter`, `reduce`
- **Collectors** (`toList()`, `groupingBy()`, etc.)
- **Optional** for null-safety
- **New Date/Time API (`java.time`)**
- **Nashorn JS Engine**
- **Base64 API**

### Concurrency
- **CompletableFuture**
- **StampedLock**
- **ConcurrentHashMap enhancements**
- **Parallel Array Sorting**

### JVM & Runtime
- **Metaspace replaces PermGen**
- **Compact Profiles**

---

## Java 11 (2018, LTS)  
*(includes Java 9 & 10 features)*

### From Java 9
- **Modules (JPMS, Project Jigsaw)**
- **JShell (REPL)**
- **Factory methods for collections**: `List.of()`, `Set.of()`, `Map.of()`
- **Private methods in interfaces**
- **Try-with-resources simplified** (`try(resource)` reuse)
- **Stream API improvements**: `takeWhile`, `dropWhile`, `iterate` with predicate
- **Optional.stream()**
- **Process API updates**
- **Reactive Streams (`Flow` API)**

### From Java 10
- **Local Variable Type Inference (`var`)**
- **Optional.orElseThrow()**
- **Unmodifiable collectors** (`Collectors.toUnmodifiableList()` etc.)
- **Performance: G1 parallel full GC**

### Java 11 Core
- **New String methods**: `isBlank()`, `lines()`, `strip()`, `repeat()`
- **HTTP Client API (standardized)**
- **Local-variable syntax for lambda parameters**
- **Single-file source code launch** (`java Hello.java`)
- **Nest-based access control**
- **Removal of Java EE / CORBA modules**
- **Flight Recorder, ZGC (low-latency GC)**

---

## Java 17 (2021, LTS)  
*(includes Java 12–16 features)*

### From Java 12
- **Switch Expressions (preview)** → `case L -> ...`
- **Compact Number Formatting**
- **Shenandoah GC (experimental)**

### From Java 13
- **Text Blocks (preview)** — multi-line strings
- **Switch Expressions refined**

### From Java 14
- **Records (preview)**
- **Pattern Matching for `instanceof` (preview)**
- **Helpful NullPointerExceptions**
- **JFR Event Streaming**

### From Java 15
- **Sealed Classes (preview)**
- **Text Blocks (final)**
- **Hidden Classes**
- **EdDSA cryptography**

### From Java 16
- **Records (final)**
- **Pattern Matching for `instanceof` (final)**
- **Vector API (incubator)**
- **Strong encapsulation of JDK internals**

### Java 17 Core
- **Sealed Classes (final)**
- **Pattern Matching for `switch` (preview)**
- **Foreign Function & Memory API (incubator)**
- **Strong encapsulation enforced by default**
- **macOS/AArch64 port**

---

## Java 21 (2023, LTS)  
*(includes Java 18–20 features)*

### From Java 18
- **Simple Web Server (for testing)**
- **UTF-8 by default**
- **Code Snippets in JavaDoc**

### From Java 19
- **Virtual Threads (preview)**
- **Structured Concurrency (incubator)**
- **Record Patterns (preview)**

### From Java 20
- **Scoped Values (incubator)**
- **Record Patterns (second preview)**
- **Virtual Threads (second preview)**

### Java 21 Core
- **Virtual Threads (final)** — lightweight concurrency (Project Loom)
- **Pattern Matching for `switch` (final)**
- **Record Patterns (final)**
- **Sequenced Collections API**
- **String Templates (preview)**
- **Key Encapsulation Mechanism (KEM) API**

---

## Java 25 (2025, LTS)  
*(includes Java 22–24 features)*

### From Java 22
- **Foreign Function & Memory API (final)**
- **Structured Concurrency (preview)**
- **String Templates (second preview)**
- **Stream Gatherers API**

### From Java 23
- **Implicit Classes/Instance Main methods (preview)**
- **Scoped Values (second incubator)**
- **Primitive types in patterns (preview)**

### From Java 24
- **Classfile API (incubator)**
- **Stream Gatherers (final)**
- **String Templates (third preview)**

### Java 25 Core
- **Compact Source Files & Instance Main Methods (final)**  
  Write programs without boilerplate `class`/`static main`.
- **Flexible Constructor Bodies**  
  Statements allowed before `super()`/`this()`.
- **Scoped Values (final)**  
  Safer alternative to `ThreadLocal`.
- **Module Import Declarations**  
  Import entire module exports at once.
- **Compact Object Headers**  
  Reduce memory footprint of objects.
- **JFR & Profiling Enhancements**  
  CPU-time profiling, cooperative sampling.
- **Dropped 32-bit x86 port**

---
