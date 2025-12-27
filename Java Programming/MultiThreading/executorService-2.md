Below are **interview-day, fact-checked, corrected notes** on **Java ExecutorService thread pool types**.
They are **short, precise, and memory-friendly**, written exactly for **last-day revision**.

---

## Java ExecutorService — Thread Pool Types (Interview Notes)

Java provides **four commonly used executor types** via the `Executors` factory class.

---

## 1. Fixed Thread Pool (`newFixedThreadPool`)

**What it is**

* A thread pool with a **fixed number of threads**
* Threads are created upfront and **reused**

**How it works**

* Tasks are submitted to a **thread-safe blocking queue**
* Worker threads:

  * Take tasks from the queue
  * Execute them
  * Pick the next task

**Key characteristics**

* Pool size never grows or shrinks
* If all threads are busy, tasks wait in the queue
* Uses an **unbounded LinkedBlockingQueue** internally

**Best used for**

* CPU-bound tasks
* Predictable workload
* Stable throughput requirements

---

## 2. Cached Thread Pool (`newCachedThreadPool`)

**What it is**

* A **dynamically sized** thread pool
* No fixed upper limit on thread count

**How it works**

* Uses a **SynchronousQueue** (no real task queue)
* When a task is submitted:

  * If a free thread exists → reuse it
  * Otherwise → create a new thread
* Threads idle for **60 seconds** are terminated

**Key characteristics**

* Can grow to a very large number of threads
* Threads are reclaimed automatically
* Fast task handoff, minimal queuing

**Best used for**

* Short-lived, bursty, I/O-heavy tasks
* When task arrival rate is unpredictable

**Caution**

* Can exhaust system resources if tasks arrive too fast

---

## 3. Scheduled Thread Pool (`newScheduledThreadPool`)

**What it is**

* Used for **delayed and periodic task execution**
* Replacement for `Timer` (more robust)

**Internal queue**

* Uses a **DelayQueue**
* Tasks are ordered by **next execution time**, not submission order

---

### Scheduling Methods

#### `schedule()`

* Runs a task **once after a delay**

#### `scheduleAtFixedRate()`

* Runs a task **periodically**
* Next execution time is based on **start time**
* If task execution takes longer than the period:

  * Next run starts immediately after completion

**Use when**

* You want a **fixed execution rate**

---

#### `scheduleWithFixedDelay()`

* Runs a task **after previous execution completes**
* Waits for the specified delay **after completion**

**Use when**

* You want a **fixed pause between executions**

---

**Best used for**

* Periodic health checks
* Metrics reporting
* Cleanup jobs
* Scheduled background work

---

## 4. Single Thread Executor (`newSingleThreadExecutor`)

**What it is**

* A fixed thread pool with **exactly one thread**

**Key guarantees**

* Tasks execute **sequentially**
* **Order of execution is preserved**
* If the thread dies due to exception:

  * Executor **creates a new one automatically**

**Best used for**

* When task order matters
* Event processing
* Serializing access to a shared resource

---

## Quick Comparison Table

| Executor Type | Threads | Queue Type       | Execution Order | Use Case         |
| ------------- | ------- | ---------------- | --------------- | ---------------- |
| Fixed         | Fixed N | BlockingQueue    | No guarantee    | CPU-bound        |
| Cached        | Dynamic | SynchronousQueue | No guarantee    | I/O bursts       |
| Scheduled     | Fixed N | DelayQueue       | Time-based      | Periodic jobs    |
| Single        | 1       | BlockingQueue    | Guaranteed      | Sequential tasks |

---

## Interview-Ready Summary

> Fixed thread pools limit concurrency and are ideal for CPU-bound work.
> Cached thread pools scale dynamically and suit short, I/O-heavy bursts.
> Scheduled executors handle delayed and recurring tasks using a delay-based queue.
> Single-thread executors guarantee task ordering while still providing fault tolerance.

---
