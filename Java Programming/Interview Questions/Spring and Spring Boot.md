### Top Spring Framework and Spring Boot Interview Questions and Answers (2025)

Here are some of the most commonly asked Spring (Core/Framework) and Spring Boot interview questions, categorized for clarity. These are based on frequently discussed topics in interviews for Java developers. I've focused on essential concepts like IoC, Dependency Injection, AOP, and key features of Spring Boot.

#### Basic Spring Framework Questions

1. **What is the Spring Framework?**  
   Spring is an open-source, lightweight Java framework for building enterprise-level applications. It simplifies development by providing features like Inversion of Control (IoC), Dependency Injection (DI), Aspect-Oriented Programming (AOP), transaction management, and integration with other technologies. Its core goal is to reduce boilerplate code and promote loose coupling.

2. **What is Inversion of Control (IoC) and Dependency Injection (DI)?**  
   IoC is a design principle where the control of object creation and lifecycle is inverted from the application code to a container. DI is a form of IoC where dependencies are injected into an object rather than the object creating them itself. Types include:  
   - Constructor Injection  
   - Setter Injection  
   - Field Injection  

3. **What is the difference between BeanFactory and ApplicationContext?**  
   BeanFactory is the basic IoC container that provides lazy loading and basic DI. ApplicationContext extends BeanFactory, adding advanced features like event propagation, internationalization (i18n), AOP integration, and eager bean loading by default. ApplicationContext is more commonly used.

4. **What are Spring Bean scopes?**  
   Common scopes:  
   - **Singleton**: One instance per IoC container (default).  
   - **Prototype**: New instance each time requested.  
   - **Request**: One instance per HTTP request (web-aware).  
   - **Session**: One instance per HTTP session.  
   - **GlobalSession**: For Portlet applications.

5. **What is Aspect-Oriented Programming (AOP) in Spring?**  
   AOP modularizes cross-cutting concerns (e.g., logging, security, transactions) separate from business logic. Key terms:  
   - Aspect: Module for cross-cutting concern.  
   - Advice: Action taken (e.g., @Before, @After, @Around).  
   - Pointcut: Expression to match join points.  
   - Join Point: Point during execution (e.g., method call).

#### Spring Boot Specific Questions

6. **What is Spring Boot?**  
   Spring Boot is a module built on top of Spring Framework to simplify setup and development of production-ready applications. It provides auto-configuration, embedded servers, and starters to reduce configuration.

7. **What are the key features of Spring Boot?**  
   - Auto-configuration: Automatically configures beans based on classpath.  
   - Starters: Dependency descriptors (e.g., spring-boot-starter-web).  
   - Embedded servers: Tomcat, Jetty, or Undertow by default.  
   - Actuator: Production-ready features like health checks, metrics.  
   - No XML configuration needed; opinionated defaults.

8. **What is the @SpringBootApplication annotation?**  
   It's a combination of three annotations:  
   - @EnableAutoConfiguration  
   - @ComponentScan  
   - @Configuration  
   It bootstraps the application and enables scanning for components.

9. **How does auto-configuration work in Spring Boot?**  
   Spring Boot scans the classpath for dependencies and uses @Conditional annotations (e.g., @ConditionalOnClass, @ConditionalOnMissingBean) to configure beans automatically. You can exclude specific auto-configurations if needed.

10. **What are Spring Boot Starters?**  
    Pre-configured dependency bundles (e.g., spring-boot-starter-data-jpa includes JPA, Hibernate, and Spring Data). They manage transitive dependencies to avoid version conflicts.

#### Intermediate/Advanced Questions

11. **How do you handle external configuration in Spring Boot?**  
    Use application.properties or application.yml. Supports profiles (e.g., application-dev.yml), environment variables, command-line arguments, and @ConfigurationProperties for type-safe binding.

12. **What is Spring Actuator?**  
    Provides endpoints for monitoring and managing applications (e.g., /health, /metrics, /env). Integrates with tools like Prometheus or Micrometer for observability.

13. **Explain @Async in Spring.**  
    Enables asynchronous method execution. Annotate methods with @Async and enable it with @EnableAsync. Returns Future or CompletableFuture for results.

14. **How does Spring support transaction management?**  
    Declarative with @Transactional annotation or programmatic with TransactionTemplate. Supports propagation levels (e.g., REQUIRED, REQUIRES_NEW) and isolation levels.

15. **What are common design patterns in Spring?**  
    - Singleton (default bean scope)  
    - Proxy (for AOP)  
    - Factory (BeanFactory)  
    - Template Method (e.g., JdbcTemplate)  
    - Front Controller (DispatcherServlet in Spring MVC)

### Advanced Spring Framework and Spring Boot Interview Questions for Experienced Developers (2025)

Here are more advanced and senior-level interview questions on Spring Framework and Spring Boot, focusing on real-world scenarios, microservices, performance, security, and deeper internals. These are commonly asked for roles requiring 5+ years of experience, including microservices architecture with Spring Cloud.

#### Advanced Spring Core and AOP Questions

16. **How does Spring AOP work internally, and what are the differences between JDK Dynamic Proxy and CGLIB proxy?**  
   Spring AOP uses proxies to implement aspects. JDK Dynamic Proxy is used when the target class implements interfaces (interface-based proxy). CGLIB is used for class-based proxying (subclassing the target) when no interfaces are implemented or for final classes/methods. CGLIB is faster but requires the class not to be final. Enable with `@EnableAspectJAutoProxy(proxyTargetClass=true)` for CGLIB.

17. **Explain Spring's event handling mechanism and how to publish custom events.**  
   Spring provides ApplicationEvent and ApplicationListener for decoupling components. Extend ApplicationEvent for custom events, publish via ApplicationEventPublisher (autowired or from ApplicationContext). Listeners implement ApplicationListener or use @EventListener annotation.

18. **What are the different types of Advice in Spring AOP, and when would you use @Around advice?**  
   Types: @Before, @After, @AfterReturning, @AfterThrowing, @Around. @Around is most powerful as it controls execution flow (proceed() to invoke target method), useful for transaction management, caching, or performance monitoring.

#### Spring Boot Advanced Questions

19. **How can you customize or override Spring Boot auto-configuration?**  
   Use `@ConditionalOnMissingBean` in custom config classes, exclude specific auto-configs with `@SpringBootApplication(exclude={...})`, or provide your own beans. Properties in application.yml can also influence conditionals like `@ConditionalOnProperty`.

20. **Explain the role of Spring Boot Actuator in production and advanced endpoints.**  
   Actuator provides monitoring endpoints (/health, /metrics, /env, /threaddump, custom). Integrate with Micrometer for Prometheus/Grafana. Secure with Spring Security; use management.endpoint.* properties for exposure.

21. **What is @ConfigurationProperties, and how does it support nested and validated configurations?**  
   Binds external properties to POJOs. Supports nested objects (inner classes), lists/maps. Add `@Validated` with JSR-380 annotations (e.g., @NotNull, @Size) for validation on startup.

22. **How does caching work in Spring Boot, and how to integrate with Redis?**  
   Enable with `@EnableCaching`. Use annotations: @Cacheable (read), @CachePut (update), @CacheEvict (remove). For Redis, add spring-boot-starter-data-redis and configure RedisCacheManager.

#### Microservices with Spring Boot and Spring Cloud

23. **What is service discovery, and how does Spring Cloud Netflix Eureka or Consul work?**  
   Services register with a registry (Eureka server). Clients discover instances dynamically. Use `@EnableEurekaClient` or `@EnableDiscoveryClient`. Handles dynamic scaling and failover.

24. **Explain client-side load balancing in Spring Cloud.**  
   Use Spring Cloud LoadBalancer (replacement for Ribbon). Annotate RestTemplate or WebClient with @LoadBalanced. Supports round-robin, weighted, zone-aware strategies.

25. **How do you implement circuit breaker pattern in Spring Boot microservices?**  
   Use Resilience4j (recommended) with @CircuitBreaker, @Retry, @TimeLimiter annotations. Fallback methods handle failures. Configure thresholds, ring-bit buffers.

26. **What is distributed tracing, and how to set it up with Spring Cloud Sleuth and Zipkin?**  
   Sleuth adds trace/span IDs to logs and MDC. Integrate with Zipkin (or OpenTelemetry) for visualization. Add spring-cloud-starter-sleuth and spring-cloud-starter-zipkin; traces propagate via headers in HTTP calls.

27. **How does Spring Cloud Gateway work as an API Gateway?**  
   Reactive, non-blocking gateway for routing, filtering (auth, rate-limiting), load balancing. Define routes in yml or Java DSL. Supports WebFlux.

28. **Explain inter-service communication patterns in microservices (synchronous vs asynchronous).**  
   Synchronous: Feign/OpenFeign or RestTemplate/WebClient (blocking/reactive). Asynchronous: Kafka/RabbitMQ with Spring Cloud Stream or @Async/@KafkaListener for event-driven.

#### Security, Testing, and Performance

29. **How to secure Spring Boot applications with Spring Security and OAuth2/JWT?**  
   Add spring-boot-starter-security. Configure WebSecurityConfigurerAdapter or SecurityFilterChain. For OAuth2: @EnableAuthorizationServer (resource server), use JWT with jjwt library or spring-security-oauth2-autoconfigure.

30. **What are best practices for testing Spring Boot applications (unit, integration, contract)?**  
   Unit: Mockito for services. Integration: @SpringBootTest with @MockBean. Slice tests: @WebMvcTest, @DataJpaTest. Contract: Pact or Spring Cloud Contract for consumer/producer stubs.
