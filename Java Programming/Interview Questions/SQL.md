
---

# 1️⃣ SQL BASICS (Still Asked — But Tricky)

---

## What is **SQL**?

### **Concept**

SQL (Structured Query Language) is:

> **A declarative language used to define, manipulate, and query relational data.**

Key idea:

* You describe **what** data you want
* The database decides **how** to get it (query planner)

💡 **Interview line**

> SQL is declarative — performance depends on the optimizer, not query order.

---

## Difference between **DDL, DML, DCL, TCL** ⭐

### **DDL (Data Definition Language)**

* Defines structure
* Affects schema
* Auto-committed

Examples:

* CREATE, ALTER, DROP, TRUNCATE

---

### **DML (Data Manipulation Language)**

* Works on data
* Transactional

Examples:

* INSERT, UPDATE, DELETE, SELECT

---

### **DCL (Data Control Language)**

* Controls access

Examples:

* GRANT, REVOKE

---

### **TCL (Transaction Control Language)**

* Controls transactions

Examples:

* COMMIT, ROLLBACK, SAVEPOINT

💡 **Senior clarity**

> DDL changes structure, DML changes data, DCL changes permissions, TCL changes transaction state.

---

## Difference between **DELETE, TRUNCATE, DROP** ⭐

| Aspect       | DELETE | TRUNCATE    | DROP    |
| ------------ | ------ | ----------- | ------- |
| Type         | DML    | DDL         | DDL     |
| Removes      | Rows   | All rows    | Table   |
| WHERE clause | ✅      | ❌           | ❌       |
| Rollback     | ✅      | ❌ (usually) | ❌       |
| Speed        | Slower | Faster      | Fastest |

💡 **Interview one-liner**

> DELETE is row-level, TRUNCATE is page-level, DROP is metadata-level.

---

## What is a **primary key**?

### **Concept**

A primary key:

> **Uniquely identifies each row in a table.**

Properties:

* Unique
* Not null
* Indexed automatically
* Enforces entity integrity

---

## Can a **primary key be NULL**?

### ❌ **No**

Because:

* NULL means “unknown”
* A row must always be identifiable

💡 **Interview clarity**

> A primary key guarantees identity — NULL breaks identity.

---

## Difference between **primary key and unique key**

| Aspect          | Primary Key | Unique Key      |
| --------------- | ----------- | --------------- |
| NULL allowed    | ❌           | ✅ (usually one) |
| Count per table | One         | Multiple        |
| Purpose         | Identity    | Uniqueness      |

💡 **Senior rule**

> Primary key = identity, unique key = constraint.

---

## Can a table have **multiple unique keys**?

### ✅ **Yes**

* Multiple columns or column combinations
* Enforce different business rules

---

## What is a **foreign key**?

### **Concept**

A foreign key:

> **Enforces referential integrity between two tables.**

It ensures:

* Child rows reference valid parent rows
* No orphan records

---

## Can a foreign key reference a **non-primary key**?

### ✅ **Yes**, if:

* The referenced column has a **UNIQUE constraint**

💡 **Interview trap**

> Foreign keys reference **candidate keys**, not just primary keys.

---

## What happens if you **delete a parent row**?

### Depends on FK rule:

* **RESTRICT / NO ACTION** → delete blocked
* **CASCADE** → child rows deleted
* **SET NULL** → FK set to NULL
* **SET DEFAULT** → default value used

💡 **Senior framing**

> Referential actions encode business rules at the database level.

---

# 2️⃣ JOINS (🔥 VERY IMPORTANT)

---

## Types of **joins in SQL**

* INNER JOIN
* LEFT JOIN
* RIGHT JOIN
* FULL OUTER JOIN
* CROSS JOIN
* SELF JOIN

---

## Difference between **INNER JOIN and LEFT JOIN** ⭐

### **INNER JOIN**

> Returns only matching rows from both tables.

Use when:

* Relationship is mandatory
* You only care about matches

---

### **LEFT JOIN**

> Returns all rows from left table + matching rows from right.

Use when:

* You want all left records
* Right side is optional

💡 **Interview one-liner**

> INNER JOIN filters, LEFT JOIN preserves.

---

## LEFT JOIN vs RIGHT JOIN — when to use?

### **Concept**

They are logically equivalent if tables are swapped.

### **Senior best practice**

> Prefer LEFT JOIN for readability and consistency.

RIGHT JOIN exists, but:

* Less readable
* Rarely used in production

---

## FULL OUTER JOIN — when is it useful?

### **Concept**

Returns:

* Matches
* Left-only rows
* Right-only rows

### **Real use cases**

* Data reconciliation
* Comparing two datasets
* Auditing mismatches

💡 **Interview insight**

> FULL OUTER JOIN is for *finding differences*, not relationships.

---

## CROSS JOIN — real use case

### **Concept**

CROSS JOIN = Cartesian product

### **Real uses**

* Generate combinations
* Calendar tables
* Test data
* Matrix generation

⚠️ **Danger**

> Rows = N × M → can explode quickly.

---

## SELF JOIN — example use case

### **Concept**

A table joined to itself.

### **Real uses**

* Hierarchies (employee → manager)
* Graph relationships
* Comparing rows in same table

💡 **Interview clarity**

> Self-join models relationships within the same entity.

---

## What happens when **join condition is missing**?

### **Result**

> CROSS JOIN (Cartesian product)

Impact:

* Massive result set
* Performance disaster

💡 **Interview trap**

> Missing join condition is one of the most expensive SQL bugs.

---

## JOIN vs SUBQUERY — which is faster and why?

### **Senior answer**

> **Neither is inherently faster — execution plans matter.**

### **General guidance**

* Correlated subqueries → often slower
* Joins → optimizer can reorder & optimize better

Modern DBs:

* Often rewrite subqueries into joins internally

💡 **Interview one-liner**

> Performance depends on the execution plan, not syntax.

---

## How do **joins work internally**?

### **High-level**

Database chooses join strategy:

* Nested Loop Join
* Hash Join
* Merge Join

Choice depends on:

* Indexes
* Table sizes
* Cardinality
* Statistics

💡 **Senior insight**

> SQL performance is driven by **join strategy**, not join syntax.

---

## What is **join cardinality**?

### **Concept**

Join cardinality is:

> **The number of rows produced by a join.**

Why it matters:

* Memory usage
* Execution plan choice
* Performance

Bad cardinality estimates →

* Wrong join strategy
* Slow queries

💡 **Interview gold**

> Cardinality estimation is the heart of query optimization.

---

## ✅ Final Interview-Ready Summary

* SQL is declarative — optimizer decides execution
* DDL defines structure, DML manipulates data
* DELETE vs TRUNCATE vs DROP differ in scope & logging
* Primary key = identity, unique key = constraint
* Foreign keys enforce referential integrity
* INNER JOIN filters; LEFT JOIN preserves
* FULL OUTER JOIN finds mismatches
* CROSS JOIN is intentional Cartesian product
* Missing join condition = disaster
* JOIN vs subquery performance depends on plan
* Join strategies depend on cardinality & indexes

---


---

# 3️⃣ WHERE vs HAVING vs GROUP BY

---

## Difference between **WHERE and HAVING** ⭐

### **WHERE**

**Concept**

> Filters **rows** *before* aggregation.

* Applied to raw rows
* Cannot see aggregated results
* Reduces data early (performance-friendly)

---

### **HAVING**

**Concept**

> Filters **groups** *after* aggregation.

* Applied to grouped results
* Can use aggregate functions
* Used for conditions on aggregates

---

### **Key difference**

| Aspect             | WHERE           | HAVING         |
| ------------------ | --------------- | -------------- |
| Filters            | Rows            | Groups         |
| Timing             | Before GROUP BY | After GROUP BY |
| Aggregates allowed | ❌               | ✅              |

💡 **Interview one-liner**

> WHERE filters input; HAVING filters output.

---

## Can you use **aggregate functions in WHERE**?

### ❌ **No**

**Why**

* WHERE runs **before** aggregation
* Aggregate values don’t exist yet

Correct approach:

* Move aggregate conditions to **HAVING**

💡 **Interview trap**

> Using aggregates in WHERE violates SQL’s execution order.

---

## Order of execution in an SQL query ⭐

### **Logical execution order (very important)**

1. FROM
2. JOIN
3. WHERE
4. GROUP BY
5. HAVING
6. SELECT
7. ORDER BY
8. LIMIT / OFFSET

💡 **Senior insight**

> SQL is written top-down but executed bottom-up.

---

## Why does **GROUP BY need all non-aggregated columns**?

### **Reason**

Because:

> SQL must produce **one value per group** for each selected column.

If a column is neither:

* Aggregated
* Nor grouped

Then:

* Its value is ambiguous within the group

💡 **Interview framing**

> GROUP BY defines the granularity of the result set.

---

## Can **GROUP BY work without aggregation**?

### ✅ **Yes**

### **What it does**

* Acts like `DISTINCT` on the grouped columns
* Removes duplicates

Example use cases:

* De-duplication
* Normalization queries

💡 **Senior note**

> GROUP BY without aggregation is legal but often clearer as DISTINCT.

---

## HAVING **without GROUP BY** — is it valid?

### ✅ **Yes**, but rare

### **How**

* Entire result set is treated as **one group**
* HAVING filters based on aggregate of whole table

Use case:

* Check global aggregates (e.g., total count > X)

💡 **Interview clarity**

> HAVING without GROUP BY operates on a single implicit group.

---

# 4️⃣ SUBQUERIES & CTEs

---

## What is a **subquery**?

### **Concept**

A subquery is:

> **A query nested inside another query.**

Used to:

* Provide derived data
* Apply conditional logic
* Compare against calculated results

---

## Types of subqueries

### 🔹 **Scalar subquery**

* Returns a single value

### 🔹 **Row subquery**

* Returns a single row

### 🔹 **Table subquery**

* Returns multiple rows/columns

### 🔹 **Correlated subquery**

* Depends on outer query

---

## Correlated vs Non-correlated subquery ⭐

### **Non-correlated subquery**

* Executes **once**
* Independent of outer query
* Can be optimized easily

---

### **Correlated subquery**

* Executes **once per outer row**
* References outer query columns
* Can be expensive

💡 **Interview one-liner**

> Correlated subqueries scale with rows; non-correlated don’t.

---

## Subquery vs JOIN — tradeoffs

### **JOIN**

**Pros**

* Optimizer-friendly
* Often faster
* Easier to index-optimize

**Cons**

* Can duplicate rows if not careful

---

### **Subquery**

**Pros**

* Clear intent
* Useful for existence checks
* Sometimes simpler logic

**Cons**

* Correlated versions can be slow
* Harder for optimizer (depending on DB)

💡 **Senior stance**

> Prefer JOIN for performance, subqueries for clarity—unless correlated.

---

## What is a **CTE (WITH clause)**?

### **Concept**

A CTE is:

> **A named, temporary result set defined within a query.**

Used for:

* Readability
* Reuse within a query
* Breaking complex logic into steps

💡 **Interview clarity**

> A CTE is a query-level abstraction, not a stored object.

---

## Recursive CTE — real use case

### **Concept**

Recursive CTEs repeatedly apply a query to its own output.

### **Real-world use cases**

* Hierarchies (org chart, categories)
* Graph traversal
* Tree structures

💡 **Senior insight**

> Recursive CTEs replace procedural loops in SQL.

---

## CTE vs Temporary Table

### **CTE**

* Exists only during query execution
* Not indexed (usually)
* Cleaner syntax
* Optimizer may inline it

---

### **Temporary Table**

* Physically materialized
* Can be indexed
* Reusable across queries in session
* Higher overhead

💡 **Interview rule**

> CTE for logic, temp table for reuse and indexing.

---

## Can a **CTE improve performance**?

### **It depends** ⭐

### **When it helps**

* Eliminates repeated subqueries
* Simplifies optimizer’s work
* Improves readability (indirectly reduces bugs)

### **When it hurts**

* If materialized unnecessarily
* Large intermediate results
* Prevents some optimizations (DB-specific)

💡 **Senior answer**

> CTEs improve *maintainability* first; performance depends on the execution plan.

---

## ✅ Final Interview-Ready Summary

* WHERE filters rows before aggregation
* HAVING filters groups after aggregation
* Aggregates cannot be used in WHERE
* SQL executes FROM → WHERE → GROUP BY → HAVING → SELECT
* GROUP BY defines result granularity
* GROUP BY can work without aggregates
* HAVING without GROUP BY creates one implicit group
* Correlated subqueries execute per row
* JOINs are usually more optimizer-friendly
* CTEs improve readability and structure
* Recursive CTEs handle hierarchies
* CTEs don’t guarantee performance gains—plans decide

---


---

# 5️⃣ AGGREGATION & ANALYTICAL QUERIES

---

## Difference between `COUNT(*)`, `COUNT(1)`, `COUNT(column)`

### `COUNT(*)`

> Counts **rows**, regardless of NULLs.

* Includes rows even if all columns are NULL
* Optimized by the database engine
* **Preferred in practice**

---

### `COUNT(1)`

> Counts rows as well (the literal `1` is non-NULL)

* Logically same as `COUNT(*)`
* Modern DB optimizers treat them identically

💡 **Interview truth**

> `COUNT(*)` and `COUNT(1)` are equivalent in modern databases.

---

### `COUNT(column)`

> Counts **non-NULL values** in that column.

* NULL values are ignored
* Semantically different

💡 **One-liner**

> `COUNT(column)` answers “how many values exist”, not “how many rows exist”.

---

## What is `DISTINCT`?

### **Concept**

`DISTINCT` removes **duplicate rows** from the result set.

* Applies to selected columns
* Happens **after FROM / WHERE**, before ORDER BY

---

## How does `DISTINCT` work internally?

### **Internal mechanics**

Database may use:

* Sorting
* Hashing
* Temporary structures

Steps:

1. Generate result rows
2. Group identical rows
3. Keep one copy

💡 **Senior insight**

> DISTINCT is essentially a GROUP BY without aggregates.

---

## Find **2nd highest salary** ⭐

### **Conceptual approaches**

1. Window function (preferred)
2. Subquery
3. Correlated subquery (avoid)

### **Senior reasoning**

* Window functions handle ties cleanly
* Avoid subqueries that scan repeatedly

💡 **Interview expectation**

> You should mention window functions, not just a subquery.

---

## Find **duplicate records**

### **Concept**

Duplicates are rows where:

> Certain columns repeat more than once.

Approach:

* Group by identifying columns
* Filter groups with count > 1

💡 **Senior insight**

> Duplicate detection always requires defining *what makes a row unique*.

---

## Running totals / cumulative sum

### **Problem**

> You need progressive aggregation without collapsing rows.

### **Correct tool**

> **Window functions**

They allow:

* Aggregation **without GROUP BY**
* Row-level results + analytics

💡 **Interview clarity**

> Running totals cannot be solved correctly with GROUP BY alone.

---

## Top N records per group ⭐

### **Classic senior problem**

Example:

* Top 3 salaries per department

### **Correct approach**

* Use window functions with partitioning
* Rank rows within each group
* Filter by rank

💡 **Interview gold**

> This is where window functions outperform GROUP BY.

---

## Difference between **GROUP BY and WINDOW functions**

| Aspect        | GROUP BY  | Window Function |
| ------------- | --------- | --------------- |
| Rows returned | Collapsed | Preserved       |
| Aggregation   | Yes       | Yes             |
| Row context   | Lost      | Retained        |
| Use case      | Summaries | Analytics       |

💡 **One-liner**

> GROUP BY summarizes; window functions analyze.

---

## What are **window functions**?

### **Concept**

Window functions:

> Perform calculations across a set of related rows **without collapsing them**.

Key parts:

* PARTITION BY → logical grouping
* ORDER BY → sequence
* Window frame → range of rows

💡 **Senior insight**

> Window functions bring analytics into SQL.

---

## Difference between `RANK`, `DENSE_RANK`, `ROW_NUMBER` ⭐

| Function   | Ties | Gaps |
| ---------- | ---- | ---- |
| ROW_NUMBER | ❌    | ❌    |
| RANK       | ✅    | ✅    |
| DENSE_RANK | ✅    | ❌    |

### **When to use**

* `ROW_NUMBER` → unique ordering
* `RANK` → competition ranking
* `DENSE_RANK` → leaderboard without gaps

💡 **Interview clarity**

> Choose ranking based on how you want to handle ties.

---

# 6️⃣ INDEXING (🔥 INTERVIEW GOLD)

---

## What is an **index**?

### **Concept**

An index is:

> **A data structure that allows fast lookup of rows without scanning the entire table.**

Usually implemented as:

* B-Tree (most common)
* Hash (specific cases)

💡 **One-liner**

> Indexes trade space and write cost for read speed.

---

## How does an index improve performance?

### **Without index**

* Full table scan → O(n)

### **With index**

* Navigate tree structure → O(log n)
* Jump directly to row location

💡 **Senior insight**

> Indexes reduce I/O, not CPU.

---

## Types of indexes

* Single-column
* Composite (multi-column)
* Unique
* Clustered
* Non-clustered
* Covering
* Partial / filtered (DB-specific)

---

## Clustered vs Non-clustered index ⭐

### **Clustered index**

* Defines **physical order** of table
* Only **one per table**
* Leaf nodes = actual data rows

### **Non-clustered index**

* Separate structure
* Leaf nodes store pointers to data
* Multiple allowed

💡 **Interview one-liner**

> Clustered index *is* the table; non-clustered indexes *point* to the table.

---

## Composite index — when to use?

### **Concept**

Index on **multiple columns**.

Use when:

* Queries filter on multiple columns together
* Column order matches query predicates

Rule:

> Left-most prefix rule

💡 **Senior insight**

> Index `(A, B)` helps `(A)` and `(A, B)` — not `(B)` alone.

---

## Index selectivity

### **Concept**

Selectivity measures:

> How many distinct values a column has.

* High selectivity → good index candidate
* Low selectivity (e.g., boolean) → poor candidate

💡 **Interview clarity**

> Indexes work best when they eliminate most rows.

---

## Why **too many indexes** are bad?

### **Costs**

* Slower INSERT / UPDATE / DELETE
* More memory usage
* Longer maintenance (rebuilds)
* Slower optimizer decisions

💡 **Senior rule**

> Indexes should support queries, not guesses.

---

## When is an **index NOT used**?

Common cases:

* Function on indexed column
* Leading wildcard (`LIKE '%abc'`)
* Low selectivity
* Type mismatch
* Small tables
* OR conditions (sometimes)

💡 **Interview trap**

> Just because an index exists doesn’t mean it’s usable.

---

## Index scan vs Index seek

### **Index Seek**

* Direct lookup
* Highly selective
* Best case

### **Index Scan**

* Traverses many index entries
* Less selective
* Still better than table scan

💡 **One-liner**

> Seek jumps; scan walks.

---

## What is a **covering index**?

### **Concept**

An index that:

> Contains **all columns needed by the query**.

Result:

* No table lookup
* Faster execution

💡 **Senior insight**

> Covering indexes eliminate extra I/O.

---

## What happens during **INSERT with indexes**?

For each index:

1. Calculate index key
2. Insert entry into index tree
3. Possibly rebalance tree
4. Write extra log records

💡 **Key point**

> Every index adds work to writes.

---

## Why do **indexes slow down writes**?

Because:

* Index structures must stay consistent
* More indexes → more updates
* Tree rebalancing is expensive

💡 **Interview one-liner**

> Reads benefit from indexes; writes pay the price.

---

## ✅ Final Interview-Ready Summary

* `COUNT(*)` counts rows; `COUNT(column)` ignores NULLs
* DISTINCT removes duplicates via sorting/hashing
* Window functions solve ranking & analytics problems
* GROUP BY collapses rows; window functions preserve them
* Indexes reduce I/O, not CPU
* Clustered index defines physical order
* Composite indexes depend on column order
* High selectivity = good index
* Too many indexes hurt write performance
* Index seek > index scan
* Covering indexes eliminate table access

---


---

# 7️⃣ TRANSACTIONS & ACID (🔥 MUST KNOW)

---

## What is a **transaction**?

### **Concept**

A transaction is:

> **A sequence of database operations executed as a single logical unit of work.**

Guarantee:

* Either **all operations succeed**
* Or **none of them take effect**

💡 **Interview line**

> A transaction moves the database from one consistent state to another.

---

## ACID Properties ⭐

---

### **A — Atomicity**

> All or nothing.

* Partial updates are never visible
* On failure → rollback

---

### **C — Consistency**

> Business and database rules are preserved.

* Constraints
* Indexes
* Foreign keys
* Triggers

⚠️ Consistency is **your responsibility + DB enforcement**

---

### **I — Isolation**

> Concurrent transactions behave *as if* they were executed sequentially.

* Prevents interference
* Controlled via isolation levels

---

### **D — Durability**

> Once committed, data survives crashes.

* WAL / redo logs
* Disk persistence

💡 **Interview gold**

> ACID is enforced using logging, locking, and isolation — not magic.

---

## What is **isolation**?

### **Concept**

Isolation controls:

> **How and when changes made by one transaction become visible to others.**

Trade-off:

* Higher isolation → fewer anomalies, lower concurrency
* Lower isolation → more concurrency, more anomalies

---

## Isolation Levels in SQL ⭐

(Standard SQL levels)

1. READ UNCOMMITTED
2. READ COMMITTED
3. REPEATABLE READ
4. SERIALIZABLE

Each level prevents more anomalies.

---

## Dirty Read, Non-repeatable Read, Phantom Read ⭐

---

### 🟥 Dirty Read

> Reading uncommitted data from another transaction.

* Happens in READ UNCOMMITTED
* Rarely allowed in real systems

---

### 🟨 Non-repeatable Read

> Same row read twice, values differ due to another commit.

* Prevented by REPEATABLE READ

---

### 🟦 Phantom Read

> Re-running a query returns new rows.

* Happens due to inserts/deletes
* Prevented by SERIALIZABLE (or MVCC tricks)

---

### **Summary Table**

| Isolation Level  | Dirty | Non-repeatable | Phantom |
| ---------------- | ----- | -------------- | ------- |
| READ UNCOMMITTED | ✅     | ✅              | ✅       |
| READ COMMITTED   | ❌     | ✅              | ✅       |
| REPEATABLE READ  | ❌     | ❌              | ⚠️      |
| SERIALIZABLE     | ❌     | ❌              | ❌       |

---

## Default isolation level

* **Most databases:** `READ COMMITTED`
* **MySQL (InnoDB):** `REPEATABLE READ`

💡 **Interview insight**

> Defaults are chosen to balance correctness and performance.

---

## What is a **deadlock in DB**?

### **Concept**

A deadlock occurs when:

> Two or more transactions wait forever for each other’s locks.

Example:

* T1 locks row A → waits for row B
* T2 locks row B → waits for row A

---

## How do you **detect deadlocks**?

### **Internally**

* DB maintains wait-for graphs
* Detects cycles

### **Externally**

* Error messages
* Logs
* Monitoring dashboards

💡 DB resolves deadlocks by **killing one transaction**.

---

## How do you **prevent deadlocks**?

### **Senior techniques**

* Access rows in **consistent order**
* Keep transactions short
* Avoid user interaction inside transactions
* Index foreign keys
* Reduce lock scope

💡 **Interview one-liner**

> Deadlocks are prevented by ordering, not retries.

---

## What happens during **rollback**?

### **Internals**

* Undo logs are applied
* Changes are reversed
* Locks are released
* Uncommitted data disappears

💡 **Important**

> Rollback restores data, not time.

---

# 8️⃣ LOCKING & CONCURRENCY (BANK INTERVIEW FAVORITE)

---

## What is **locking**?

### **Concept**

Locking is:

> **A mechanism to control concurrent access to shared data.**

Purpose:

* Prevent lost updates
* Maintain consistency
* Enforce isolation

---

## Row-level vs Table-level locking

### **Row-level**

* Locks only affected rows
* High concurrency
* More complex

### **Table-level**

* Locks entire table
* Simple
* Low concurrency

💡 **Senior insight**

> Modern databases prefer row-level locking.

---

## Shared lock vs Exclusive lock

### **Shared Lock (S-lock)**

* Read-only
* Multiple readers allowed
* Blocks writers

---

### **Exclusive Lock (X-lock)**

* Read/write
* Only one transaction
* Blocks all others

---

## Optimistic vs Pessimistic Locking ⭐

---

### **Optimistic Locking**

> Assume conflicts are rare.

* No locks during read
* Check version before commit
* Fail if changed

Used in:

* High-read systems
* Distributed systems
* ORMs (e.g., Hibernate @Version)

---

### **Pessimistic Locking**

> Assume conflicts are common.

* Lock on read
* Others wait
* Strong consistency

Used in:

* Banking
* Inventory systems
* Critical updates

💡 **Interview line**

> Optimistic avoids locks; pessimistic avoids surprises.

---

## What is **MVCC**?

### **Concept**

MVCC (Multi-Version Concurrency Control):

> Allows readers to see **consistent snapshots** without blocking writers.

How:

* Each row has multiple versions
* Readers read old versions
* Writers create new versions

💡 **Key benefit**

> Readers never block writers.

---

## How does **READ COMMITTED** work internally?

### **Using MVCC**

* Each query sees **only committed data**
* Snapshot refreshed per statement
* No dirty reads
* Non-repeatable reads possible

💡 **Interview insight**

> READ COMMITTED gives statement-level consistency.

---

## How does **REPEATABLE READ** prevent issues?

### **Mechanism**

* Same snapshot used for entire transaction
* Prevents non-repeatable reads
* Phantom reads handled via MVCC / locks (DB-specific)

💡 **MySQL note**

> InnoDB uses **next-key locks** to prevent phantoms.

---

## Gap locks & Next-key locks (MySQL specific)

### **Gap Lock**

* Locks a **range between index records**
* Prevents inserts

### **Next-key Lock**

* Record lock + gap lock
* Prevents phantoms

💡 **Interview gold**

> Gap locks protect ranges, not rows.

---

## What happens if **two transactions update the same row**?

### **Outcome**

* First transaction acquires X-lock
* Second transaction:

  * Waits (pessimistic)
  * Or fails version check (optimistic)

Possible results:

* Blocking
* Timeout
* Deadlock
* Retry

💡 **Senior insight**

> Concurrency problems surface at commit time, not read time.

---

## ✅ Final Interview-Ready Summary

* Transactions ensure atomic state transitions
* ACID is enforced via logging, locking, MVCC
* Isolation controls visibility between transactions
* Dirty, non-repeatable, and phantom reads are isolation anomalies
* Default isolation is usually READ COMMITTED
* Deadlocks are detected via wait graphs
* Rollback uses undo logs
* Row-level locking enables concurrency
* Optimistic locking scales; pessimistic locking protects
* MVCC allows non-blocking reads
* Gap locks prevent phantom inserts
* Conflicts are resolved by blocking, aborting, or retrying

---


---

# 9️⃣ PERFORMANCE & QUERY OPTIMIZATION (⭐ Senior Signal)

---

## How do you **optimize a slow SQL query?** ⭐

### **Senior mindset**

Never start by rewriting SQL blindly.
Follow a **diagnostic sequence**.

---

### ✅ Step-by-step approach

#### 1️⃣ **Understand the query intent**

* What business question is it answering?
* Expected result size?
* Real usage pattern?

> Many slow queries are *doing the wrong thing efficiently*.

---

#### 2️⃣ **Check execution plan (`EXPLAIN`)**

This tells you:

* Access paths (index scan vs table scan)
* Join methods
* Estimated vs actual rows
* Cost hotspots

---

#### 3️⃣ **Check indexes**

* Are indexes aligned with:

  * WHERE clauses
  * JOIN conditions
  * ORDER BY / GROUP BY
* Check composite index order

---

#### 4️⃣ **Reduce data early**

* Filter as early as possible
* Avoid `SELECT *`
* Push predicates into WHERE, not HAVING

---

#### 5️⃣ **Fix cardinality problems**

* Bad joins
* Missing predicates
* Wrong assumptions about uniqueness

---

### **Interview one-liner**

> Query tuning is about reducing data as early and as cheaply as possible.

---

## What is **EXPLAIN / execution plan**?

### **Concept**

`EXPLAIN` shows:

> **How the database plans to execute your query.**

It reveals:

* Join order
* Index usage
* Scan types
* Estimated row counts
* Cost estimates

💡 **Senior insight**

> SQL performance is decided by the execution plan, not query syntax.

---

## What is a **full table scan**?

### **Concept**

A full table scan means:

> **The database reads every row of the table.**

Occurs when:

* No usable index
* Low selectivity
* Optimizer thinks scan is cheaper
* Small table

💡 **Key point**

> Full scan is not always bad — only bad when unnecessary.

---

## Why does a query work **fast in dev but slow in prod**?

### **Classic senior question**

### **Root causes**

* Data volume difference
* Missing indexes in prod
* Outdated statistics
* Different execution plans
* Parameter sniffing
* Different hardware / I/O
* Concurrent load

💡 **Interview gold**

> Queries fail in prod because *optimizers optimize for data*, not schema.

---

## How do you **detect missing indexes**?

### **Approaches**

* Execution plan shows table scans
* Slow query logs
* Index usage statistics
* Queries filtering/joining on non-indexed columns

### **Heuristic**

> Columns used frequently in WHERE / JOIN / ORDER BY are index candidates.

---

## How do **statistics affect query plans**?

### **Concept**

Statistics tell the optimizer:

* Row counts
* Data distribution
* Cardinality
* Selectivity

If stats are stale:

* Optimizer makes bad choices
* Wrong join order
* Wrong index usage

💡 **Senior insight**

> Bad statistics = bad execution plan.

---

## How does DB **choose an index**?

### **Decision factors**

* Selectivity
* Cardinality
* Index depth
* Cost estimate
* Covered columns

The optimizer picks the **cheapest plan**, not the “best looking” one.

💡 **Interview clarity**

> Index choice is probabilistic, not deterministic.

---

## Query tuning vs Schema tuning

### **Query tuning**

* Rewrite SQL
* Add predicates
* Change joins
* Reduce columns

### **Schema tuning**

* Add/remove indexes
* Change column types
* Normalize / denormalize
* Partition tables

💡 **Senior rule**

> Query tuning fixes symptoms; schema tuning fixes root causes.

---

## Pagination problems (OFFSET vs Keyset) ⭐

---

### ❌ OFFSET pagination

```sql
LIMIT 10 OFFSET 100000
```

**Problems**

* DB must scan and discard rows
* Gets slower as OFFSET grows
* Unstable with concurrent inserts

---

### ✅ Keyset (seek) pagination

```sql
WHERE id > last_seen_id
LIMIT 10
```

**Benefits**

* Uses index efficiently
* Stable performance
* Scales to millions of rows

💡 **Interview one-liner**

> OFFSET pagination doesn’t scale; keyset pagination does.

---

## How do you handle **large tables (millions of rows)**?

### **Senior strategies**

* Proper indexing
* Partitioning
* Archiving old data
* Avoid OFFSET
* Batch processing
* Asynchronous jobs
* Read replicas

💡 **Key mindset**

> Large data requires architectural solutions, not just SQL tricks.

---

# 🔟 NULLs, DATA QUALITY & EDGE CASES

---

## How does **NULL work in SQL**?

### **Concept**

`NULL` means:

> **Unknown or missing value**, not zero or empty.

Key property:

* NULL is not equal to anything — even another NULL

---

## NULL vs empty string

| NULL                     | Empty String      |
| ------------------------ | ----------------- |
| Unknown                  | Known value       |
| No storage (DB-specific) | Stored            |
| Needs IS NULL            | Compared normally |

💡 **Interview trap**

> NULL ≠ '' ≠ 0

---

## How does **NULL affect joins**?

### **Key behavior**

* NULL never matches NULL
* `=` comparison fails
* Rows with NULL join keys may be excluded

💡 **Senior insight**

> NULL breaks equality joins.

---

## NULL with aggregate functions

* COUNT(column) → ignores NULL
* SUM / AVG → ignores NULL
* COUNT(*) → counts rows

💡 **Interview clarity**

> Aggregates skip NULL by design.

---

## IS NULL vs = NULL ⭐

### ❌ Wrong

```sql
column = NULL
```

Always returns UNKNOWN (false).

### ✅ Correct

```sql
column IS NULL
```

💡 **Interview one-liner**

> NULL requires IS, not =.

---

## How do you **handle NULL safely** in queries?

### **Techniques**

* COALESCE
* Default values
* IS NULL checks
* Proper schema constraints
* Avoid NULLs when possible

💡 **Senior rule**

> Handle NULLs explicitly — never assume.

---

## COALESCE vs NVL

### **COALESCE**

* ANSI SQL standard
* Supports multiple arguments
* Portable

### **NVL**

* Oracle-specific
* Two arguments only

💡 **Interview stance**

> Prefer COALESCE for portability.

---

## ✅ Final Interview-Ready Summary

* Slow queries must be diagnosed, not guessed
* EXPLAIN is the primary tuning tool
* Full table scans are sometimes valid
* Dev vs prod slowness is usually data & stats
* Indexes depend on selectivity & statistics
* OFFSET pagination doesn’t scale
* Large tables require architectural thinking
* NULL represents unknown, not empty
* NULL breaks equality and joins
* Aggregates ignore NULL
* Always use IS NULL
* COALESCE is portable and safer

---


---

# 1️⃣1️⃣ CONSTRAINTS & NORMALIZATION

---

## Types of **constraints**

### **Primary Key**

* Uniquely identifies a row
* Enforces entity integrity
* Automatically indexed

---

### **Foreign Key**

* Enforces referential integrity
* Prevents orphan records

---

### **Unique**

* Ensures uniqueness (non-primary)
* Can be multiple per table

---

### **Not Null**

* Prevents missing data
* Improves data quality

---

### **Check**

* Enforces business rules
* Validates column values

---

### **Default**

* Assigns default value when none provided

💡 **Interview insight**

> Constraints push correctness into the database, not application code.

---

## What is **normalization**?

### **Concept**

Normalization is:

> **The process of structuring data to reduce redundancy and dependency.**

Goals:

* Eliminate duplication
* Avoid update anomalies
* Improve data integrity

💡 **Senior framing**

> Normalization optimizes correctness; performance comes later.

---

## Normal Forms ⭐

---

### **1NF — First Normal Form**

> Eliminate repeating groups.

Rules:

* Atomic values
* No multi-valued columns

---

### **2NF — Second Normal Form**

> Eliminate partial dependency.

Rules:

* Already in 1NF
* Non-key columns depend on the **whole primary key**

---

### **3NF — Third Normal Form**

> Eliminate transitive dependency.

Rules:

* Already in 2NF
* Non-key columns depend **only on the key**

💡 **Interview one-liner**

> 1NF fixes structure, 2NF fixes keys, 3NF fixes relationships.

---

## When would you **denormalize**?

### **Use denormalization when**

* Read performance is critical
* Join cost is high
* Data is relatively stable
* Reporting-heavy systems

Examples:

* Analytics tables
* Caching tables
* Materialized views

💡 **Senior rule**

> Normalize first, denormalize deliberately.

---

## Pros & cons of **denormalization**

### **Pros**

* Faster reads
* Fewer joins
* Simpler queries

### **Cons**

* Data duplication
* Update anomalies
* More complex writes
* Integrity harder to enforce

💡 **Interview insight**

> Denormalization trades correctness complexity for performance.

---

## Cascading deletes — pros & cons

---

### **Pros**

* Simplifies cleanup
* Prevents orphan records
* Encodes business rules in DB

---

### **Cons**

* Dangerous in production
* Hard to audit
* Risk of accidental mass deletion
* Hidden side effects

💡 **Senior stance**

> Cascades should be explicit, rare, and well-documented.

---

# 1️⃣2️⃣ DATABASE DESIGN (Backend Focus)

---

## How do you **design tables for performance**?

### **Senior checklist**

* Choose proper primary keys
* Use correct data types
* Index selectively
* Avoid over-normalization
* Partition large tables
* Avoid wide tables when possible

💡 **Key mindset**

> Schema design defines your performance ceiling.

---

## Choosing **data types** — why it matters

### **Why important**

* Storage size
* Cache efficiency
* Index size
* Comparison speed

### **Example**

* INT vs BIGINT
* VARCHAR vs CHAR
* TIMESTAMP vs DATETIME

💡 **Interview line**

> Smaller data types = better cache utilization.

---

## Storing **dates & timestamps**

### **Best practices**

* Use native date/time types
* Store in UTC
* Convert in application layer
* Avoid string storage

💡 **Senior insight**

> Time zones belong to presentation, not storage.

---

## UUID vs Auto-increment IDs ⭐

---

### **Auto-increment**

**Pros**

* Small
* Fast indexing
* Sequential inserts
* Cache-friendly

**Cons**

* Predictable
* Harder in distributed systems

---

### **UUID**

**Pros**

* Globally unique
* Great for distributed systems
* No coordination needed

**Cons**

* Large index size
* Random inserts → index fragmentation
* Slower joins

💡 **Interview gold**

> UUIDs scale globally; auto-increments scale locally.

---

## Soft delete vs Hard delete

---

### **Soft delete**

* Mark record as deleted
* Data retained

**Pros**

* Auditability
* Recovery
* Compliance

**Cons**

* Query complexity
* Index bloat
* Risk of accidental inclusion

---

### **Hard delete**

* Physically remove row

**Pros**

* Clean data
* Better performance

**Cons**

* Data loss
* No audit trail

💡 **Senior stance**

> Soft delete for business data, hard delete for technical data.

---

## Audit columns (`created_at`, `updated_at`)

### **Purpose**

* Tracking changes
* Debugging
* Compliance
* Analytics

### **Best practices**

* Use DB-generated timestamps
* Immutable `created_at`
* Update `updated_at` automatically

💡 **Interview insight**

> Audit columns are cheap insurance.

---

## Multi-tenant schema design

---

### **Common approaches**

#### 1️⃣ Shared schema, tenant_id column

* Simple
* Scales well
* Needs strict isolation logic

#### 2️⃣ Separate schema per tenant

* Better isolation
* Harder to manage

#### 3️⃣ Separate database per tenant

* Strong isolation
* High operational cost

💡 **Senior rule**

> Isolation level increases from column → schema → database.

---

## ✅ Final Interview-Ready Summary

* Constraints enforce correctness at DB level
* Normalization reduces redundancy and anomalies
* 1NF → atomicity, 2NF → key dependency, 3NF → transitive dependency
* Denormalize for read-heavy performance
* Cascading deletes are powerful but dangerous
* Schema design sets performance limits
* Data type choice impacts storage & speed
* UUID vs auto-increment is a distribution tradeoff
* Soft deletes improve auditability
* Audit columns are essential
* Multi-tenancy is a balance between isolation and complexity

---


---

# 1️⃣3️⃣ SCENARIO-BASED SQL QUESTIONS (VERY IMPORTANT)

---

## 🟥 Query taking **10 seconds** — how do you debug? ⭐

### **Senior mindset**

Never guess. **Measure first.**

---

### **Step-by-step approach**

#### 1️⃣ Reproduce safely

* Same query
* Same parameters
* Same environment (or prod replica)

---

#### 2️⃣ Get execution plan (`EXPLAIN / EXPLAIN ANALYZE`)

Look for:

* Full table scans
* Wrong join order
* Index scans vs seeks
* Estimated vs actual rows mismatch

---

#### 3️⃣ Check indexes

* Missing indexes on:

  * WHERE
  * JOIN
  * ORDER BY / GROUP BY
* Composite index order correctness

---

#### 4️⃣ Reduce data early

* Add predicates
* Move filters from HAVING to WHERE
* Avoid SELECT *

---

#### 5️⃣ Validate data assumptions

* Cardinality
* Duplicate joins
* Skewed data

💡 **Interview one-liner**

> Performance problems are execution plan problems.

---

## 🟨 Duplicate records appearing — how do you fix?

### **Diagnosis**

* Identify what “duplicate” means (business key)
* Check if uniqueness is enforced in DB

---

### **Fix**

1. Remove existing duplicates
2. Add **UNIQUE constraint**
3. Fix application logic
4. Add idempotency safeguards

💡 **Senior insight**

> Duplicates are data integrity bugs, not query bugs.

---

## 🟥 Data inconsistency between services — what do you do?

### **Root causes**

* Eventual consistency
* Failed retries
* Partial updates
* Missing transactions

---

### **Senior solution**

* Identify source of truth
* Add reconciliation jobs
* Implement idempotency
* Use outbox pattern / CDC
* Improve observability

💡 **Interview gold**

> Distributed consistency is a design problem, not a SQL problem.

---

## 🟥 DB CPU at **100%** — steps to investigate ⭐

### **Step-by-step**

#### 1️⃣ Identify top queries

* Slow query logs
* APM
* Query statistics

---

#### 2️⃣ Check execution plans

* Full scans
* Hash joins on large datasets
* Bad cardinality estimates

---

#### 3️⃣ Check indexing

* Missing indexes
* Unused indexes
* Index bloat

---

#### 4️⃣ Check concurrency

* Lock contention
* Deadlocks
* Hot rows

---

#### 5️⃣ Check system-level factors

* I/O wait
* Memory pressure
* Context switching

💡 **Senior rule**

> High DB CPU usually means inefficient queries or missing indexes.

---

## 🟥 Deadlocks in production — how do you resolve?

### **Resolution steps**

1. Capture deadlock graphs
2. Identify conflicting queries
3. Enforce consistent lock ordering
4. Reduce transaction scope
5. Add retries with backoff

💡 **Interview clarity**

> Deadlocks are fixed by design discipline, not configuration.

---

## 🟨 Migration added column — table locked — what went wrong?

### **Root cause**

* DDL operation requiring full table rewrite
* Long metadata locks
* Blocking writes

---

### **Correct approach**

* Online schema change
* Add nullable column first
* Backfill in batches
* Add constraints later

💡 **Senior insight**

> Schema migrations must be designed for production traffic.

---

## 🟥 Large DELETE query timed out — solution?

### **Problem**

* DELETE locks rows
* Generates undo logs
* Can block readers/writers

---

### **Fixes**

* Delete in batches
* Use indexed predicates
* Use soft deletes
* Archive then delete
* Partition pruning

💡 **Interview one-liner**

> Large deletes should be incremental, not monolithic.

---

# 🧠 COMMONLY MISSED BUT ASKED

---

## Difference between **UNION and UNION ALL** ⭐

| UNION                    | UNION ALL        |
| ------------------------ | ---------------- |
| Removes duplicates       | Keeps duplicates |
| Sorting/hashing required | No extra work    |
| Slower                   | Faster           |

💡 **Senior rule**

> Use UNION ALL unless you need deduplication.

---

## EXISTS vs IN

### **EXISTS**

* Stops at first match
* Correlated
* Efficient for large subqueries

### **IN**

* Compares full set
* Can be inefficient with NULLs

💡 **Interview insight**

> EXISTS checks existence; IN checks membership.

---

## TRUNCATE rollback behavior

### **Behavior**

* DDL in most DBs
* Auto-commit
* Cannot be rolled back (usually)

💡 **Interview trap**

> TRUNCATE bypasses transactional safety.

---

## Temporary tables vs CTE

### **Temporary tables**

* Materialized
* Can be indexed
* Persist for session

### **CTE**

* Logical
* Cleaner syntax
* Usually not indexed

💡 **Senior stance**

> CTEs are for readability; temp tables are for performance reuse.

---

## SQL Injection — how to prevent?

### **Prevention**

* Prepared statements
* Parameter binding
* ORM query builders
* Input validation

💡 **Interview one-liner**

> Never concatenate SQL strings with user input.

---

## Prepared statements — why faster?

### **Reasons**

* Precompiled execution plan
* Reduced parsing
* Plan reuse
* Safer execution

💡 **Senior insight**

> Prepared statements reduce CPU and improve security.

---

## How ORMs generate SQL — pros & cons

### **Pros**

* Faster development
* Abstraction
* Portability

### **Cons**

* Inefficient queries
* N+1 problems
* Harder debugging
* Hidden performance issues

💡 **Interview framing**

> ORMs improve productivity, not performance.

---

## N+1 problem at DB level

### **Concept**

* 1 query to fetch parents
* N queries to fetch children

### **Impact**

* High latency
* DB overload

### **Fix**

* Joins
* Fetch joins
* Batch fetching

💡 **Interview clarity**

> N+1 is a query design problem, not a database limitation.

---

## ✅ Final Interview-Ready Summary

* Always debug slow queries via execution plans
* Data integrity requires DB constraints
* DB CPU spikes point to inefficient queries
* Deadlocks require consistent access patterns
* Schema migrations must be production-safe
* Large deletes must be batched
* UNION ALL is faster than UNION
* EXISTS often outperforms IN
* TRUNCATE bypasses transactions
* Prepared statements improve performance and security
* ORMs trade performance for productivity
* N+1 is a design anti-pattern

---
