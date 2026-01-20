# Kafka Notes – Topics & Partitions (High‑Yield)

These notes capture **only the important concepts** you need while watching a long Kafka course.

---

## 1. Kafka Topic

* A **Kafka topic** is a named entity in Kafka.
* Think of a topic like a **table in a database**.
* Topics live inside a **Kafka Broker**.
* **Producers** write messages to a topic using the topic name.
* **Consumers** read messages from a topic using the topic name.

### Producer–Consumer Model

* Producers **send messages** to a topic.
* Consumers **continuously poll (pull)** Kafka for new messages.
* Even after a consumer reads a message, **the message remains in Kafka** until its **retention period expires**.

👉 Key idea: **Kafka is not a queue** — messages are not deleted immediately after consumption.

---

## 2. Kafka Partitions

* A **topic is divided into one or more partitions**.
* Each partition is where messages are actually stored.
* In real systems, topics often have **many partitions** (10, 50, 100+).

### Why Partitions Matter

* Partitions enable **scalability and parallel consumption**.
* More partitions → higher throughput and consumer scalability.

---

## 3. Partition Properties

### Ordered, Immutable Log

* Each partition is:

  * **Ordered** (messages have a strict order)
  * **Immutable** (once written, messages cannot be changed)

### Offset

* Every record in a partition has an **offset**.
* Offset is:

  * A **unique, sequential number**
  * Assigned **when the message is produced**
* Offsets are **partition‑specific**.

Example:

* Partition 0: offsets → 0, 1, 2, 3, 4
* Partition 1: offsets → 0, 1, 2, 3, 4, 5

👉 Offsets **start from 0 in each partition** and grow independently.

---

## 4. Ordering Guarantee

* **Ordering is guaranteed only within a single partition**.
* Kafka **does NOT guarantee ordering across partitions**.

👉 If your use case requires strict ordering:

* Ensure all related messages go to the **same partition**.

---

## 5. Storage Model

* Messages are stored on disk as a **log file**.
* Kafka uses a **distributed commit log** model.
* Logs:

  * Are append‑only
  * Grow continuously as new records arrive

👉 Similar to database commit logs, but **distributed across brokers**.

---

## 6. How Messages Are Appended

* When a producer sends a message:

  * It is appended to a partition’s log
  * Offset is incremented by 1

Example:

* Partition 0 offset: 3 → 4
* Partition 1 offset: 5 → 6

This continues as new records are produced.

---

## 7. Producer & Partition Control

* The **producer controls which partition** a message goes to.
* Partition selection strategies will be covered later (e.g., key‑based, round‑robin, custom partitioner).

👉 Partition choice directly impacts:

* Ordering
* Performance
* Consumer scalability

---

## 8. Key Takeaways (Exam / Interview Ready)

* Topic = logical stream of messages
* Partition = physical storage unit
* Offsets are:

  * Per‑partition
  * Sequential
  * Immutable
* Ordering is **only per partition**
* Kafka keeps data even after consumption (retention‑based)
* Partitions enable **horizontal scalability**

---

