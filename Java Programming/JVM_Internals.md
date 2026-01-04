Below are **clean, corrected, interview-day JVM Architecture notes**.
They are **short, precise, fact-checked**, and written to be **read in 5–10 minutes before an interview**.

---

# JVM Architecture — Interview Notes

## What is JVM?

* JVM (Java Virtual Machine) **loads, verifies, and executes Java bytecode**
* Provides **platform independence**
* Created when you run:

  ```bash
  java MyApp
  ```

---

## High-Level JVM Architecture (3 Components)

1. **Class Loader Subsystem**
2. **Runtime Data Areas (Memory)**
3. **Execution Engine**

---

## 1. Class Loader Subsystem

### Purpose

* Loads `.class` files into JVM memory
* Follows **parent delegation model**

---

### Class Loader Phases

#### 1️⃣ Load

* Reads bytecode from:

  * File system
  * JAR files
  * Network (implementation-dependent)

**Types of Class Loaders**

1. **Bootstrap ClassLoader**

   * Loads core Java classes
   * `java.lang`, `java.util`, etc.
   * In Java 8: from `rt.jar`
   * In Java 9+: from **modules**

2. **Extension (Platform) ClassLoader**

   * Loads classes from `lib/ext` (Java 8)
   * Platform modules (Java 9+)

3. **Application ClassLoader**

   * Loads classes from:

     * Classpath
     * `-cp` or `-classpath`

---

#### 2️⃣ Link

Link phase has **three steps**:

##### a) Verify

* Ensures bytecode is valid and secure
* Checks:

  * Bytecode format
  * Stack safety
  * Access rules
* Prevents malicious code

##### b) Prepare

* Allocates memory for **static variables**
* Initializes them to **default values**

  * `int → 0`
  * `boolean → false`
  * `object → null`

⚠️ **No user-defined values assigned here**

##### c) Resolve

* Converts **symbolic references** → **direct references**
* Example:

  * Class names
  * Method references
  * Field references

> Note: Resolution can be **lazy** (done at runtime)

---

#### 3️⃣ Initialize

* Executes:

  * Static initializers (`static {}`)
  * Static variable assignments
* Happens **once per class**

---

### Common Class Loading Exceptions

* **ClassNotFoundException**

  * Thrown when class is missing at runtime
  * Usually from `Class.forName()`

* **NoClassDefFoundError**

  * Class was present at compile time
  * Missing at runtime (dependency issue)

---

## 2. Runtime Data Areas (JVM Memory)

JVM defines **five memory areas**.

---

### 1️⃣ Method Area (Class Metadata)

Stores:

* Class structure
* Method metadata
* Static variables
* Runtime constant pool
* Bytecode

#### Java 8+ Change

* **PermGen removed**
* Replaced by **Metaspace**
* Metaspace:

  * Uses **native memory**
  * Grows dynamically
  * Can be capped using:

    ```
    -XX:MaxMetaspaceSize
    ```

---

### 2️⃣ Heap

Stores:

* Objects
* Instance variables
* Arrays

**Key points**

* Shared across threads
* Managed by **Garbage Collector**
* Tuned using:

  ```
  -Xms (initial heap)
  -Xmx (max heap)
  ```

Common error:

* `OutOfMemoryError: Java heap space`

---

### 3️⃣ Java Stack (Per Thread)

* Each thread has its own stack
* Stores **stack frames**

Each stack frame contains:

* Local variables
* Operand stack
* Method return info

**Error**

* `StackOverflowError`

  * Caused by deep or infinite recursion
  * Tuned via:

    ```
    -Xss
    ```

---

### 4️⃣ PC Register (Per Thread)

* Holds address of **next bytecode instruction**
* Required for thread switching

---

### 5️⃣ Native Method Stack

* Used for JNI (native C/C++ calls)
* Platform-dependent

---

### Memory Scope Summary

| Memory Area  | Scope      |
| ------------ | ---------- |
| Heap         | JVM-wide   |
| Method Area  | JVM-wide   |
| Java Stack   | Per thread |
| PC Register  | Per thread |
| Native Stack | Per thread |

---

## 3. Execution Engine

Responsible for **executing bytecode**

### Components

---

### 1️⃣ Interpreter

* Reads bytecode line-by-line
* Executes immediately
* Slower (no optimization)

---

### 2️⃣ JIT Compiler (Just-In-Time)

* Converts **hot bytecode** → native machine code
* Avoids repeated interpretation
* Improves performance drastically

---

### 3️⃣ HotSpot Profiler

* Identifies frequently executed code
* Feeds data to JIT compiler
* Enables aggressive optimizations

---

### 4️⃣ Garbage Collector

* Frees unused objects from Heap
* Different GC algorithms exist:

  * G1
  * ZGC
  * Shenandoah
  * Parallel GC

(Details usually asked separately)

---

### Native Interface

* Execution engine uses **JNI**
* Communicates with OS-level libraries:

  * `.dll` (Windows)
  * `.so` (Linux)

---

## Interview Snapshot (Must Remember)

> JVM has three main parts:
> **Class Loader**, **Runtime Data Areas**, and **Execution Engine**.
> Classes are loaded → verified → prepared → resolved → initialized.
> Objects live in Heap, metadata in Metaspace, stacks are per thread.
> Execution is optimized using JIT and HotSpot profiling.

---
