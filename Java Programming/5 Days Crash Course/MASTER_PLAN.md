Got it, Sam. This is a **5-day crash plan** designed for **interview mastery**, not learning from scratch. It assumes you already *use* Java + Spring Boot daily (which you do at Citi) and focuses on **answering under pressure, explaining clearly, and covering interviewer traps**.

This is **intense but realistic**. Treat it like an onsite sprint.

---

# DAILY STRUCTURE (same every day)

**Total ~7–8 hrs/day**

* **Core Tech (3.5 hrs)** – Java / Hibernate / Spring Boot
* **Behavioral (1 hr)** – STAR stories + delivery
* **LeetCode Revision (2 hrs)** – 2 NeetCode 250 problems
* **Rapid Recall (30 min)** – verbal explanation, no notes

---

## DAY 1 – CORE JAVA DEEP MASTERY (Most Asked)

### Java (3.5 hrs)

Focus on **why**, not syntax.

**Must-master topics**

* JVM internals:

  * Heap vs Stack vs Metaspace
  * Young/Old Gen, GC types (G1 vs CMS vs ZGC – high level)
* `equals()` / `hashCode()` (HashMap internals ⚠️)
* `Comparable` vs `Comparator`
* `volatile`, `synchronized`, locks, happens-before
* Thread safety in collections
* `final`, immutability, String pool
* Exception hierarchy + best practices
* Java 8+ features (streams pitfalls, parallel streams)

**Practice**

* Explain **HashMap put() flow**
* Explain **why main is static**
* Explain **how GC impacts latency systems**

---

### Behavioral (1 hr)

Prepare **these 3 stories fully**

1. Production issue / outage you fixed
2. Performance optimization (latency / TPS)
3. Conflict with teammate / pressure situation

Use **STAR**, but keep answers **2–3 minutes max**.

---

### LeetCode (2 hrs)

**Revision (NeetCode 250)**

1. Two Sum
2. Valid Parentheses

Explain:

* Approach
* SC / TC
* Edge cases out loud

---

## DAY 2 – ADVANCED JAVA + CONCURRENCY

### Java (3.5 hrs)

**Concurrency (VERY HIGH ROI)**

* Thread vs Runnable vs ExecutorService
* Fixed vs Cached thread pools
* Future vs CompletableFuture
* Deadlock, livelock, starvation
* `ConcurrentHashMap` internals
* Visibility vs atomicity
* `AtomicInteger` vs locks

**Design questions**

* Design a thread-safe rate limiter
* Producer–consumer in Java

---

### Behavioral (1 hr)

Prepare:

* “Tell me about yourself” (Java-heavy, role-aligned)
* “Why are you leaving your current job?”
* “Why this company?”

---

### LeetCode

1. Best Time to Buy and Sell Stock
2. Valid Anagram

---

## DAY 3 – HIBERNATE / JPA MASTERY (Interview Favorite)

### Hibernate / JPA (3.5 hrs)

**Core**

* Entity lifecycle
* `persist()` vs `merge()`
* Lazy vs Eager (N+1 problem ⚠️)
* First-level vs Second-level cache
* `@Transactional` behavior
* Dirty checking
* `@OneToMany` vs `@ManyToMany`
* `FetchType`, `CascadeType`
* Optimistic vs pessimistic locking

**Explain clearly**

* How Hibernate generates SQL
* How to fix N+1
* Transaction propagation

---

### Behavioral (1 hr)

Prepare:

* Failure story (what you learned)
* Handling ambiguity
* Working with tight deadlines

---

### LeetCode

1. Group Anagrams
2. Longest Substring Without Repeating Characters

---

## DAY 4 – SPRING BOOT INTERVIEW MASTERY

### Spring Boot (3.5 hrs)

**Core**

* Spring vs Spring Boot
* IOC & Dependency Injection
* Bean lifecycle
* `@Component` vs `@Service` vs `@Repository`
* `@Autowired` vs constructor injection
* Profiles & config loading order
* Actuator
* Exception handling (`@ControllerAdvice`)
* REST best practices

**Advanced**

* Filters vs Interceptors
* Async processing
* Spring Boot auto-configuration
* Transaction management in Spring

---

### Behavioral (1 hr)

Leadership principles:

* Mentoring juniors
* Owning a system
* Pushing back on bad designs

---

### LeetCode

1. Merge Intervals
2. Product of Array Except Self

---

## DAY 5 – SYSTEM THINKING + MOCK DAY

### Mixed Review (3 hrs)

* Explain your **current system architecture**
* Draw a **high-level design** verbally
* Explain:

  * How you handle scale
  * Failures
  * Latency
  * Caching
  * Messaging (Kafka / Solace)

---

### Behavioral (1.5 hrs)

Full mock:

* Tell me about yourself
* Biggest challenge
* Conflict
* Failure
* Why should we hire you?

Record yourself if possible.

---

### LeetCode (Final)

1. Binary Tree Level Order Traversal
2. Lowest Common Ancestor of a Binary Tree

---

### Final 30 min

* Rapid-fire explanations:

  * HashMap
  * GC
  * Transactions
  * Spring DI
  * Lazy loading
  * Thread pools

---

## KEY INTERVIEW RULES (IMPORTANT)

* **Speak slower than normal**
* **Explain before coding**
* **Admit uncertainty confidently**
* **Tie answers to real production experience**

---

If you want, next I can:

* Convert this into a **hour-by-hour calendar**
* Run **mock Java interviews**
* Create **one-page cheat sheets per topic**
* Do **Oracle-style deep Java grilling**

Just tell me.
