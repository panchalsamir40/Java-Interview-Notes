### Top 100 Java Interview Questions for Experienced Developers (2026 Edition)

Here is a curated list of **100 important Java interview questions** tailored for experienced developers (5–15+ years). These focus on advanced core Java concepts, JVM internals, concurrency, collections, modern features (Java 8–21+), design patterns, performance tuning, and real-world scenarios. Questions are categorized for better preparation, with brief explanations or key points where relevant.

#### OOP and Core Concepts (1–15)
### Answers to Java Interview Questions for Experienced Developers

Here are detailed explanations for the requested questions, based on Java features as of early 2026 (current LTS versions include Java 21 and Java 25, with Java 17 still supported).

1. **Explain the difference between composition and inheritance. Why is composition preferred in many cases?**  
   **Inheritance** ("is-a" relationship) allows a subclass to inherit fields and methods from a superclass, promoting code reuse through hierarchy. However, it creates tight coupling—the subclass depends heavily on the superclass implementation, making changes brittle (fragile base class problem).  
   **Composition** ("has-a" relationship) involves an object containing instances of other classes, delegating behavior to them. It offers greater flexibility: you can change composed objects at runtime, mix behaviors easily, and avoid inheritance depth issues.  
   Composition is preferred (as per "Effective Java" and GoF principles) for loose coupling, better testability (via mocks), and avoiding multiple inheritance pitfalls. Use inheritance sparingly for true is-a relationships; favor composition for most designs.

2. **What is the diamond problem in multiple inheritance, and how does Java handle it with interfaces?**  
   The diamond problem occurs in languages supporting multiple inheritance: a class inherits from two parents that both provide the same method, causing ambiguity (which version to use?).  
   Java avoids this for classes (single inheritance only), but interfaces allowed multiple inheritance. Pre-Java 8, no issue (interfaces had no implementations). Java 8+ added default methods, potentially creating conflicts.  
   Java resolves it by requiring explicit override in the implementing class if conflicts arise. Rules: class methods win over interfaces; if two interfaces conflict, the implementer must override. This forces resolution and avoids ambiguity.

3. **How does method overriding differ from method overloading? Provide scenarios where one is preferred.**  
   **Overriding** (runtime polymorphism): Subclass provides a specific implementation of a superclass method (same signature, return type covariant). Resolved at runtime via dynamic dispatch.  
   **Overloading** (compile-time polymorphism): Multiple methods in the same class with same name but different parameters (type/number/order). Resolved at compile time.  
   Scenarios:  
   - Overriding: Extending behavior (e.g., `Animal.speak()` overridden in `Dog` for "woof"). Preferred for polymorphic hierarchies.  
   - Overloading: Convenience APIs (e.g., `System.out.println()` variants for different types). Preferred for flexible method calls without subclasses.

4. **Explain abstraction vs encapsulation with real-world examples.**  
   **Abstraction**: Hiding complex implementation details, exposing only essential features (via abstract classes/interfaces). Focus: What an object does.  
   Example: Car driver uses steering wheel/pedals (interface) without knowing engine internals.  
   **Encapsulation**: Bundling data and methods, restricting access (private fields, public getters/setters). Focus: How data is protected.  
   Example: Bank account class with private balance; access via deposit/withdraw methods to prevent invalid states.  
   Together: Abstraction uses encapsulation to hide details.

5. **What are sealed classes in Java 17+? How do they improve code safety?**  
   Sealed classes (JEP 409, Java 17) restrict which classes can extend/implement them using the `sealed` modifier and `permits` clause.  
   Example:  
   ```java
   public sealed class Shape permits Circle, Rectangle, Triangle { }
   ```  
   They improve safety by enabling exhaustive pattern matching (e.g., in switch), reducing runtime errors from unhandled subtypes. Also aids maintainability—compilers enforce permitted subclasses.

6. **Describe the use of record classes (Java 16+). When would you use a record over a traditional class?**  
   Records (JEP 395, Java 16) are immutable data carriers: concise syntax for classes holding data (auto-generates constructor, getters, equals/hashCode/toString).  
   Example: `record Point(int x, int y) { }`  
   Use records for DTOs, value objects, or immutable data (e.g., API responses). Prefer over traditional classes when no mutable state/behavior needed—reduces boilerplate, ensures immutability, and provides canonical implementations.

7. **What are pattern matching for instanceof and switch (Java 14–21)? Show examples.**  
   **instanceof** (Java 16): Combines type check and cast.  
   Example:  
   ```java
   if (obj instanceof String s) { System.out.println(s.length()); }  // s scoped
   ```  
   **switch** (Java 21 final): Supports patterns, guarded cases, null handling.  
   Example:  
   ```java
   switch (obj) {
       case String s -> System.out.println(s.length());
       case Integer i when i > 0 -> System.out.println("Positive");
       case null -> System.out.println("Null");
       default -> System.out.println("Other");
   }
   ```  
   Reduces boilerplate, improves type safety/exhaustiveness.

8. **Explain virtual threads in Java 21. How do they differ from platform threads?**  
   Virtual threads (JEP 444, finalized in Java 21) are lightweight threads managed by JVM, not OS. They enable millions of concurrent tasks with low overhead.  
   **Platform threads**: 1:1 with OS threads—heavy (memory/creation cost), suitable for CPU-bound.  
   **Virtual threads**: Mounted on "carrier" platform threads; unmount during blocking (I/O), freeing carriers. Ideal for I/O-bound (e.g., servers).  
   Difference: Scalability—virtual threads simplify thread-per-request model without reactive complexity.

9. **What is the purpose of the permits clause in sealed classes/interfaces?**  
   The `permits` clause explicitly lists subclasses/subinterfaces allowed to extend/implement a sealed class/interface.  
   Example: `sealed interface Node permits Leaf, Branch { }`  
   Purpose: Enforces restricted hierarchy at compile time, enabling exhaustive analysis (e.g., pattern matching) and better domain modeling.

10. **How do String Templates (preview in Java 21) work? Compare with String formatting.**  
   String Templates were previewed in Java 21/22 (JEP 430/459) for safe interpolation: `STR."Value: \{value}"` with processors for validation (e.g., SQL escaping).  
   However, the feature was withdrawn in Java 23+ for redesign due to community feedback. As of 2026, it's not standard—use alternatives.  
   Comparison: vs `String.format("%s", value)` or `+` concatenation—templates aimed for type-safety/security, but current options lack runtime validation.

11. **What are Sequenced Collections in Java 21? Why were they introduced?**  
   Introduced in Java 21 (JEP 431): New interfaces (`SequencedCollection`, `SequencedSet`, `SequencedMap`) for collections with defined encounter order. Add methods like `getFirst()`, `getLast()`, `reversed()`.  
   Retro-fitted (e.g., List implements SequencedCollection).  
   Why: Unified API for ordered collections (previously scattered: Deque for ends, no reverse view). Simplifies accessing first/last/reverse across List, LinkedHashSet/Map, SortedSet/Map.

12. **Explain the Foreign Function & Memory API (Java 22 preview). Use cases?**  
   Finalized in Java 22 (JEP 454, after previews): Safe, efficient access to off-heap memory and native code (replacement for JNI).  
   Uses arenas for memory management, downcalls/upcalls for native functions.  
   Use cases: High-performance native interop (e.g., calling C libraries), off-heap data processing (faster than ByteBuffer), systems programming without JNI brittleness.

13. **How does Java handle immutability? Best practices for immutable classes.**  
   Java supports via `final` fields, no setters, defensive copies. Records (Java 16+) are inherently immutable.  
   Best practices (Joshua Bloch):  
   - All fields `private final`.  
   - No mutators.  
   - Immutable dependencies.  
   - Defensive copies for mutable inputs/outputs.  
   - Factory methods if needed.

14. **What is covariance and contravariance in generics? Provide examples.**  
   **Covariance**: Subtype arrays allowed where supertype expected (unsafe: `Number[] n = new Integer[1]; n[0] = 1.5;` throws exception). Generics: Use `? extends T` (get-only).  
   Example: `List<? extends Number> list = new ArrayList<Integer>();`  
   **Contravariance**: `? super T` (put-only).  
   Example: `List<? super Integer> list = new ArrayList<Number>(); list.add(5);`  
   PECS: Producer Extends, Consumer Super.

15. **Explain type erasure in generics. What are its implications and workarounds (e.g., reified generics)?**  
   Type erasure: Generics compiled to raw types (bounds to Object/casts); erased at runtime for backward compatibility.  
   Implications: No runtime type info (e.g., can't `new T()`, `instanceof List<String>` illegal). Limits reflection.  
   Workarounds: Pass Class<T>, super type tokens, or frameworks (e.g., Jackson). No full reified generics yet (Project Valhalla exploring).

#### Collections Framework (16–30)

### Answers to Java Collections Interview Questions (16–30)

Here are detailed, experienced-level answers to the requested questions on Java Collections Framework, focusing on internals, concurrency, performance, and best practices (as of Java 21+ in 2026).

16. **How does HashMap work internally in Java 8+ (bucket, treeification)?**  
   HashMap uses an array of buckets (Node[] table). The index for a key is calculated as `(hashCode() & (table.length - 1))` after perturbing the hash for better distribution.  
   Each bucket holds a linked list of entries (Node<K,V>).  
   **Java 8+ improvement (treeification)**: If a bucket's linked list exceeds **TREEIFY_THRESHOLD (8)** entries and the table capacity is at least **MIN_TREEIFY_CAPACITY (64)**, the linked list is converted to a balanced red-black tree (TreeNode) for O(log n) operations instead of O(n).  
   On shrinkage or removal, it may **degenerate** back to a linked list if nodes fall below **UNTREEIFY_THRESHOLD (6)**.  
   This balances worst-case performance during hash collisions (e.g., poor hashCode).

17. **Difference between ConcurrentHashMap and Hashtable? Segment locking in older versions.**  
   | Feature                  | Hashtable                          | ConcurrentHashMap (Java 8+)                  |
   |--------------------------|------------------------------------|----------------------------------------------|
   | Synchronization          | All methods synchronized (coarse-grained lock) | Fine-grained locking (per bucket + CAS)     |
   | Nulls                    | No null keys/values                | Allows null values (not keys) in some ops    |
   | Performance              | Poor under high concurrency (single lock) | High scalability (multiple threads)         |
   | Iterator                 | Fail-fast                          | Fail-safe (weakly consistent)                |
   | Legacy                   | Old (Java 1.0)                     | Modern (Java 5+, improved in 7/8)            |

   Older ConcurrentHashMap (Java 7) used **segment locking** (16 segments by default, each with its own lock) for better concurrency. Java 8+ replaced it with finer-grained locking on individual buckets using CAS (compare-and-swap) and synchronized only when needed, plus treeification support.

18. **When does HashMap convert linked list to red-black tree? Thresholds?**  
   Conversion (treeification) happens when:  
   - Number of nodes in a bucket ≥ **8** (TREEIFY_THRESHOLD)  
   - AND table capacity ≥ **64** (MIN_TREEIFY_CAPACITY)  

   If capacity < 64, it resizes instead.  
   Degeneration back to linked list when nodes ≤ **6** (UNTREEIFY_THRESHOLD).  
   These thresholds prevent excessive tree creation in small maps and handle hash collision attacks.

19. **Explain fail-fast vs fail-safe iterators with examples.**  
   **Fail-fast**: Throws `ConcurrentModificationException` if the collection is structurally modified (add/remove) during iteration (except via iterator's own remove()). Uses modCount check.  
   Examples: ArrayList, HashMap, HashSet iterators.  
   **Fail-safe**: No exception on modification; works on a snapshot/clone or allows concurrent mods without failing.  
   Examples:  
   - CopyOnWriteArrayList (iterates over immutable snapshot)  
   - ConcurrentHashMap (weakly consistent view)  

   Fail-safe is safer in concurrent environments but may have higher memory overhead or stale data.

20. **How does CopyOnWriteArrayList achieve thread safety? Trade-offs?**  
   It uses **immutable snapshot + copy-on-write**:  
   - Underlying array is `volatile`.  
   - Reads (iteration, get) operate directly on current array (no locking).  
   - Writes (add, set, remove) lock, create a new array copy with modification, then assign via volatile write.  

   **Trade-offs**:  
   - Excellent for **read-heavy** scenarios (iteration is fast and thread-safe).  
   - Very expensive writes (O(n) copy).  
   - High memory usage (temporary double storage during writes).  
   - Iterators see snapshot (may miss latest changes).  
   Ideal: Event listeners, observer lists.

21. **Difference between ArrayList, LinkedList, and Vector.**  
   | Feature               | ArrayList                  | LinkedList                         | Vector                          |
   |-----------------------|----------------------------|------------------------------------|---------------------------------|
   | Internal structure    | Dynamic array              | Doubly-linked list                 | Dynamic array (like ArrayList) |
   | Random access         | O(1)                       | O(n)                               | O(1)                            |
   | Insertion/deletion    | Slow in middle (O(n))      | Fast (O(1) if node known)          | Slow in middle                  |
   | Thread safety         | Not thread-safe            | Not thread-safe                    | Synchronized methods            |
   | Performance           | Best for reads             | Best for frequent add/remove       | Slower due to sync overhead     |
   | Legacy                | Preferred modern choice    | Use as Deque                       | Legacy (avoid, use Collections.synchronizedList) |

22. **How does TreeMap maintain ordering? Comparator vs Comparable.**  
   TreeMap uses a **red-black tree** (self-balancing BST) to store entries sorted by keys.  
   - Natural ordering: Keys must implement `Comparable` (compareTo).  
   - Custom ordering: Provide `Comparator` in constructor.  
   Operations (put, get, remove) are O(log n).  
   Null keys allowed only with custom Comparator that handles nulls.

23. **What is WeakHashMap? Real-world use cases (e.g., caching).**  
   WeakHashMap uses **weak references** for keys (via WeakReference). When a key has no strong references elsewhere, the entry is automatically removed by GC.  
   Values are strongly referenced until key is collected.  
   **Use cases**:  
   - Memory-sensitive caches (e.g., metadata cache where entries should expire when keys are no longer used).  
   - Canonicalizing mappings (e.g., intern-like behavior).  
   Not for general caching (use Caffeine/Guava instead).

24. **Explain IdentityHashMap. When to use reference equality (==)?**  
   IdentityHashMap uses **reference equality (==)** for keys instead of `equals()`, and `System.identityHashCode()` instead of `hashCode()`.  
   Violates general Map contract intentionally.  
   **Use cases**:  
   - Object graph traversal (e.g., serialization, detecting cycles).  
   - Tracking objects by identity (e.g., in debugging tools).  
   - When logical equality is irrelevant (rare).

25. **Performance comparison: HashSet vs TreeSet vs LinkedHashSet.**  
   | Feature               | HashSet                    | TreeSet                            | LinkedHashSet                   |
   |-----------------------|----------------------------|------------------------------------|---------------------------------|
   | Order                 | No order                   | Sorted (natural/custom)            | Insertion order                 |
   | Add/Contains/Remove   | O(1) average               | O(log n)                           | O(1) average                    |
   | Memory overhead       | Lowest                     | Higher (tree nodes)                | Slightly higher (linked list)   |
   | Null allowed          | Yes                        | No (unless Comparator allows)      | Yes                             |
   | Best for              | Fast lookup, no order      | Sorted unique elements             | Insertion-ordered unique        |

26. **How to make a custom object usable as a HashMap key? (hashCode/equals contract)**  
   Override `equals()` and `hashCode()` consistently:  
   - If two objects are equal (`a.equals(b)`), they **must** have same hashCode.  
   - Consistent: Multiple calls return same hashCode (if fields unchanged).  
   - Good distribution: Different objects should have different hashCodes.  
   Use immutable fields in computation.  
   Tools: Lombok @EqualsAndHashCode, Objects.hash(), or IDE generation.  
   Violation causes lost entries or poor performance.

27. **What happens on hash collisions in HashMap?**  
   Colliding entries (same bucket index) are stored in a linked list (or tree post-Java 8).  
   - First: Added to list head.  
   - Search: Linear scan (or tree traversal if treeified).  
   - Worst case: O(n) if many collisions (e.g., bad hashCode).  
   Treeification mitigates this for large collisions.

28. **Explain ConcurrentSkipListMap. When to use over ConcurrentHashMap?**  
   ConcurrentSkipListMap is a scalable, thread-safe sorted map using **concurrent skip list** (multi-level index over linked list).  
   Provides O(log n) operations with high concurrency (lock-free reads, fine-grained writes).  
   **Use over ConcurrentHashMap when**:  
   - You need sorted order (natural or Comparator).  
   - Range queries, navigation (higherKey, subMap).  
   - High concurrency + ordering required (e.g., leaderboards, priority maps).  
   Trade-off: Slightly slower than ConcurrentHashMap for pure put/get.

29. **Difference between remove() in Iterator vs Collection.**  
   - `Iterator.remove()`: Safely removes current element during iteration (updates modCount correctly, no CME).  
   - `Collection.remove(obj)` during iteration: Structural modification → throws ConcurrentModificationException in fail-fast collections (ArrayList, HashMap).  
   Always prefer Iterator.remove() in loops.

30. **How to sort a Map by values efficiently?**  
   ```java
   List<Map.Entry<K, V>> list = new ArrayList<>(map.entrySet());
   list.sort(Map.Entry.comparingByValue());  // or custom Comparator

   // Preserve order in new map
   LinkedHashMap<K, V> sortedMap = new LinkedHashMap<>();
   for (Map.Entry<K, V> entry : list) {
       sortedMap.put(entry.getKey(), entry.getValue());
   }
   ```  
   Efficient: O(n log n) time, O(n) space.  
   Alternative: Use Stream (Java 8+):  
   ```java
   map.entrySet().stream()
      .sorted(Map.Entry.comparingByValue())
      .collect(Collectors.toMap(
          Map.Entry::getKey, Map.Entry::getValue,
          (e1, e2) -> e1, LinkedHashMap::new));
   ```  
   Note: If values not Comparable, provide Comparator.

These answers cover deep internals expected in senior Java interviews. Practice explaining with diagrams (buckets, trees) and real scenarios. Good luck!

#### Multithreading & Concurrency (31–50)
### Answers to Java Multithreading & Concurrency Questions (31–50)

Detailed explanations for experienced developers, including internals, best practices, and modern Java features (up to Java 21+ as of 2026).

#### 31. Explain the Java Memory Model (JMM). Happens-before relationship.
The **Java Memory Model (JMM)** (defined in JLS Chapter 17) specifies how threads interact through memory—rules for when a write by one thread is visible to a read by another.  
- Without JMM, compilers/JVMs could reorder instructions aggressively, breaking multithreaded code.  
- Key guarantees: **visibility**, **ordering**, and **atomicity** for variables.  

**Happens-before relationship**: Partial ordering of actions. If action A happens-before B, then results of A are visible to B.  
Key happens-before rules:  
- Program order (within thread).  
- Monitor lock: unlock → subsequent lock on same monitor.  
- Volatile: write → subsequent read.  
- Thread start: start() → first action in thread.  
- Thread join: last action in thread → join returns.  
- Final field initialization → first read (special guarantee).  
- Transitivity.  

JMM enables safe publication (e.g., immutable objects) and avoids data races.

#### 32. What is volatile keyword? How does it differ from synchronized?
**volatile**: Ensures **visibility** and **ordering** (no reordering across volatile reads/writes).  
- Write: Flushes to main memory.  
- Read: Loads fresh from main memory.  
- No atomicity for compound actions (e.g., `i++` not atomic).  

**vs synchronized**:  
| Aspect               | volatile                              | synchronized                          |
|----------------------|---------------------------------------|---------------------------------------|
| Mutual exclusion     | No                                    | Yes                                   |
| Atomicity            | Only single read/write                | Full block                            |
| Visibility           | Yes                                   | Yes                                   |
| Performance          | Cheaper (no lock)                     | Expensive (monitor enter/exit)        |
| Use case             | Flags, one-time publication           | Critical sections, compound actions   |

Example: `volatile boolean running` safely stops threads. Use volatile for simple flags; synchronized for complex state.

#### 33. Difference between Runnable and Callable. Role in ExecutorService.
| Feature              | Runnable                              | Callable<V>                           |
|----------------------|---------------------------------------|---------------------------------------|
| Method               | void run()                            | V call() throws Exception             |
| Return value         | None                                  | V                                     |
| Exceptions           | Must handle internally                | Can throw checked                     |

**ExecutorService role**:  
- `execute(Runnable)`: Fire-and-forget (no result).  
- `submit(Runnable/Callable)`: Returns `Future<V>` (or `Future<Void>`).  
Callable enables async computation with results/exceptions (basis for CompletableFuture).

#### 34. How does ThreadPoolExecutor work? Core parameters and rejection policies.
**ThreadPoolExecutor** manages a pool of worker threads + task queue.  
Lifecycle: Threads created on demand, idle threads terminated after keep-alive.  

**Core parameters**:  
- `corePoolSize`: Minimum threads kept alive.  
- `maximumPoolSize`: Max threads (including queued).  
- `keepAliveTime`: Idle thread survival beyond core.  
- `workQueue`: BlockingQueue (e.g., LinkedBlockingQueue, SynchronousQueue).  
- `threadFactory`, `RejectedExecutionHandler`.  

**Execution flow**:  
1. Task submitted → if < core → new thread.  
2. Else → offer to queue.  
3. Queue full → create up to max threads.  
4. Max reached + full → reject.  

**Rejection policies** (RejectedExecutionHandler):  
- AbortPolicy (default): Throw RejectedExecutionException.  
- CallerRunsPolicy: Caller thread runs task (feedback loop).  
- DiscardPolicy: Silent drop.  
- DiscardOldestPolicy: Drop oldest queued.  
Custom possible.

#### 35. Explain deadlock, livelock, and starvation. Detection and prevention.
- **Deadlock**: Threads wait forever for locks held cyclically (e.g., T1 holds L1 waits L2; T2 holds L2 waits L1).  
- **Livelock**: Threads actively retry but never progress (e.g., two polite people stepping aside repeatedly).  
- **Starvation**: Thread never gets CPU/lock due to scheduling (e.g., low-priority or unfair locks).  

**Detection**: ThreadMXBean.findDeadlockedThreads(), jstack dumps.  
**Prevention**:  
- Lock ordering (always acquire in same order).  
- Try-lock with timeout (tryLock()).  
- Avoid nested locks.  
- Use higher-level concurrency (e.g., ConcurrentHashMap).  
- Fair locks (ReentrantLock(true)).  

#### 36. What is ReentrantLock? Advantages over synchronized.
**ReentrantLock**: Explicit mutual exclusion lock (java.util.concurrent.locks).  
- Reentrant (same thread can reacquire).  
- Supports fairness, tryLock(), lockInterruptibly().  

**Advantages over synchronized**:  
- Try lock without blocking.  
- Interruptible lock acquisition.  
- Timed lock waits.  
- Fairness policy (prevents starvation).  
- Condition support (multiple Conditions vs wait/notify).  
- Better diagnostics (isLocked(), getOwner()).  

Use synchronized for simple cases; ReentrantLock for advanced control.

#### 37. Difference between ForkJoinPool and common ForkJoinPool in Java 8+.
**ForkJoinPool**: Designed for divide-and-conquer (work-stealing). Each worker has deque; steals from others when idle.  
**commonPool()** (Java 8+): Static shared instance (parallel streams default).  
- Size: `Runtime.availableProcessors() - 1` (leaves one for main).  
- Daemon threads, managed lifecycle.  

**Custom ForkJoinPool**: For isolation (e.g., avoid stream interference).  
```java
ForkJoinPool custom = new ForkJoinPool(4);
custom.submit(task);
```
Common pool convenient but can cause thread starvation if tasks block.

#### 38. How does CompletableFuture improve over Future? Key methods (thenCompose, thenCombine).
**Future limitations**: Blocking get(), no chaining, no easy composition.  

**CompletableFuture improvements**:  
- Non-blocking chaining (CompletionStage).  
- Manual completion.  
- Async execution stages.  

**Key methods**:  
- `thenCompose`: Flatten nested futures (chain dependent async calls).  
  ```java
  cf1.thenCompose(result1 -> supplyAsync(() -> f(result1)))
  ```  
- `thenCombine`: Combine independent futures.  
  ```java
  cf1.thenCombine(cf2, (r1, r2) -> r1 + r2)
  ```  
Others: thenApply, exceptionally, allOf, anyOf.

#### 39. Explain Structured Concurrency (Java 19+ preview). Benefits?
**Structured Concurrency** (JEP 428/453, incubator/preview until Java 22+): Treat groups of related tasks as single unit.  
- Uses **StructuredTaskScope** (Java 21+ preview): Forks sub-tasks, joins all, propagates errors.  
- Ensures no orphaned threads, clear lifecycle.  

**Benefits**:  
- Prevents leaks.  
- Easier error handling (shutdown on failure).  
- Better observability.  
- Simplifies cancellation.  
Alternative to unstructured ExecutorService sprawl.

#### 40. What are virtual threads (Project Loom, Java 21)? Carrier threads vs virtual.
**Virtual threads** (JEP 444, Java 21): Lightweight threads (~1KB stack) managed by JVM, not OS.  
- Millions possible vs thousands of platform threads.  
- **Carrier threads**: Actual OS/platform threads (pool) that run virtual threads.  
- During blocking (I/O), virtual thread **unmounts** → carrier freed for others.  

Ideal for thread-per-request servers (simple code vs reactive).  
```java
Thread.startVirtualThread(runnable);
```

#### 41. How to propagate context (e.g., MDC, SecurityContext) in async tasks?
Default: Context lost on new threads.  
**Solutions**:  
- **TaskDecorator** (Spring): Wrap runnable.  
  ```java
  executor.setTaskDecorator(() -> {
      Map<String,String> mdc = MDC.getCopyOfContextMap();
      return () -> {
          MDC.setContextMap(mdc);
          // task.run()
      };
  });
  ```  
- **CompletableFuture**: Use custom Executor or manually copy.  
- **Virtual threads (Java 21+)**: Thread-local inheritance possible (preview).  
- Libraries: ContextSnapshot (Micronaut), Scoped Values (Java 21+ preview).

#### 42. Difference between @Async in Spring and CompletableFuture.
| Aspect               | Spring @Async                         | CompletableFuture                     |
|----------------------|---------------------------------------|---------------------------------------|
| Framework            | Spring-specific                       | Pure Java 8+                          |
| Configuration        | @EnableAsync + TaskExecutor           | No setup needed                       |
| Result handling      | Future/CompletableFuture              | Built-in chaining                     |
| Context propagation  | TaskDecorator support                 | Manual or Scoped Values               |
| Exception handling   | AsyncUncaughtExceptionHandler         | exceptionally/handle                  |
| Use case             | Spring apps simplicity                | General async composition             |

Spring @Async hides boilerplate; CompletableFuture more flexible/powerful.

#### 43. Explain CountDownLatch vs CyclicBarrier vs Semaphore.
| Class                | Purpose                               | Reusable? | Parties wait? |
|----------------------|---------------------------------------|-----------|---------------|
| CountDownLatch       | One-time: Wait for N events           | No        | Yes (await)   |
| CyclicBarrier        | Wait for fixed threads mutually       | Yes       | Yes           |
| Semaphore            | Limit concurrent access (permits)     | Yes       | Optional      |

- Latch: Coordinator → workers countDown().  
- Barrier: All threads rendezvous.  
- Semaphore: Throttling (acquire/release).

#### 44. What is the double-checked locking idiom? Why is it broken without volatile?
**DCL** for lazy singleton:  
```java
if (instance == null) {           // 1
    synchronized(lock) {
        if (instance == null) {   // 2
            instance = new Singleton(); // 3
        }
    }
}
```
**Broken without volatile**: JVM may reorder 3 (allocate → initialize) → another thread sees partially constructed object (non-null but fields default).  
**volatile** prevents reordering + ensures visibility.

#### 45. How does StampedLock work? Optimistic reading.
**StampedLock**: Non-reentrant read/write lock with optimistic read.  
- `writeLock()` → stamp, exclusive.  
- `tryOptimisticRead()` → stamp (no lock).  
- `validate(stamp)` → check if write occurred since.  

If invalid → fallback to pessimistic `readLock()`.  
**Advantages**: High read concurrency (no blocking).  
**Disadvantages**: No reentrancy, manual validation.

#### 46. Explain ReadWriteLock. When to prefer over synchronized.
**ReadWriteLock**: Separate read (shared) and write (exclusive) locks.  
- Multiple readers concurrent.  
- Writer exclusive.  

**Prefer over synchronized**: High read-heavy workloads (e.g., caches).  
ReentrantReadWriteLock implementation (fairness option).  
**Caveat**: Writers can starve readers without fairness.

#### 47. What causes ThreadLocal memory leaks? Best practices.
**Leaks**: ThreadLocal holds strong ref to value; if thread lives long (thread pools), value unreachable only after thread death.  
- Common in web apps (request threads reused).  

**Best practices**:  
- Always `threadLocal.remove()` in finally (or use try-with-resources wrappers).  
- Use framework support (Spring: RequestContextHolder clears).  
- Avoid in long-lived pools.  
- Scoped Values (Java 21+) as immutable alternative.

#### 48. Difference between submit() and execute() in ExecutorService.
| Method               | Return                               | Exception handling                    |
|----------------------|--------------------------------------|---------------------------------------|
| execute(Runnable)    | void                                 | Uncaught → Thread.setUncaughtExceptionHandler |
| submit(Runnable/Callable) | Future<?> / Future<V>           | Wrapped in ExecutionException         |

**submit** preferred for result tracking and exception propagation.

#### 49. How to handle uncaught exceptions in threads/CompletableFuture?
**Threads**:  
- `Thread.setUncaughtExceptionHandler()` (per thread or default).  
- Override `ThreadGroup.uncaughtException()`.  
- Executor: `ThreadPoolExecutor.setRejectedExecutionHandler` + custom ThreadFactory.  

**CompletableFuture**:  
- `exceptionally(fn)` or `handle((res, ex) -> ...)`.  
- `CompletableFuture.allOf(...).join()` propagates first exception.  
- Global: `CompletableFuture.setDefaultExecutor` + custom.

#### 50. Explain the happens-before guarantees for final fields.
**Final fields**: Once constructor finishes, values guaranteed visible to other threads without synchronization (freeze guarantee).  
- No reordering of final writes after constructor.  
- Enables safe publication of immutable objects (e.g., `public final List<String> items`).  
- Does NOT apply if reference escapes constructor (this escape).  
Key for effective immutability pattern.

#### Exception Handling & JVM Internals (51–65)

51. Difference between checked and unchecked exceptions. Best practices.

52. How does try-with-resources work internally (AutoCloseable)?

53. Explain OutOfMemoryError vs StackOverflowError. Causes and fixes.

54. What are the different memory areas in JVM (Heap, Stack, Metaspace)?

55. Difference between young and old generation GC.

56. Explain G1 vs ZGC vs Shenandoah garbage collectors.

57. How does Generational ZGC (Java 21) improve performance?

58. What is escape analysis? Impact on allocation.

59. Explain class loading mechanism. Parent delegation model.

60. What are JVM flags for tuning GC (-XX:+UseG1GC, etc.)?

61. How to analyze heap dumps (jmap, Eclipse MAT)?

62. Difference between minor, major, and full GC.

63. What is metaspace? Difference from PermGen.

64. Explain JIT compilation phases (C1, C2 compilers).

65. How does biased locking work? Revocation issues.

#### Java 8+ Features & Streams (66–80)

66. Explain lambda expressions and functional interfaces.

67. What are default and static methods in interfaces (Java 8)?

68. How does Stream API work? Lazy vs eager operations.

69. Difference between map() and flatMap() in Streams.

70. Explain Collectors: groupingBy, partitioningBy, toMap.

71. What are optional methods? Best practices to avoid NullPointerException.

72. How do parallel streams work? When to avoid them.

73. Explain the Spliterator interface.

74. Difference between intermediate and terminal operations.

75. What are method references? Types (:: syntax).

76. Explain record patterns in deconstruction (Java 19+).

77. How does pattern matching for switch (Java 21) handle null?

78. What are unnamed patterns and variables (Java 21+)?

79. Explain the new HTTP Client in Java 11+.

80. Virtual threads impact on reactive programming (e.g., WebFlux).

#### Design Patterns, Best Practices & Miscellaneous (81–100)

81. Explain Singleton pattern variants (eager, lazy, enum).

82. Factory vs Abstract Factory vs Builder pattern.

83. When to use Observer pattern? Java implementation (PropertyChangeSupport).

84. Explain Decorator pattern. Relation to Java IO classes.

85. What is Dependency Injection? Benefits in large apps.

86. Strategy pattern vs State pattern.

87. Explain Proxy pattern. Dynamic proxies in Java.

88. How to implement circuit breaker (Resilience4j)?

89. Best practices for equals() and hashCode().

90. Explain immutable objects design (Joshua Bloch guidelines).

91. What are modules in Java 9+ (JPMS)? Benefits and migration issues.

92. How to handle module accessibility (opens, exports).

93. Difference between String, StringBuilder, StringBuffer.

94. Explain intern() method in String.

95. What is the contract for compareTo() in Comparable?

96. How to debug performance issues (jstack, jvisualvm)?

97. Explain microbenchmarking with JMH.

98. What are var (local variable type inference) limitations (Java 10+)?

99. Explain text blocks (Java 15). Advantages.

100. How has Java evolved for cloud-native (e.g., GraalVM native images)?
