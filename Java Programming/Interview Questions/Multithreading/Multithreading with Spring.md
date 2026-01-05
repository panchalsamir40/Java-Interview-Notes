### Multithreading and Concurrency in Spring Boot Interview Questions (2025)

Here are commonly asked interview questions on multithreading, concurrency, and asynchronous processing in Spring Boot. These range from basic to advanced, often targeting mid-to-senior Java/Spring developers (3–10+ years). Questions focus on @Async, ThreadPool configuration, TaskExecutor, exception handling, transaction propagation, and integration with reactive programming.

#### Basic Questions

1. **How do you enable asynchronous execution in Spring Boot?**  
   Add `@EnableAsync` on a `@Configuration` class (or on the main `@SpringBootApplication` class). Then annotate methods with `@Async`. The method should return `void`, `Future`, `CompletableFuture`, or `ListenableFuture`.

2. **What is the default TaskExecutor used by @Async in Spring Boot?**  
   If no custom executor is configured, Spring uses `SimpleAsyncTaskExecutor`. It creates a new thread for every task (no thread pooling/reuse), which is not recommended for production due to resource exhaustion risk.

3. **How do you configure a custom ThreadPoolTaskExecutor in Spring Boot?**  
   Create a `@Bean` of type `TaskExecutor` (usually `ThreadPoolTaskExecutor`):  
   ```java
   @Bean
   public TaskExecutor taskExecutor() {
       ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
       executor.setCorePoolSize(10);
       executor.setMaxPoolSize(50);
       executor.setQueueCapacity(100);
       executor.setThreadNamePrefix("async-");
       executor.initialize();
       return executor;
   }
   ```
   Optionally qualify with `@Async("taskExecutor")`.

4. **What happens if @Async method is called from within the same class?**  
   The `@Async` annotation is ignored because Spring creates proxies (AOP) at the bean level. Self-invocation bypasses the proxy → synchronous execution.

#### Intermediate Questions

5. **How do you return a result from an @Async method?**  
   Return `CompletableFuture<T>` (preferred in modern Java) or `ListenableFuture<T>`. Caller can use `.get()`, `.join()`, or chain with `.thenApply()` etc.

6. **How do you handle exceptions in @Async methods?**  
   By default, exceptions are swallowed. Implement `AsyncUncaughtExceptionHandler`:  
   ```java
   @Component
   public class CustomAsyncExceptionHandler implements AsyncUncaughtExceptionHandler {
       @Override
       public void handleUncaughtException(Throwable ex, Method method, Object... params) {
           // log or notify
       }
   }
   ```
   And register it via `@EnableAsync(asyncUncaughtExceptionHandler = ...)`. For return types like CompletableFuture, use `.exceptionally()` or `.handle()`.

7. **What is the difference between @Async and @Scheduled?**  
   - `@Async`: Executes a method asynchronously when called.  
   - `@Scheduled`: Executes a method periodically using a scheduler (requires `@EnableScheduling`).

8. **Can @Async methods participate in the caller's transaction?**  
   No. A new thread is used, so transaction context is not propagated by default. Use `@Transactional` on the async method if it needs its own transaction, or configure a transaction-aware TaskExecutor.

9. **How do you make @Async methods transactional?**  
   Annotate the async method with `@Transactional`. Spring creates a new transaction on the new thread (propagation REQUIRES_NEW effectively).

#### Advanced Questions

10. **How does Spring propagate context (Security, Transaction, MDC logging) to async threads?**  
    Use a custom `TaskDecorator` in ThreadPoolTaskExecutor:  
    ```java
    executor.setTaskDecorator(context -> () -> {
        // copy SecurityContext, MDC, etc.
        SecurityContextHolder.setContext(SecurityContextHolder.getContext());
        MDC.setContextMap(originalMdc);
        try {
            context.run();
        } finally {
            MDC.clear();
        }
    });
    ```

11. **What are the risks of using SimpleAsyncTaskExecutor in production?**  
    Unlimited thread creation → potential OutOfMemoryError or thread exhaustion under load. Always configure a bounded ThreadPoolTaskExecutor.

12. **Explain the difference between ThreadPoolTaskExecutor and ThreadPoolExecutor.**  
    `ThreadPoolTaskExecutor` is Spring's wrapper around Java's `ThreadPoolExecutor`, adding lifecycle support, integration with Spring's TaskExecutor interface, and async features.

13. **How do you limit concurrency or rate-limit @Async calls?**  
    Use a bounded queue in ThreadPoolTaskExecutor (with custom RejectedExecutionHandler, e.g., CallerRunsPolicy). Or integrate with Resilience4j `@Bulkhead` or `@RateLimiter` annotations.

14. **When would you prefer WebFlux/Reactive (Project Reactor) over @Async for concurrency?**  
    - `@Async`: Thread-per-request, blocking I/O, suitable for CPU-bound or short tasks.  
    - Reactive (Mono/Flux): Non-blocking, event-loop model, better for I/O-bound high-concurrency (thousands of connections) with fewer threads.

15. **How do you test @Async methods in Spring Boot?**  
    Use `@SpringBootTest` with `Awaitility` or `CountDownLatch` to wait for async completion. Or mock TaskExecutor and verify invocations.

#### Scenario-Based/Tricky Questions

16. **What happens if an @Async method calls another @Async method?**  
    Both execute asynchronously (each on separate threads from the pool), provided they are invoked through proxies.

17. **How do you shutdown gracefully and wait for all @Async tasks to complete?**  
    Configure executor with `setWaitForTasksToCompleteOnShutdown(true)` and `setAwaitTerminationSeconds(...)`.

18. **Can you use @Async with CompletableFuture.supplyAsync() internally?**  
    Yes, but it's redundant. Prefer letting Spring manage the executor. You can inject the TaskExecutor and use `CompletableFuture.supplyAsync(..., taskExecutor)`.

19. **What is AsyncRestTemplate vs WebClient vs RestTemplate in async scenarios?**  
    - RestTemplate: Blocking.  
    - AsyncRestTemplate: Deprecated, callback-based async.  
    - WebClient: Modern, reactive non-blocking (preferred for high concurrency).

20. **How does Spring Boot integrate with Java's virtual threads (Project Loom, Java 21+)?**  
    Configure a virtual thread-based TaskExecutor:  
    ```java
    @Bean
    public TaskExecutor taskExecutor() {
        return Executors.newVirtualThreadPerTaskExecutor();
    }
    ```
    Great for I/O-bound async tasks with massive concurrency and low overhead.
