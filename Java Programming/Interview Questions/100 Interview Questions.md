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

Here are detailed, up-to-date answers (as of January 2026) to the **Exception Handling & JVM Internals** questions (51–65) that are commonly asked in senior Java developer interviews.

### 51. Difference between checked and unchecked exceptions. Best practices.

| Aspect                  | Checked Exceptions                              | Unchecked Exceptions                           |
|-------------------------|-------------------------------------------------|------------------------------------------------|
| Inheritance             | Extend `Exception` (but not `RuntimeException`) | Extend `RuntimeException` or `Error`           |
| Compile-time check      | Must be declared in `throws` or caught          | No compile-time obligation                     |
| Purpose                 | Recoverable conditions (external problems)      | Programming errors or irrecoverable situations |
| Examples                | `IOException`, `SQLException`, `ParseException` | `NullPointerException`, `IllegalArgumentException`, `OutOfMemoryError` |
| When thrown             | Usually by I/O, network, DB operations          | Usually by application logic bugs              |

**Best practices (2025–2026)**:
- Use **checked** only for truly recoverable situations where the caller is expected to handle it (e.g., file not found → ask user for new path).
- Prefer **unchecked** (custom `RuntimeException` subclasses) for almost everything else → cleaner code, less try-catch clutter.
- Wrap checked exceptions in unchecked ones when deep in call stack (e.g., `new RuntimeException("Failed to read config", ex)`).
- Avoid `throws Exception` or `throws Throwable` — be specific.
- Use `record` + pattern matching for rich custom exceptions (Java 21+).

### 52. How does try-with-resources work internally (AutoCloseable)?

`try-with-resources` (introduced in Java 7) automatically closes resources that implement `AutoCloseable` (or its subtype `Closeable`).

**Internal transformation** (compiler desugaring):
```java
try (Resource r = new Resource()) {
    // use r
}
```

becomes roughly:
```java
Resource r = new Resource();
Throwable suppressed = null;
try {
    // use r
} catch (Throwable t) {
    suppressed = t;
    throw t;
} finally {
    if (r != null) {
        try {
            r.close();
        } catch (Throwable t) {
            if (suppressed != null) {
                suppressed.addSuppressed(t);
            } else {
                throw t;
            }
        }
    }
}
```

**Key points**:
- Multiple resources → closes in **reverse order** of declaration.
- `close()` exceptions are **suppressed** (added via `addSuppressed()`) — primary exception remains primary.
- Works with any `AutoCloseable` (not just I/O).
- Java 9+ allows effectively final variables: `try (existingResource) { ... }`

### 53. Explain OutOfMemoryError vs StackOverflowError. Causes and fixes.

| Error Type              | OutOfMemoryError                                   | StackOverflowError                               |
|-------------------------|----------------------------------------------------|--------------------------------------------------|
| Thrown when             | JVM cannot allocate more memory (usually heap)     | Thread stack space exhausted                     |
| Main causes             | - Too many objects<br>- Memory leaks<br>- Large single allocation<br>- Metaspace exhaustion | - Deep/infinite recursion<br>- Very large stack frames |
| Subtypes                | Java heap space, GC overhead limit, Metaspace, Native memory, etc. | No subtypes                                      |
| Typical fixes           | - Increase -Xmx<br>- Fix leaks (heap dump analysis)<br>- Use off-heap / direct buffers<br>- Optimize data structures | - Increase -Xss (per thread stack size)<br>- Refactor recursion → iteration / stack-based<br>- Reduce local variables / object size |

**Note**: Both are `Error` (unchecked) — catching them is rarely useful except for recovery logging.

### 54. What are the different memory areas in JVM (Heap, Stack, Metaspace)?

Modern HotSpot JVM (Java 21–25) memory areas:

| Area            | Managed by     | Contents                                      | Shared / Per-thread | GC'ed?     | Size control flags                  |
|-----------------|----------------|-----------------------------------------------|----------------------|------------|-------------------------------------|
| Heap            | JVM            | All objects and arrays                        | Shared               | Yes        | -Xms, -Xmx                          |
| Metaspace       | Native         | Class metadata, interned strings, annotations | Shared               | Yes (full GC) | -XX:MetaspaceSize, MaxMetaspaceSize |
| Thread Stacks   | Native         | Method call frames, local variables, params   | Per-thread           | No         | -Xss (per thread)                   |
| Code Cache      | Native         | JIT-compiled code                             | Shared               | Yes (sweep) | -XX:ReservedCodeCacheSize           |
| Compressed Oops | -              | Pointers in heap (compressed in 64-bit)       | -                    | -          | -XX:+UseCompressedOops              |
| Direct Memory   | Application    | Off-heap (ByteBuffer.allocateDirect)          | Shared               | Manual     | -XX:MaxDirectMemorySize             |

**Important**: Since Java 8, PermGen → Metaspace (native memory, auto-sizing).

### 55. Difference between young and old generation GC

Based on **generational hypothesis** — most objects die young.

| Aspect                 | Young Generation (Minor GC)                  | Old Generation (Major GC)                     |
|------------------------|----------------------------------------------|-----------------------------------------------|
| Contains               | Eden + 2 Survivor spaces                     | Tenured / Old space                           |
| Object lifetime        | Short-lived                                  | Long-lived (survived multiple minor GCs)      |
| Collection frequency   | Very frequent                                | Much less frequent                            |
| Collection type        | Copying collector (fast)                     | Mark-Sweep-Compact or concurrent variants     |
| Pause time             | Usually very short                           | Longer (especially in G1/ZGC reduced)         |
| Promotion              | Survivors → tenured after threshold          | -                                             |

**Promotion**: After surviving several minor GCs (controlled by `-XX:MaxTenuringThreshold`, default 15), objects move to old generation.

### 56. Explain G1 vs ZGC vs Shenandoah garbage collectors

All are low-pause, concurrent collectors (Java 21–25 era):

| Collector       | Default since | Pause target       | Throughput impact | Best for                          | Key technique                          |
|-----------------|---------------|--------------------|-------------------|-----------------------------------|----------------------------------------|
| **G1**          | Java 9        | ~200 ms (tunable)  | Good              | General purpose, < ~32GB heaps    | Region-based, mixed young+old collections |
| **ZGC**         | Production-ready Java 15 | <1 ms (even TB heaps) | ~10–20% lower than G1 | Latency-sensitive, large heaps    | Colored pointers + load barriers       |
| **Shenandoah**  | Java 12 (Red Hat) | <10 ms             | Slightly better than ZGC | Latency-sensitive, large heaps    | Forwarding pointers + concurrent evac  |

**2026 recommendation**:
- **G1** — still the safe default for most applications
- **ZGC** — when you need sub-millisecond p99.99 latency (cloud, trading, streaming)
- **Shenandoah** — good alternative if ZGC throughput is too low

### 57. How does Generational ZGC (Java 21) improve performance?

**Generational ZGC** (JEP 439, production-ready in Java 21) added young/old generation separation to the originally non-generational ZGC.

**Major improvements**:
- Much faster collection of short-lived objects (young generation)
- Reduced allocation rate pressure on old generation
- Lower average pause times (often 10–30 µs p99 instead of ~200–500 µs)
- Better throughput in allocation-heavy workloads (closer to G1)
- Still keeps max pause <1 ms (even for multi-TB heaps)

**Trade-off**: Slightly higher allocation overhead due to generational barriers, but in practice most workloads show 5–20% better throughput than non-generational ZGC.

**Enable**: `-XX:+UseZGC -XX:+ZGenerational`

### 58. What is escape analysis? Impact on allocation.

**Escape analysis** — JIT compiler optimization that determines whether an object **escapes** the method/thread scope.

**Outcomes**:
1. **No escape** → object can be **allocated on stack** (or eliminated completely via scalar replacement)
2. **Partial escape** → some fields can be optimized
3. **Escapes** → normal heap allocation

**Impact**:
- Stack allocation → no GC pressure, faster creation/death
- Scalar replacement → completely removes allocation (fields become locals)
- Common in temporary objects (builders, iterators, lambdas)

**Escape analysis** is a compiler optimization used by the **Java HotSpot JVM** to determine **whether an object “escapes” the scope in which it is created** (usually a method or a thread).

In simple terms, it answers this question:

> *Can this object be accessed outside the current method or thread?*

---

## What does “escape” mean?

An object **escapes** if a reference to it can be used **outside** its original scope.

### Levels of escape

1. **No escape** – Object is used only inside the method.
2. **Method escape** – Object is returned or passed to another method.
3. **Thread escape** – Object is shared across threads (e.g., stored in a static field).

---

## Example

```java
public void foo() {
    Point p = new Point(1, 2);  // Does not escape
    System.out.println(p.x);
}
```

Here, `Point` **does not escape** the method `foo()`.

```java
public Point bar() {
    return new Point(1, 2);  // Escapes the method
}
```

Here, the object **escapes** because it’s returned.

---

## Impact on Allocation (Why Escape Analysis Matters)

Escape analysis allows the JVM to **optimize memory allocation and synchronization**.

### 1. Stack Allocation (instead of Heap)

* Normally, **all objects are allocated on the heap**
* If the JVM proves an object **does not escape**, it may allocate it **on the stack**
* Stack allocation is:

  * Faster
  * Automatically freed when the method exits
  * No GC overhead

✔ **Result:** Better performance, less garbage collection

---

### 2. Scalar Replacement (Object Elimination)

The JVM may **remove the object entirely** and replace it with individual fields.

```java
void foo() {
    Point p = new Point(1, 2);
    int sum = p.x + p.y;
}
```

After escape analysis, this can become (conceptually):

```java
int x = 1;
int y = 2;
int sum = x + y;
```

✔ **Result:**

* No object allocation at all
* No memory usage for the object

---

### 3. Lock Elimination

If an object does not escape a thread, synchronization is unnecessary.

```java
void foo() {
    synchronized (new Object()) {
        // work
    }
}
```

The JVM can remove the `synchronized` block entirely.

✔ **Result:**

* Faster execution
* No locking overhead

---

## When Escape Analysis Is Applied

* Performed by the **JIT (Just-In-Time) compiler**
* Happens **at runtime**, not compile time
* Enabled by default in modern JVMs

### JVM flags (for learning/debugging)

```bash
-XX:+DoEscapeAnalysis
-XX:+PrintEscapeAnalysis
```

---

## Summary Table

| Aspect             | Without Escape Analysis | With Escape Analysis |
| ------------------ | ----------------------- | -------------------- |
| Object allocation  | Heap only               | Stack / eliminated   |
| Garbage Collection | More GC pressure        | Less GC              |
| Synchronization    | Always enforced         | Locks may be removed |
| Performance        | Slower                  | Faster               |

---

## Key Takeaway

**Escape analysis helps the JVM avoid unnecessary heap allocations and synchronization**, leading to:

* Faster execution
* Lower memory usage
* Reduced garbage collection overhead




**Flags**: `-XX:+DoEscapeAnalysis` (enabled by default), `-XX:+PrintEscapeAnalysis` (diagnostic)

### 59. Explain class loading mechanism. Parent delegation model.

**Class loading phases**: Loading → Linking (Verification, Preparation, Resolution) → Initialization

**Class loaders hierarchy** (parent delegation):
1. Bootstrap ClassLoader (null parent) → core JDK classes (`rt.jar`, `java.*`)
2. Platform ClassLoader → JDK modules
3. Application ClassLoader → application classpath

**Parent delegation model**:
- When a class loader gets a request, it first delegates to parent.
- Only if parent cannot find it → child tries to load.
- **Purpose**: Prevents duplicate classes, protects core classes from malicious overrides

**Breaks**: Custom loaders (OSGi, plugins), context class loaders (thread.setContextClassLoader())

## Java Class Loading Mechanism

The **class loading mechanism** in Java is the process by which the **JVM loads, links, and initializes classes** at runtime.
Classes are loaded **on demand**, not all at once when the program starts.

---

## Phases of Class Loading

![Image](https://media.geeksforgeeks.org/wp-content/uploads/jvmclassloader.jpg)

![Image](https://javatechonline.com/wp-content/uploads/2021/05/JVM_Architecture-1.jpg)

![Image](https://codepumpkin.com/wp-content/uploads/2017/07/jvm_architecture2.png)

### 1. **Loading**

* The JVM reads the `.class` bytecode and creates a `Class` object in memory.
* Bytecode may come from:

  * File system
  * JAR files
  * Network
* Done by a **ClassLoader**

---

### 2. **Linking**

Linking has **three sub-steps**:

#### a) Verification

* Ensures bytecode is valid and safe
* Checks:

  * Class format
  * Stack usage
  * Type safety
* Prevents malicious bytecode

#### b) Preparation

* Allocates memory for **static variables**
* Assigns **default values**

```java
static int x;   // x = 0
static Object o; // o = null
```

#### c) Resolution

* Converts symbolic references into direct memory references
* Example: resolving method and field references

---

### 3. **Initialization**

* Executes **static initializers** and **static blocks**
* Assigns actual values

```java
static int x = 10;

static {
    System.out.println("Class initialized");
}
```

⚠️ Happens **only once per class**

---

## Types of Class Loaders

![Image](https://docs.oracle.com/cd/E19501-01/819-3659/images/dgdeploy2.gif)

![Image](https://media.geeksforgeeks.org/wp-content/uploads/jvmclassloader.jpg)

Java uses a **hierarchical class loader system**:

1. **Bootstrap ClassLoader**

   * Loads core Java classes (`java.lang.*`)
   * Written in native code
   * Loads from `rt.jar` (Java 8) or modules (Java 9+)

2. **Extension ClassLoader**

   * Loads classes from `jre/lib/ext`
   * Optional libraries

3. **Application (System) ClassLoader**

   * Loads classes from classpath (`-classpath`)

---

## Parent Delegation Model

### What is Parent Delegation?

> Before loading a class, a class loader **delegates the request to its parent**.

Only if the parent **cannot find the class**, the child loader attempts to load it.

---

### How Parent Delegation Works

![Image](https://miro.medium.com/1%2AVdQrbVT2XxRAwa9p7twKdg.png)

![Image](https://codepumpkin.com/wp-content/uploads/2017/08/classLoaderDelegationAlgo.png)

**Flow:**

1. Application ClassLoader receives request
2. Delegates to Extension ClassLoader
3. Delegates to Bootstrap ClassLoader
4. If not found → control returns downward
5. Child loader loads the class

---

### Example

```java
Class.forName("java.lang.String");
```

* Application ClassLoader asks parent
* Bootstrap ClassLoader loads `String`
* Prevents redefinition of core classes

---

## Why Parent Delegation Is Important

### 1. **Security**

* Prevents overriding core Java classes
* Example: malicious `java.lang.String`

### 2. **Consistency**

* Core classes are loaded only once
* Avoids multiple versions of the same class

### 3. **Stability**

* Ensures trusted Java APIs are always used

---

## Breaking Parent Delegation (Advanced)

Some frameworks **intentionally break** parent delegation:

* Application servers
* OSGi
* Custom ClassLoaders

Example:

* Web servers loading different versions of libraries

⚠️ Requires careful handling to avoid `ClassCastException`

---

## Summary

| Aspect                | Description                         |
| --------------------- | ----------------------------------- |
| Class Loading         | Load → Link → Initialize            |
| Loading Time          | On demand                           |
| ClassLoader Hierarchy | Bootstrap → Extension → Application |
| Parent Delegation     | Parent loads first                  |
| Benefits              | Security, consistency, stability    |

---

### Key Takeaway

The **parent delegation model ensures that core Java classes are always loaded by trusted class loaders**, making Java secure and robust.


### 60. What are JVM flags for tuning GC (-XX:+UseG1GC, etc.)?

**Important GC tuning flags (Java 21–25)**:

| Flag                                | Purpose                                                  | Typical values                          |
|-------------------------------------|----------------------------------------------------------|-----------------------------------------|
| `-XX:+UseG1GC`                      | Use G1 collector                                         | Default since Java 9                    |
| `-XX:+UseZGC`                       | Use ZGC                                                  | For low-latency                         |
| `-XX:+ZGenerational`                | Enable generational ZGC                                  | Recommended                             |
| `-XX:MaxGCPauseMillis`              | Target pause time (G1)                                   | 200 (default)                           |
| `-XX:G1HeapRegionSize`              | Region size (G1)                                         | 1M–32M (auto)                           |
| `-Xms`, `-Xmx`                      | Initial & maximum heap size                              | Equal for production                    |
| `-XX:NewRatio`                      | Old : Young ratio                                        | 2 (default)                             |
| `-XX:MaxTenuringThreshold`          | Max age before promotion                                 | 15 (default)                            |
| `-Xlog:gc*`                         | GC logging                                               | Very useful for tuning                  |

**Rule of thumb**: Set -Xms = -Xmx, tune pause target, then analyze GC logs.

### 61. How to analyze heap dumps (jmap, Eclipse MAT)?

**Generate heap dump**:
- `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=...`
- `jcmd <pid> GC.heap_dump filename=heap.hprof`
- `jmap -dump:live,format=b,file=heap.hprof <pid>`

**Analysis tools**:
1. **Eclipse Memory Analyzer (MAT)** — most popular
   - Leak suspects report
   - Dominator tree
   - Histogram
   - OQL queries
2. **jhat** — simple browser-based (legacy)
3. **VisualVM** / **JMC** — integrated heap dump analysis
4. **heaphero.io** — online analyzer

**Typical steps**:
1. Find largest objects (histogram)
2. Find retainers (what keeps them alive)
3. Check class histograms
4. Look for accumulation patterns

### 62. Difference between minor, major, and full GC

| Type         | Scope                          | Frequency | Stop-the-world? | Typical duration |
|--------------|--------------------------------|-----------|------------------|------------------|
| **Minor GC** | Young generation only          | Very high | Yes              | Very short       |
| **Major GC** | Old generation (may include young) | Low    | Yes (depends on collector) | Longer           |
| **Full GC**  | Entire heap + metaspace        | Rare      | Usually yes      | Longest          |

**Modern collectors**:
- **G1**: Mixed collections (young + some old regions) — no pure major/full
- **ZGC/Shenandoah**: Mostly concurrent — very few stop-the-world phases

### 63. What is metaspace? Difference from PermGen

**Metaspace** (since Java 8):
- Stores class metadata, method bytecode, interned strings, annotations
- Located in **native memory** (not heap)
- Automatically resizes (grows/shrinks)
- Garbage collected during full GC (when metaspace is full)

**PermGen** (Java 7 and earlier):
- Part of heap
- Fixed size (`-XX:MaxPermSize`)
- Frequent OutOfMemoryError: PermGen
- No auto-sizing

**Key difference**: Metaspace → no more fixed-size PermGen issues, but can still cause native OOM if grows uncontrollably.

### 64. Explain JIT compilation phases (C1, C2 compilers)

**Tiered compilation** (default since Java 7):

1. **Interpreter** → first execution
2. **C1 (Client compiler)** — fast, less optimized (levels 1–3)
   - Quick compilation
   - Used for startup/hot code
3. **C2 (Server compiler)** — slow, highly optimized (level 4)
   - Aggressive inlining, loop optimizations, escape analysis
   - Used after method becomes very hot

**Thresholds**:
- Compilation triggered after ~1500–10000 invocations (varies)
- Background compilation (non-blocking)

**Flags**:
- `-XX:+TieredCompilation` (default)
- `-XX:CompileThreshold=...`
- `-XX:+PrintCompilation` (diagnostic)

### 65. How does biased locking work? Revocation issues

**Biased locking** — optimization for uncontended locks (enabled by default until Java 15, removed in Java 21+).

**How it works**:
- First thread that acquires a lock **biases** the object (stores thread ID in mark word)
- Subsequent acquisitions by same thread → no atomic CAS (very fast)
- If another thread tries to acquire → **revocation** occurs

**Revocation**:
- Bulk revocation (all instances of class) or single-object
- Stop-the-world safepoint (pause all threads)
- Inflates lock to thin/fat monitor

**Issues**:
- High revocation rate → performance degradation (especially startup)
- For this reason, biased locking was **disabled by default** in Java 15+ and **completely removed** in Java 21+

**Modern recommendation**: Rely on lightweight locking (fast-path CAS) and stack-locking (Java 21+ enhancements).


### Java 8+ Features & Streams Questions (66–80)

Here are clear, interview-ready explanations covering Java 8 through Java 21+ features (as of January 2026, with Java 21 LTS and Java 25 in active use).

#### 66. Explain lambda expressions and functional interfaces.
**Lambda expressions** (Java 8) are anonymous functions that implement **functional interfaces** (interfaces with exactly one abstract method — SAM).  
Syntax: `(parameters) -> expression` or `(params) -> { statements; }`  

**Functional interfaces** are marked with `@FunctionalInterface` (optional but recommended).  
Examples: `Runnable`, `Comparator`, `Function<T,R>`, `Predicate<T>`, `Consumer<T>`, `Supplier<T>`.  

**Purpose**: Enable functional programming style — pass behavior as data.  
Example:  
```java
Comparator<String> byLength = (s1, s2) -> s1.length() - s2.length();
list.sort(byLength);
```
Lambdas are converted to instances of the target functional interface at compile time.

#### 67. What are default and static methods in interfaces (Java 8)?
**Default methods** provide default implementation in interfaces (using `default` keyword).  
**Static methods** are utility methods attached to the interface.  

**Purpose**: Allow interface evolution without breaking existing implementations (backward compatibility).  

Examples:
```java
interface Vehicle {
    default void start() {                      // Default
        System.out.println("Engine starting...");
    }

    static int calculateFuel(int distance) {    // Static
        return distance / 10;
    }
}
```
**Conflicts**: If multiple defaults conflict, implementing class must override.  
Classic use case: `Collection.stream()` added via default method.

#### 68. How does Stream API work? Lazy vs eager operations.
**Stream API** processes collections/data in a functional, declarative way.  

**Pipeline structure**:
1. **Source**: `collection.stream()` or `Stream.of(...)`
2. **Intermediate operations** (lazy): Return new Stream, no computation yet → `filter`, `map`, `sorted`, `distinct`, `limit`, etc.
3. **Terminal operation** (eager): Triggers actual processing → `collect`, `forEach`, `reduce`, `count`, `anyMatch`, `findFirst`, etc.

**Lazy evaluation**: Intermediate ops are only executed when terminal op is called → efficient chaining, short-circuiting (e.g., `findFirst()` stops early).  
**Internal iteration**: Stream handles iteration (vs external for-loop).  
**Parallel**: `.parallel()` uses ForkJoinPool.

#### 69. Difference between map() and flatMap() in Streams.
Both transform elements → return Stream.

| Operation | Input → Output             | Flattens? | Use Case                              |
|-----------|----------------------------|-----------|---------------------------------------|
| **map**   | T → R                      | No        | 1-to-1 transformation                 |
| **flatMap** | T → Stream<R>            | Yes       | 1-to-many + flatten nested structures |

## Difference between `map()` and `flatMap()` in Java Streams

Both `map()` and `flatMap()` are **intermediate operations** in the Java Stream API, but they serve **different purposes**.

---

## 1. `map()` — One-to-One Transformation

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AHO2Pjql2s4Yg3BuKmGEp1Q.png)

![Image](https://i.sstatic.net/7e1tY.jpg)

### What it does

* Transforms **each element** of a stream into **exactly one element**
* Output stream size = input stream size

### Signature

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper)
```

### Example

```java
List<String> names = List.of("alice", "bob");

List<Integer> lengths =
    names.stream()
         .map(String::length)
         .toList();
```

**Result**

```
[5, 3]
```

✔ One input → one output

---

## 2. `flatMap()` — One-to-Many (Flattening)

![Image](https://custom-images.strikinglycdn.com/res/hrscywv4p/image/upload/c_limit%2Cfl_lossy%2Ch_9000%2Cw_1200%2Cf_auto%2Cq_auto/784839/535425_651851.png)

![Image](https://wkrzywiec.is-a.dev/posts/049_java-series-flatmap/map-fx_hu_88dac164dab23cce.png)

### What it does

* Transforms **each element** into a **Stream**
* Flattens all resulting streams into **one single stream**
* Output size can be **smaller, equal, or larger**

### Signature

```java
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper)
```

### Example (Nested Collections)

```java
List<List<String>> list = List.of(
    List.of("a", "b"),
    List.of("c", "d")
);

List<String> result =
    list.stream()
        .flatMap(List::stream)
        .toList();
```

**Result**

```
[a, b, c, d]
```

✔ Nested → flattened

---

## Key Difference (Conceptual)

| Operation   | Input | Output                         |
| ----------- | ----- | ------------------------------ |
| `map()`     | `T`   | `R`                            |
| `flatMap()` | `T`   | `Stream<R>` → flattened to `R` |

---

## Side-by-Side Example

```java
List<String> sentences = List.of("hello world", "java streams");

// map()
List<String[]> mapResult =
    sentences.stream()
             .map(s -> s.split(" "))
             .toList();

// flatMap()
List<String> flatMapResult =
    sentences.stream()
             .flatMap(s -> Arrays.stream(s.split(" ")))
             .toList();
```

### Results

```
map():      [["hello","world"], ["java","streams"]]
flatMap():  ["hello","world","java","streams"]
```

---

## When to Use Which?

### Use `map()` when:

* You want a **simple transformation**
* One input → one output
* No nesting involved

Examples:

* `String → Integer`
* `Object → DTO`

---

### Use `flatMap()` when:

* Your transformation returns a **collection or stream**
* You want to **flatten nested structures**
* One input → zero, one, or many outputs

Examples:

* `List<List<T>> → List<T>`
* Optional handling

```java
Optional<String> opt = Optional.of("java");

opt.stream()
   .flatMap(s -> Stream.of(s.split("")))
   .forEach(System.out::println);
```

---

## Common Interview One-Liner

> **`map()` transforms elements; `flatMap()` transforms and flattens.**

---

## Summary Table

| Feature        | `map()`          | `flatMap()`                   |
| -------------- | ---------------- | ----------------------------- |
| Mapping        | 1 → 1            | 1 → many                      |
| Returns        | Stream of values | Stream of streams (flattened) |
| Stream nesting | Preserved        | Removed                       |
| Typical use    | Simple mapping   | Flattening                    |

---

### Key Takeaway

If your mapping function returns a **single value**, use `map()`.
If it returns a **collection or stream**, use `flatMap()`.


#### 70. Explain Collectors: groupingBy, partitioningBy, toMap.
**Collectors** accumulate stream elements into collections/results.

- **groupingBy(Function classifier)** → Map<K, List<T>>  
  ```java
  Map<String, List<Person>> byCity = people.stream()
      .collect(Collectors.groupingBy(Person::getCity));
  ```

- **partitioningBy(Predicate)** → Map<Boolean, List<T>> (binary split)  
  ```java
  Map<Boolean, List<Person>> adults = people.stream()
      .collect(Collectors.partitioningBy(p -> p.getAge() >= 18));
  ```

- **toMap(keyMapper, valueMapper)** → Map<K,V>  
  ```java
  Map<Long, String> idToName = people.stream()
      .collect(Collectors.toMap(Person::getId, Person::getName,
                                (old, newVal) -> old)); // merge function
  ```

**Advanced**: `groupingBy(classifier, downstream)` (e.g., `counting()`, `summingInt()`).

#### 71. What are optional methods? Best practices to avoid NullPointerException.
**Optional<T>** (Java 8) represents a value that may be absent — wrapper around nullable reference.

**Key methods**:
- `of(T)` / `ofNullable(T)` / `empty()`
- `isPresent()` / `isEmpty()`
- `get()` (dangerous)
- `orElse(T)`, `orElseGet(Supplier)`, `orElseThrow()`
- `map(Function)`, `flatMap()`, `filter()`
- `ifPresent(Consumer)`

**Best practices**:
- Return `Optional` from methods that may return null
- **Never** use `get()` directly → prefer `orElse*` or `orElseThrow()`
- Chain transformations: `opt.map(...).orElse(default)`
- Avoid `Optional` fields/parameters → use for return values only
- **Do not** replace every null check — use when absence is meaningful

#### 72. How do parallel streams work? When to avoid them.
**parallelStream()** or `.parallel()` splits data via **Spliterator**, processes on **ForkJoinPool.commonPool()** (≈ CPU cores - 1 threads).

**How it works**:
- Source split into chunks
- Work-stealing: idle threads steal tasks
- Terminal operation combines results

**When to avoid**:
- Small datasets (overhead > benefit)
- Stateful operations / side effects
- Blocking I/O inside lambda (starves common pool)
- Ordered streams with heavy ops (`.sorted()`, `.distinct()`)
- Need predictable order

**Rule**: Use parallel for **pure, CPU-bound** operations on large data. Prefer sequential for I/O-bound.

#### 73. Explain the Spliterator interface.
**Spliterator** (Java 8) = **Splitable Iterator** — enables efficient parallel processing.

**Key methods**:
- `tryAdvance(Consumer)` — process one element
- `trySplit()` — split for parallelism (returns child Spliterator)
- `estimateSize()` — size hint
- `characteristics()` — flags (ORDERED, SIZED, IMMUTABLE, DISTINCT, CONCURRENT, NONNULL, SUBSIZED)

**Used by**: Streams for decomposition (arrays fast, HashMap harder).  
Custom Spliterators allow parallel processing of non-standard sources.

#### 74. Difference between intermediate and terminal operations.
| Type          | Return type | Execution     | Chaining? | Examples                          |
|---------------|-------------|---------------|-----------|-----------------------------------|
| **Intermediate** | Stream     | Lazy          | Yes       | map, filter, sorted, limit, skip |
| **Terminal**     | Non-Stream | Eager         | No        | collect, forEach, reduce, count, anyMatch |

**Intermediate** build pipeline (no work done).  
**Terminal** triggers computation. Only **one** terminal per pipeline.

#### 75. What are method references? Types (:: syntax).
**Method references** — shorthand for lambdas that call existing methods.  
Syntax: `Class::method` or `instance::method`

**Types**:
1. **Static** — `Class::staticMethod`
2. **Instance (particular object)** — `obj::instanceMethod`
3. **Instance (arbitrary object)** — `Class::instanceMethod`
4. **Constructor** — `Class::new`

**Examples**:
```java
list.forEach(System.out::println);                // instance
Comparator.comparing(Person::getAge);             // instance arbitrary
Supplier<List<String>> list = ArrayList::new;     // constructor
```

Cleaner than equivalent lambda when no extra logic.

#### 76. Explain record patterns in deconstruction (Java 19+).
**Record patterns** (final in Java 21) allow deconstruction of records in `instanceof` and `switch`.

**Example**:
```java
record Point(int x, int y) {}

if (obj instanceof Point(int x, int y)) {
    System.out.println(x + y);  // x, y bound
}
```

**Nested**:
```java
if (obj instanceof Pair(Point p1, Point p2)) { ... }
```

**Benefits**: Concise, type-safe destructuring. Only works with records (canonical constructor).

#### 77. How does pattern matching for switch (Java 21) handle null?
**Pattern matching for switch** (final in Java 21) supports type patterns, guards (`when`), and **explicit null handling**.

**Null behavior**:
- `case null -> ...` explicitly handles null
- Without `case null`: `NullPointerException` thrown (backward compatible)

**Example**:
```java
String result = switch (obj) {
    case null        -> "Input is null";
    case String s    -> "String: " + s.length();
    case Integer i   -> "Integer: " + i;
    default          -> "Other";
};
```

**Encourages** exhaustive handling — no more separate null checks outside switch.

#### 78. What are unnamed patterns and variables (Java 21+)?
**Unnamed patterns & variables** (final in Java 22 via JEP 456) use `_` (underscore) for intentionally unused variables/patterns.

**Examples**:
```java
// Unnamed variable
try { ... } catch (Exception _) { log.error("Error occurred"); }

// Unnamed pattern
if (obj instanceof Point(_, int y)) {
    System.out.println("Y = " + y);  // x ignored
}

switch (pair) {
    case Pair(String _, Integer i) -> System.out.println("Second: " + i);
}
```

**Benefits**: Cleaner code, no unused variable warnings, clearer intent.

#### 79. Explain the new HTTP Client in Java 11+.
**java.net.http.HttpClient** (standardized in Java 11) — modern, non-blocking, HTTP/1.1 + HTTP/2 client.

**Key features**:
- Synchronous: `send(request, handler)`
- Asynchronous: `sendAsync(request, handler)` → `CompletableFuture<HttpResponse>`
- Builder pattern: `HttpClient.newBuilder().version(HttpClient.Version.HTTP_2).build()`
- Supports WebSocket, timeouts, redirects, authentication
- Body handlers: `BodyHandlers.ofString()`, `ofFile()`, `ofByteArray()`

**Example**:
```java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com"))
    .GET()
    .build();

HttpResponse<String> response = client.send(request, BodyHandlers.ofString());
```

Replaces old `HttpURLConnection` — cleaner, async-native, HTTP/2 support.

#### 80. Virtual threads impact on reactive programming (e.g., WebFlux).
**Virtual threads** (final in Java 21) — lightweight, millions possible, unmount on blocking I/O → make **blocking code scale** like non-blocking.

**Impact on reactive (Spring WebFlux / Reactor)**:
- **Traditional reason for reactive**: Avoid thread-per-request model (platform threads expensive → few threads, non-blocking needed)
- **With virtual threads**: Thread-per-request becomes viable again — blocking JDBC, RestTemplate, etc. scale to thousands of concurrent requests with low overhead
- **Many benchmarks** (2024–2026) show virtual threads (Spring MVC) often **match or beat** WebFlux in throughput/latency for typical REST APIs, especially with blocking dependencies
- **WebFlux still better** when: true non-blocking drivers end-to-end, complex streaming/backpressure, WebSocket, or extreme throughput with minimal memory

**Trend** (2026): Many teams migrate **from WebFlux to Spring MVC + virtual threads** for simplicity/debuggability — reactive complexity often not justified anymore.

These answers are interview-focused: concise, with examples, and highlighting trade-offs. Good luck with your preparation!

#### Design Patterns, Best Practices & Miscellaneous (81–100)
Here are detailed, interview-ready answers for questions **81–100** (Design Patterns, Best Practices & Miscellaneous) — perfect for senior Java developer interviews in 2026.

### 81. Explain Singleton pattern variants (eager, lazy, enum)

**Singleton** ensures exactly one instance and provides global access point.

**Variants**:

1. **Eager Initialization** (simple & thread-safe)
   ```java
   public class EagerSingleton {
       private static final EagerSingleton INSTANCE = new EagerSingleton();
       private EagerSingleton() {} // private constructor
       public static EagerSingleton getInstance() { return INSTANCE; }
   }
   ```
   - Created at class loading → thread-safe, but initializes even if never used

2. **Lazy Initialization + Double-Checked Locking** (most common safe lazy variant)
   ```java
   public class LazySingleton {
       private static volatile LazySingleton instance;
       private LazySingleton() {}
       
       public static LazySingleton getInstance() {
           if (instance == null) {                   // first check (no lock)
               synchronized (LazySingleton.class) {
                   if (instance == null) {           // second check
                       instance = new LazySingleton();
                   }
               }
           }
           return instance;
       }
   }
   ```
   - `volatile` prevents partial object visibility

3. **Enum Singleton** (Joshua Bloch's recommended — cleanest & safest)
   ```java
   public enum EnumSingleton {
       INSTANCE;
       
       public void doSomething() {
           // business logic
       }
   }
   ```
   - Handles serialization, reflection attacks, threading automatically
   - Most concise and bulletproof

**Recommendation in 2026**: Use **enum** unless you have very specific reasons not to.

### 82. Factory vs Abstract Factory vs Builder pattern

| Pattern              | Purpose                                      | When to use                                      | Key Characteristic                     |
|----------------------|----------------------------------------------|--------------------------------------------------|----------------------------------------|
| **Factory Method**   | Delegate object creation to subclasses       | Need different implementations of same interface | One method that returns product        |
| **Abstract Factory** | Create families of related objects           | Need interchangeable product families            | Multiple factory methods               |
| **Builder**          | Construct complex objects step-by-step       | Many optional parameters, immutable objects      | Fluent interface, step-by-step build   |

**Examples**:
- Factory Method: `Document.createDocument("pdf")` → returns different concrete document
- Abstract Factory: `GUIFactory` → creates Button + Checkbox for Windows/Mac theme
- Builder: `Pizza.Builder().size(12).cheese(true).pepperoni().build()`

**Modern preference**: Builder is extremely common (Lombok `@Builder`, records + builder pattern).

### 83. When to use Observer pattern? Java implementation (PropertyChangeSupport)

**Observer** = one-to-many dependency: when subject state changes, all observers are notified automatically.

**When to use**:
- UI updates when model changes (classic MVC)
- Event systems (button clicks, property changes)
- Publish-subscribe scenarios where decoupling is important

**Java built-in implementation** — `java.beans.PropertyChangeSupport` (most common):

```java
public class Stock {
    private final PropertyChangeSupport support = new PropertyChangeSupport(this);
    private double price;

    public void setPrice(double newPrice) {
        double old = this.price;
        this.price = newPrice;
        support.firePropertyChange("price", old, newPrice);
    }

    public void addPriceListener(PropertyChangeListener listener) {
        support.addPropertyChangeListener("price", listener);
    }
}
```

Modern alternatives: Spring `ApplicationEventPublisher`, RxJava, Project Reactor.

### 84. Explain Decorator pattern. Relation to Java IO classes

**Decorator** adds responsibilities to objects **dynamically** by wrapping them — follows Open-Closed principle.

**Structure**:
- Component interface
- Concrete component
- Decorator implements component + holds wrapped component

**Java IO classic example**:
```java
InputStream is = new BufferedInputStream(
                  new DataInputStream(
                  new FileInputStream("file.txt")));
```
- Each decorator adds behavior (buffering, data conversion) without modifying base class
- Chainable → very flexible

Other uses: Logging decorators, security wrappers, compression streams.

### 85. What is Dependency Injection? Benefits in large apps

**Dependency Injection** (DI) = providing dependencies to a class from outside (instead of creating inside).

**Types**:
- Constructor Injection (most recommended)
- Setter Injection
- Field Injection (avoid in production)

**Benefits in large apps**:
- Loose coupling → easy to swap implementations
- Testability → easy mocking/stubbing
- Maintainability → configuration centralized
- Scalability → modular design
- Promotes SOLID principles (especially Dependency Inversion)

**Frameworks**: Spring (most popular), Guice, CDI, Micronaut, Quarkus.

### 86. Strategy pattern vs State pattern

Both are behavioral patterns that use composition over inheritance.

| Aspect              | Strategy Pattern                          | State Pattern                                |
|---------------------|-------------------------------------------|----------------------------------------------|
| Purpose             | Interchangeable algorithms/behaviors      | Object behavior changes with internal state  |
| Who chooses         | Client/context chooses strategy           | Object itself changes state internally       |
| State change        | External                                  | Internal (state transitions)                 |
| Typical use         | Sorting algorithms, payment methods       | TCP connection states, order lifecycle       |
| Analogy             | Plug different strategies                 | State machine with different behaviors       |

**Key**: Strategy → context delegates to strategy  
State → context delegates to current state object

### 87. Explain Proxy pattern. Dynamic proxies in Java

**Proxy** provides a surrogate/placeholder for another object to control access.

**Types**:
- Virtual Proxy (lazy loading)
- Protection Proxy (security)
- Remote Proxy (RMI)
- Smart Proxy (caching, logging)

**Dynamic Proxy in Java** (`java.lang.reflect.Proxy`):
```java
InvocationHandler handler = (proxy, method, args) -> {
    System.out.println("Before method");
    Object result = method.invoke(realObject, args);
    System.out.println("After method");
    return result;
};

Service service = (Service) Proxy.newProxyInstance(
    Service.class.getClassLoader(),
    new Class[]{Service.class},
    handler);
```

**Limitations**: Works only with interfaces  
**Class-based proxies**: CGLIB, ByteBuddy (used by Spring AOP)

### 88. How to implement circuit breaker (Resilience4j)

**Resilience4j CircuitBreaker** — most popular in Spring Boot 3.x

**Annotation style** (recommended):
```java
@Service
public class BackendService {
    
    @CircuitBreaker(name = "backendService", fallbackMethod = "fallback")
    public String callExternal() {
        return restTemplate.getForObject("http://external/api", String.class);
    }
    
    public String fallback(Throwable t) {
        return "Fallback response - service unavailable";
    }
}
```

**Configuration** (application.yml):
```yaml
resilience4j:
  circuitbreaker:
    instances:
      backendService:
        slidingWindowSize: 100
        failureRateThreshold: 50
        waitDurationInOpenState: 10000   # 10s
        permittedNumberOfCallsInHalfOpenState: 3
```

**States**: Closed → Open → Half-Open

### 89. Best practices for equals() and hashCode()

**Rules** (from Effective Java):
- If two objects are equal according to `equals()`, they **must** have the same `hashCode()`
- If `equals()` is false, `hashCode()` can be same or different
- `equals()` must be **reflexive**, **symmetric**, **transitive**, **consistent**, and handle `null` correctly

**Best practices**:
- Always override both together
- Use same fields in both
- Prefer `Objects.equals()` and `Objects.hash()`
- Make fields used in equals/hashCode **final** if possible
- For performance: cache hashCode (immutable objects)
- Use Lombok `@EqualsAndHashCode` or IDE generators
- **Never** use mutable fields in `equals()/hashCode()` if object used in HashMap/Set

### 90. Explain immutable objects design (Joshua Bloch guidelines)

**Immutable object** = state cannot change after construction.

**Bloch's rules**:
1. Don’t provide any mutators (setters)
2. Ensure class cannot be extended (make `final` or private constructor)
3. Make all fields `private final`
4. Ensure exclusive access to any mutable components
   - Defensive copy on input
   - Defensive copy on output
5. Don’t allow `this` to escape during construction

**Example**: `String`, `BigInteger`, `LocalDateTime`

**Benefits**: Thread-safety, no side effects, safe in collections, caching friendly.

### 91–100 (remaining questions)

Due to length constraints, here are concise versions of the remaining:

91. **Java Platform Module System (JPMS, Java 9+)**  
    Strong encapsulation, explicit dependencies (`module-info.java`). Benefits: smaller runtimes, better security, reliable configuration. Migration issues: automatic modules, split packages, reflection breaks.

92. **Module accessibility**  
    `exports` → public API  
    `opens` → reflection access at runtime  
    `uses` / `provides` → services  
    `requires` → dependencies

93. **String vs StringBuilder vs StringBuffer**  
    String: immutable  
    StringBuilder: mutable, not thread-safe (fastest)  
    StringBuffer: mutable, synchronized (thread-safe, slower)

94. **String intern()**  
    Adds string to pool → returns canonical reference. Useful for deduplication. Memory resident (Java 7+ in heap).

95. **compareTo() contract**  
    Consistent with equals, transitive, antisymmetric (sign reverse), total order.

96. **Debug performance**  
    `jstack` → thread dumps (deadlocks)  
    `jvisualvm` / JMC → CPU, memory, heap, profiling

97. **JMH microbenchmarking**  
    `@Benchmark`, `@Warmup`, `@Measurement`, `@Fork` — avoids dead-code elimination, warmup, JIT effects.

98. **var limitations (Java 10+)**  
    Local variables only, requires initializer, no null/poly expressions, no fields/params/returns.

99. **Text blocks (Java 15+)**  
    ```java
    String html = """
                  <html>
                      <body>Hello</body>
                  </html>
                  """;
    ```
    Preserves formatting, strips incidental whitespace.

100. **Java cloud-native evolution**  
     Modules → smaller images  
     GraalVM native images → AOT, fast startup, tiny footprint  
     Virtual threads → massive concurrency with simple code  
     CRaC → instant startup  
     Spring AOT, Quarkus, Micronaut → native compilation

These answers are designed to be both comprehensive and concise — perfect for explaining during interviews. Good luck with your preparation!