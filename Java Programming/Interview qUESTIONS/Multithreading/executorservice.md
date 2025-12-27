### Top Java Executor Framework Interview Questions and Answers

Here are **15 commonly asked interview questions** on the Executor framework, ranging from basic to advanced. Each includes a concise, interview-ready answer with key points to emphasize.

1. **What is the Executor framework and why do we use it?**  
   **Answer**: The Executor framework (java.util.concurrent) is a high-level API for managing thread execution. It decouples task submission from thread management. Instead of manually creating threads (`new Thread().start()`), we submit `Runnable` or `Callable` tasks to an Executor.  
   Benefits: Thread reuse (via pools), better resource utilization, easier lifecycle management, scalability, and avoidance of thread leaks.

2. **What is the difference between `Executor.execute(Runnable)` and `ExecutorService.submit(Runnable/Callable)`?**  
   **Answer**:  
   - `execute(Runnable)`: Fire-and-forget; no return value or way to track completion/exception.  
   - `submit(Runnable)`: Returns a `Future<?>` to check status or cancel.  
   - `submit(Callable)`: Returns `Future<T>` with the result of the computation.  
   `submit()` is preferred when you need results or exception handling.

3. **Explain the core interfaces in the Executor hierarchy.**  
   **Answer**:  
   - `Executor`: Basic – `void execute(Runnable)`.  
   - `ExecutorService` extends Executor: Adds shutdown, `submit()`, `invokeAll()`, `invokeAny()`.  
   - `ScheduledExecutorService` extends ExecutorService: Adds scheduling (`schedule`, `scheduleAtFixedRate`, etc.).

4. **What are the common ways to create thread pools using `Executors` factory class?**  
   **Answer**:  
   - `newFixedThreadPool(int n)` → Fixed threads, unbounded queue.  
   - `newCachedThreadPool()` → Dynamic threads (0 core, max Integer.MAX), 60s idle timeout.  
   - `newSingleThreadExecutor()` → One thread, sequential execution.  
   - `newScheduledThreadPool(int core)` → For periodic tasks.  
   - `newWorkStealingPool()` → ForkJoinPool with parallelism = CPU cores (ideal for recursive tasks).

5. **What happens if you submit a task to a shutdown ExecutorService?**  
   **Answer**: It throws `RejectedExecutionException`. Always check `isShutdown()` or handle gracefully.

6. **How do you properly shut down an ExecutorService?**  
   **Answer**:  
   ```java
   executorService.shutdown();           // Graceful: no new tasks, wait for existing
   // or
   List<Runnable> notExecuted = executorService.shutdownNow(); // Aggressive cancel
   executorService.awaitTermination(10, TimeUnit.SECONDS); // Optional wait
   ```
   Always shutdown in finally blocks or application hooks to prevent leaks.

7. **Explain the parameters of ThreadPoolExecutor constructor.**  
   **Answer**:  
   - `corePoolSize`: Minimum threads kept alive.  
   - `maximumPoolSize`: Max threads (creates extra when queue full).  
   - `keepAliveTime`: Idle time for extra threads before termination.  
   - `workQueue`: Queue for holding tasks (e.g., LinkedBlockingQueue, ArrayBlockingQueue, SynchronousQueue).  
   - `threadFactory`: Custom thread creation (naming, daemon).  
   - `handler`: Rejection policy when saturated (Abort, CallerRuns, Discard, etc.).

8. **What is the task execution flow in ThreadPoolExecutor?**  
   **Answer**:  
   1. If threads < corePoolSize → create new thread.  
   2. If all core threads busy → offer task to workQueue.  
   3. If queue full and threads < maximumPoolSize → create new thread.  
   4. If queue full and at maximumPoolSize → reject using handler.

9. **Which rejection policies do you know? When would you use CallerRunsPolicy?**  
   **Answer**:  
   - `AbortPolicy` (default): Throws RejectedExecutionException.  
   - `CallerRunsPolicy`: Caller thread executes the task → slows down submission, prevents overload.  
   - `DiscardPolicy`: Silently drops task.  
   - `DiscardOldestPolicy`: Drops oldest queued task.  
   Use `CallerRunsPolicy` for backpressure in bounded systems (e.g., rate limiting).

10. **What is the difference between `shutdown()` and `shutdownNow()`?**  
    **Answer**:  
    - `shutdown()`: No new tasks, waits for running/completing queued tasks.  
    - `shutdownNow()`: Attempts to stop running tasks (interrupt), returns list of queued tasks never started.

11. **What is a ForkJoinPool and when should you use it?**  
    **Answer**: Implements work-stealing algorithm. Ideal for divide-and-conquer recursive tasks (e.g., parallel merge sort, stream parallel processing). `newWorkStealingPool()` creates one with parallelism ≈ CPU cores. Java 8 parallel streams use the common ForkJoinPool by default.

12. **How do virtual threads (Java 21+) affect Executors?**  
    **Answer**: Use `Executors.newVirtualThreadPerTaskExecutor()` → creates a virtual thread per task. Extremely lightweight (millions possible), great for I/O-bound workloads. No need for tuning pool sizes. Blocking code scales massively without reactive programming.

13. **What is `CompletableFuture` and how does it relate to Executors?**  
    **Answer**: Advanced Future with functional-style composition (`thenApply`, `thenCombine`, `exceptionally`). Many methods accept an Executor (default: ForkJoinPool.commonPool()). Used heavily with async tasks (`supplyAsync(task, executor)`).

14. **How do you handle exceptions in tasks submitted to an Executor?**  
    **Answer**: Exceptions in `Runnable` are swallowed unless you check `Future.get()` (wraps in ExecutionException). Always use `Callable` or wrap `Runnable` with try-catch and propagate via Future. For uncaught exceptions, set an `UncaughtExceptionHandler` on threads or override `afterExecute()` in ThreadPoolExecutor.

15. **What is ExecutorCompletionService and when to use it?**  
    **Answer**: Wraps an ExecutorService. Allows retrieving completed futures in completion order (`take()`/`poll()`) instead of submission order. Useful when processing results as soon as any task finishes (e.g., searching multiple sources and returning first result).

### Bonus Quick Tips for Interviews
- Always mention proper shutdown to show production awareness.
- Know when to choose fixed vs cached vs virtual thread executors.
- Be ready to draw the ThreadPoolExecutor flow diagram.
- Mention that direct `new Thread()` is almost never recommended in production.

These questions cover ~90% of what interviewers ask about Executors. Practice explaining with small code snippets on a whiteboard! Good luck with your interviews!