Below is a **senior-level, concept-first explanation of REST & HTTP**, focused on **semantics, guarantees, and real interview traps**—not textbook definitions.

---

# 1️⃣ REST Fundamentals (Must Know)

---

## What is **REST**?

### **Concept**

REST (Representational State Transfer) is:

> **An architectural style for building scalable, loosely-coupled networked systems.**

It defines **constraints** on how clients and servers interact—not APIs, not payloads.

💡 **Interview line**

> REST describes *how systems should behave*, not *how endpoints should look*.

---

## REST vs **SOAP** ⭐

| REST                | SOAP                   |
| ------------------- | ---------------------- |
| Architectural style | Protocol               |
| Lightweight         | Heavy (XML, envelopes) |
| Uses HTTP semantics | Abstracted over HTTP   |
| JSON/XML            | XML only               |
| Easy to scale       | Complex tooling        |

💡 **Senior stance**

> SOAP standardizes everything; REST standardizes principles.

---

## What does **RESTful** mean?

An API is RESTful if it:

* Treats **everything as a resource**
* Uses **HTTP methods correctly**
* Is **stateless**
* Uses **standard status codes**
* Follows **REST constraints**

💡 **Interview clarity**

> RESTful ≠ “uses JSON over HTTP”.

---

## Is REST a **protocol** or an **architectural style**?

### ✅ **Architectural style**

* No strict spec
* No mandatory format
* Implemented *using* HTTP

💡 **Interview trap**

> REST is not something you “install”.

---

## Core **REST constraints** ⭐

1. **Client–Server**
2. **Stateless**
3. **Cacheable**
4. **Uniform Interface**
5. **Layered System**
6. **Code on Demand** (optional)

💡 **Interview gold**

> Stateless + uniform interface = scalability.

---

## What is **statelessness** in REST?

### **Concept**

Each request:

> **Contains all information required to process it.**

* Server stores **no client session**
* Authentication via tokens, not sessions

---

## Why is statelessness important?

* Horizontal scalability
* Easier caching
* Simpler failure recovery
* No sticky sessions

💡 **Senior insight**

> Statelessness shifts complexity from server memory to request design.

---

## What is a **resource** in REST?

A resource is:

> **A conceptual entity identified by a URI.**

Examples:

* `/users/123`
* `/orders/987`

---

## Resource vs **representation**

| Resource        | Representation    |
| --------------- | ----------------- |
| Concept         | Concrete format   |
| Stable identity | Can vary          |
| `/users/123`    | JSON / XML / HTML |

💡 **Interview line**

> You manipulate representations, not resources.

---

## What is **HATEOAS**?

### **Concept**

Hypermedia As The Engine Of Application State:

> Clients discover actions via links in responses.

Example:

```json
{
  "id": 123,
  "links": {
    "update": "/users/123",
    "delete": "/users/123"
  }
}
```

💡 **Reality**

* Rarely implemented fully
* Shows deep REST understanding

---

# 2️⃣ HTTP METHODS (Very Common)

---

## GET vs POST vs PUT vs PATCH vs DELETE ⭐

| Method | Purpose         | Idempotent |
| ------ | --------------- | ---------- |
| GET    | Read            | ✅          |
| POST   | Create / Action | ❌          |
| PUT    | Replace         | ✅          |
| PATCH  | Partial update  | ❌          |
| DELETE | Remove          | ✅          |

💡 **Interview clarity**

> Method choice defines semantics, not payload shape.

---

## PUT vs PATCH — when to use which?

### **PUT**

* Full replacement
* Client knows complete resource
* Missing fields are removed

### **PATCH**

* Partial update
* Only changed fields sent

💡 **Senior rule**

> PUT = replace, PATCH = modify.

---

## Can GET have a **request body**?

### ⚠️ Technically yes, **practically no**

* HTTP spec allows it
* Servers/proxies ignore it
* Breaks caching

💡 **Interview answer**

> Avoid it—use query params.

---

## Is **DELETE idempotent**?

### ✅ **Yes**

* First DELETE removes resource
* Subsequent DELETEs have no additional effect

💡 **Key point**

> Idempotent ≠ safe.

---

## Which HTTP methods are **idempotent**? ⭐

* GET
* PUT
* DELETE
* HEAD
* OPTIONS

---

## Safe vs Unsafe HTTP methods

### **Safe**

* Do not modify server state
* GET, HEAD

### **Unsafe**

* Modify state
* POST, PUT, PATCH, DELETE

💡 **Interview line**

> Safe methods can be cached; unsafe cannot.

---

## When should **POST be idempotent**?

### **Normally**

* POST is **not idempotent**

### **Exception**

* When client supplies idempotency key
* Payment APIs, retries

💡 **Senior insight**

> Idempotency can be *designed*, even if method isn’t.

---

## Why POST is **not idempotent**?

Because:

* Server decides resource identity
* Multiple identical POSTs → multiple resources

---

## What happens if **PUT is called twice**?

* Same state after both calls
* No additional side effects

💡 **Interview clarity**

> PUT’s idempotency is about *state*, not execution count.

---

## Can **POST be cached**?

### ❌ **By default**

* No, because side effects are unknown

### ⚠️ Possible with explicit headers (rare)

---

# 3️⃣ HTTP STATUS CODES (Interview Favorite)

| Range | Meaning           |
| ----- | ----------------- |
| `1xx` | Informational     |
| `2xx` | Success           |
| `3xx` | Redirection       |
| `4xx` | Client-side error |
| `5xx` | Server-side error |

| Code             | Meaning                               | When to use                                 |
| ---------------- | ------------------------------------- | ------------------------------------------- |
| `200 OK`         | Request succeeded                     | Successful `GET`, `PUT`, or normal response |
| `201 Created`    | New resource created                  | Successful `POST` that creates something    |
| `202 Accepted`   | Request accepted but processing later | Async processing, queue-based request       |
| `204 No Content` | Success but no response body          | Successful `DELETE` or update with no body  |

| Code                    | Meaning                       | When to use                     |
| ----------------------- | ----------------------------- | ------------------------------- |
| `301 Moved Permanently` | Resource permanently moved    | Old URL permanently replaced    |
| `302 Found`             | Temporary redirect            | Temporary redirect              |
| `304 Not Modified`      | Cached version is still valid | Browser/client cache validation |

| Code                         | Meaning                                  | When to use                                            |
| ---------------------------- | ---------------------------------------- | ------------------------------------------------------ |
| `400 Bad Request`            | Invalid request                          | Malformed JSON, missing required fields, invalid input |
| `401 Unauthorized`           | Not authenticated                        | User did not log in or token is missing/invalid        |
| `403 Forbidden`              | Authenticated but not allowed            | User logged in but lacks permission                    |
| `404 Not Found`              | Resource does not exist                  | Order, user, trade, or endpoint not found              |
| `405 Method Not Allowed`     | HTTP method not supported                | `POST` used on endpoint that only supports `GET`       |
| `408 Request Timeout`        | Client took too long                     | Request timeout from client side                       |
| `409 Conflict`               | Conflict with current state              | Duplicate resource, optimistic locking failure         |
| `415 Unsupported Media Type` | Unsupported content type                 | Client sends XML but API expects JSON                  |
| `422 Unprocessable Entity`   | Valid syntax, failed business validation | Quantity negative, invalid trade state                 |
| `429 Too Many Requests`      | Rate limit exceeded                      | Too many requests from same client                     |

| Code                        | Meaning                            | When to use                                 |
| --------------------------- | ---------------------------------- | ------------------------------------------- |
| `500 Internal Server Error` | Generic server failure             | Unexpected exception                        |
| `502 Bad Gateway`           | Bad response from upstream service | Gateway/proxy got invalid response          |
| `503 Service Unavailable`   | Service temporarily unavailable    | Overloaded, maintenance, dependency down    |
| `504 Gateway Timeout`       | Upstream timeout                   | Gateway waited too long for another service |


💡 **Senior rule**

> Status code tells *what happened*, not just “success”.

---

## When should you return **201 Created**?

* On successful resource creation
* Include `Location` header pointing to new resource

---

## 400 vs 401 vs 403 ⭐

| Code | Meaning                          |
| ---- | -------------------------------- |
| 400  | Bad request (invalid input)      |
| 401  | Unauthorized (not authenticated) |
| 403  | Forbidden (not allowed)          |

💡 **Interview trap**

> 401 ≠ “no permission”.

---

## 404 vs 410

* **404**: Resource not found (may exist later)
* **410**: Resource permanently gone

💡 **Use 410** for deleted, non-recoverable resources.

---

## 409 Conflict — real use cases ⭐

Use when:

* Version mismatch (optimistic locking)
* Duplicate creation
* Business rule conflict

💡 **Interview gold**

> 409 signals *state conflict*, not invalid input.

---

## 422 Unprocessable Entity — when to use?

* Request is syntactically valid
* Semantically invalid

Example:

* Invalid state transition
* Domain validation failure

---

## 500 vs 502 vs 503

| Code | Meaning                                    |
| ---- | ------------------------------------------ |
| 500  | Server bug                                 |
| 502  | Bad gateway (upstream error)               |
| 503  | Service unavailable (overload/maintenance) |

💡 **Senior insight**

> 503 tells clients to retry later.

---

## When to return **202 Accepted**?

* Request accepted for **async processing**
* Processing not completed yet

Examples:

* Background jobs
* Long-running tasks

---

## Why not always return **200**?

Because:

* Loses semantic meaning
* Breaks client behavior (retries, caching)
* Hides errors

💡 **Interview line**

> Status codes are part of the API contract.

---

## ✅ Final Interview-Ready Summary

* REST is an architectural style, not a protocol
* Statelessness enables scalability
* Resources ≠ representations
* HTTP methods define intent and guarantees
* Idempotency is about *state*, not execution
* GET is safe; POST is not
* Correct status codes matter
* 409 = conflict, not validation error
* 202 = async accepted
* Returning 200 everywhere is an anti-pattern

---

Below is a **senior-level, interview-grade guide to REST API Design & Request/Response modeling**. This focuses on **principles, trade-offs, and real production decisions**, not surface rules.

---

# 4️⃣ REST API DESIGN & URI NAMING (High Signal)

---

## RESTful URI naming **best practices** ⭐

### **Core rule**

> URIs identify **resources**, not actions.

### **Guidelines**

* Use **nouns**, not verbs
* Use **lowercase**
* Use **hyphens**, not underscores
* Avoid file extensions
* Be consistent

### **Good examples**

```
/users
/users/123
/orders/987/items
```

### **Bad examples**

```
/getUser
/createOrder
/update-user
```

💡 **Interview one-liner**

> URLs describe *what*, HTTP methods describe *how*.

---

## Plural vs singular resources

### **Best practice**

* Use **plural** for collections
* Singular only for sub-resources or conceptual entities

```
/users
/users/{id}
```

💡 **Why plural?**

* Avoids ambiguity
* Consistent collection semantics

---

## Verbs vs nouns in URLs

### ❌ Avoid verbs

```
/createOrder
/payInvoice
```

### ✅ Use nouns + HTTP methods

```
POST /orders
POST /invoices/{id}/payments
```

💡 **Interview clarity**

> Actions belong in HTTP methods, not URLs.

---

## Nested resources — when to use?

### **Use nesting when**

* Child resource **cannot exist independently**
* Relationship is strong

Example:

```
/orders/{orderId}/items
```

---

## Deep nesting problems

### ❌ Bad

```
/users/{id}/orders/{orderId}/items/{itemId}/details
```

### **Problems**

* Hard to maintain
* Breaks flexibility
* Tightly couples resources

💡 **Senior rule**

> Nest only one level deep.

---

## How to model **relationships** in REST?

### **Approaches**

1. Nested endpoints
2. Links (HATEOAS-lite)
3. Query parameters

Example:

```
GET /orders?userId=123
```

💡 **Interview insight**

> Relationships are navigational, not hierarchical.

---

## Filtering vs searching vs sorting

### **Filtering**

Reduce result set

```
/orders?status=PAID
```

### **Searching**

Full-text / fuzzy

```
/orders?q=invoice
```

### **Sorting**

Order results

```
/orders?sort=createdAt,desc
```

💡 **Rule**

> One concern per parameter.

---

## Pagination — **offset vs cursor** ⭐

### **Offset pagination**

```
?page=5&size=20
```

**Pros**

* Simple

**Cons**

* Slow for large datasets
* Inconsistent results

---

### **Cursor pagination**

```
?cursor=eyJpZCI6MTAwfQ==
```

**Pros**

* Fast
* Stable

**Cons**

* Harder to implement

💡 **Senior rule**

> Offset for admin tools; cursor for high-scale APIs.

---

## How do you design **bulk APIs**?

### **Design principles**

* Single endpoint
* Array-based payload
* Partial success support

Example:

```
POST /orders/bulk
```

Response includes:

* Success items
* Failed items with reasons

💡 **Interview insight**

> Bulk APIs need failure isolation.

---

## How do you design **partial updates**?

### **Best approach**

* Use **PATCH**
* Define semantics clearly
* Avoid ambiguous updates

Example:

```json
{
  "status": "CANCELLED"
}
```

💡 **Senior clarity**

> PATCH modifies state; PUT replaces state.

---

## How do you expose **actions** in REST?

### **Correct way**

Model actions as **resources** or **state transitions**

Example:

```
POST /orders/{id}/cancel
```

or

```
PATCH /orders/{id}
{ "status": "CANCELLED" }
```

💡 **Interview stance**

> Actions are just resource state changes.

---

# 5️⃣ REQUEST / RESPONSE DESIGN

---

## What goes in **headers vs body**?

### **Headers**

* Metadata
* Authentication
* Content negotiation
* Correlation IDs

### **Body**

* Resource data
* Payload content

💡 **Rule**

> Headers describe the message; body contains the message.

---

## What is **Content-Type**?

Defines:

> **Format of the request/response body**

Example:

```
Content-Type: application/json
```

---

## What is **Accept** header?

Defines:

> **Formats the client can understand**

Example:

```
Accept: application/json
```

---

## Content negotiation ⭐

### **Concept**

Server selects response format based on:

* Accept header
* Server capability

💡 **Interview clarity**

> Content negotiation decouples clients from formats.

---

## JSON vs XML — tradeoffs

| JSON           | XML               |
| -------------- | ----------------- |
| Lightweight    | Verbose           |
| Human-readable | Schema-heavy      |
| Faster parsing | Strong validation |
| Web-friendly   | Enterprise legacy |

💡 **Senior stance**

> JSON dominates modern APIs; XML survives in legacy systems.

---

## How do you design **consistent API responses**?

### **Standard structure**

```json
{
  "data": {...},
  "meta": {...},
  "errors": [...]
}
```

Benefits:

* Predictable
* Easier client handling

---

## Error response design **best practices** ⭐

### **Include**

* Error code (machine-readable)
* Message (human-readable)
* Field-level details
* Correlation ID

Example:

```json
{
  "errorCode": "ORDER_NOT_FOUND",
  "message": "Order does not exist",
  "details": { "orderId": "123" }
}
```

💡 **Interview gold**

> Errors are part of the API contract.

---

## Should error messages be **human-readable**?

### ✅ Yes — but carefully

* Safe for clients
* No internal stack traces
* No sensitive info

💡 **Senior rule**

> Human-readable for users, machine-readable for systems.

---

## How do you **version API responses**?

### **Common strategies**

1. URI versioning (`/v1`)
2. Header versioning
3. Media-type versioning

💡 **Industry default**

> URI versioning for simplicity.

---

## How do you handle **backward compatibility**?

### **Best practices**

* Never remove fields
* Add optional fields
* Maintain old versions
* Deprecate gracefully
* Version breaking changes

💡 **Interview clarity**

> Backward compatibility is a promise, not an accident.

---

## ✅ Final Interview-Ready Summary

* URIs represent nouns, not verbs
* Use plural resources
* Nest sparingly
* Model relationships carefully
* Filtering, searching, sorting are distinct
* Cursor pagination scales better
* Bulk APIs need partial success handling
* PATCH for partial updates
* Headers = metadata, body = data
* Content negotiation decouples formats
* Consistent responses reduce client complexity
* Errors must be structured and safe
* Versioning is inevitable
* Backward compatibility must be intentional

---

### 🔥 One-line senior takeaway

> Great REST APIs are **predictable, semantic, and evolvable**.

Below is a **senior-level, interview-grade explanation** of **Idempotency, Safety, Authentication, and Authorization in REST APIs**. I’ll focus on **why these exist, how they fail in production, and how seniors design them**, not textbook definitions.

---

# 6️⃣ IDENTITY, IDEMPOTENCY & SAFETY

---

## What is **idempotency**? ⭐

### **Concept**

An operation is **idempotent** if:

> **Calling it multiple times produces the same final state as calling it once.**

Key point:

* Idempotency is about **state**, not **number of executions**

Example:

* `PUT /users/123` → same result even if called 10 times
* `DELETE /orders/5` → deleted once, remains deleted

💡 **Interview one-liner**

> Idempotency means “safe to retry”.

---

## Why **idempotency matters** in REST?

Because **failures are normal**:

* Network timeouts
* Client retries
* Load balancer retries
* Mobile clients resending requests

Without idempotency:

* Duplicate payments
* Duplicate orders
* Corrupt state

💡 **Senior insight**

> If your API isn’t idempotent, retries become data bugs.

---

## How do you implement **idempotency in POST**?

POST is **not idempotent by default**, but you can **design it to be**.

### **Approach**

* Client sends a unique idempotency key
* Server stores the result for that key
* Subsequent requests with same key return the same result

Example:

```
POST /payments
Idempotency-Key: 8f3a-91bc-42
```

💡 **Key idea**

> Idempotency is enforced by the server, not the HTTP method.

---

## Idempotency-Key header — **how it works**

### **Flow**

1. Client generates unique key
2. Server checks if key exists
3. If not:

   * Process request
   * Store response against key
4. If yes:

   * Return stored response
   * Do NOT re-execute logic

### **Storage**

* DB table
* Cache (Redis)
* TTL-based cleanup

💡 **Interview gold**

> Idempotency keys turn retries into cache hits.

---

## How do **retries affect APIs**?

Retries can cause:

* Duplicate writes
* Inconsistent state
* Side-effect amplification

### **Safe retries require**

* Idempotent endpoints
* Idempotent downstream systems
* Proper timeout handling

💡 **Senior rule**

> Retries are inevitable; unsafe APIs are optional.

---

## Duplicate request handling

### **Techniques**

* Idempotency keys
* Natural business keys (orderId, paymentId)
* Unique DB constraints
* Deduplication tables

💡 **Interview clarity**

> Deduplication belongs at the system boundary.

---

## How do you prevent **double submissions**?

Common in:

* Payments
* Forms
* Mobile apps

### **Solutions**

* Idempotency keys
* Client-generated request IDs
* Server-side locking
* Optimistic locking
* UI disable-on-submit (not sufficient alone)

💡 **Senior insight**

> UI prevention is convenience; backend prevention is correctness.

---

## Idempotent **consumer** vs idempotent **API**

### **Idempotent API**

* Same request → same state
* Protects server state

### **Idempotent Consumer**

* Can process same message multiple times safely
* Protects downstream systems

💡 **Interview gold**

> APIs handle retries; consumers handle duplicates.

---

# 7️⃣ AUTHENTICATION & AUTHORIZATION (REST-Centric)

---

## Authentication vs Authorization ⭐

| Authentication | Authorization    |
| -------------- | ---------------- |
| Who are you?   | What can you do? |
| Identity       | Permissions      |
| Login          | Access control   |

💡 **Interview trap**

> AuthN ≠ AuthZ.

---

## Session-based vs Token-based authentication

### **Session-based**

* Server stores session
* Requires sticky sessions
* Hard to scale

---

### **Token-based**

* Client sends token each request
* Server is stateless
* Easy horizontal scaling

💡 **Senior stance**

> Stateless systems scale; stateful systems coordinate.

---

## Why REST APIs prefer **stateless auth**?

Because:

* No server memory per client
* Easier load balancing
* Better fault tolerance
* Simpler scaling

💡 **Interview one-liner**

> Stateless auth aligns with REST’s stateless constraint.

---

## JWT vs Opaque tokens

### **JWT**

* Self-contained
* Signed
* Readable claims
* No DB lookup needed

⚠️ Downsides:

* Hard to revoke
* Larger size

---

### **Opaque tokens**

* Random string
* Requires lookup
* Easy to revoke
* Centralized control

💡 **Senior insight**

> JWT favors performance; opaque tokens favor control.

---

## Where should the token be sent?

### **Best practice**

```
Authorization: Bearer <token>
```

Avoid:

* Query parameters (leakage)
* Cookies (unless browser-based with CSRF protection)

---

## What is **OAuth2** (high level)?

OAuth2 is:

> **A delegation framework**, not authentication.

It allows:

* Apps to access resources
* On behalf of users
* Without sharing passwords

Actors:

* Resource Owner
* Client
* Authorization Server
* Resource Server

💡 **Interview clarity**

> OAuth2 answers “can this app access this resource?”

---

## Access token vs Refresh token

### **Access Token**

* Short-lived
* Sent with every request
* Used for authorization

### **Refresh Token**

* Long-lived
* Used to get new access tokens
* Stored securely

💡 **Senior rule**

> Short-lived access tokens limit blast radius.

---

## Token expiration handling

### **Flow**

* Access token expires
* Client uses refresh token
* New access token issued
* Old token becomes invalid

💡 **Interview insight**

> Expiration is security’s safety net.

---

## How do you **revoke tokens**?

### **JWT**

* Token blacklist
* Short expiration
* Rotate signing keys

### **Opaque tokens**

* Delete from store
* Immediate revocation

💡 **Senior insight**

> Revocation is why many banks prefer opaque tokens.

---

## How do you **secure REST APIs**?

### **Core checklist**

* HTTPS everywhere
* Strong authentication
* Authorization checks per endpoint
* Input validation
* Rate limiting
* Idempotency for critical writes
* Proper status codes
* Audit logging

💡 **Interview gold**

> Security is layered, not a single feature.

---

## ✅ Final Interview-Ready Summary

### **Idempotency**

* Idempotency = safe retries
* Critical for POST in payments/orders
* Implemented via Idempotency-Key
* Retries without idempotency cause data corruption
* APIs prevent duplicates; consumers tolerate them

### **Authentication & Authorization**

* AuthN = identity, AuthZ = permission
* REST prefers stateless auth
* Tokens scale better than sessions
* JWT vs opaque is performance vs control
* OAuth2 is delegation, not login
* Access tokens expire; refresh tokens renew
* Revocation strategy depends on token type
* API security is multi-layered

---

### 🔥 One-line senior takeaway

> **Retries are inevitable, attackers are persistent, and systems fail — idempotency and stateless security are what keep APIs correct and scalable.**

Below is a **senior-level, interview-grade explanation** of **Performance, Scalability, Resilience, and API Evolution in REST systems**. This is exactly how strong backend engineers reason in **bank / FAANG / high-traffic environments**.

---

# 8️⃣ PERFORMANCE & SCALABILITY (Senior-Level)

---

## How do you **cache REST APIs**? ⭐

### **Caching layers**

1. **Client-side** (browser / mobile)
2. **CDN / reverse proxy** (CloudFront, Akamai)
3. **Server-side** (in-memory / Redis)
4. **Database-level**

REST caching relies heavily on **HTTP semantics**, not custom logic.

💡 **Interview one-liner**

> Good REST APIs are cache-friendly by design.

---

## HTTP caching headers (ETag, Cache-Control)

### **Cache-Control**

Controls **who caches and for how long**.

Examples:

```
Cache-Control: public, max-age=60
Cache-Control: no-store
```

---

### **ETag**

* A hash or version of the resource representation
* Changes when resource changes

---

## Conditional requests (If-None-Match)

### **Flow**

1. Client sends `If-None-Match: <etag>`
2. Server compares with current ETag
3. If unchanged → `304 Not Modified`
4. No response body sent

💡 **Interview clarity**

> Conditional requests save bandwidth, not server work.

---

## Why **ETag improves performance**?

* Avoids sending large payloads
* Reduces network usage
* Faster client response
* Works well with CDNs

💡 **Senior insight**

> ETag optimizes *data transfer*, not computation.

---

## Gzip compression — pros & cons

### **Pros**

* Smaller payloads
* Faster network transfer
* Lower bandwidth cost

### **Cons**

* CPU overhead
* Not ideal for small responses
* Already-compressed data gains little

💡 **Senior rule**

> Compress large, text-heavy responses—not everything.

---

## Rate limiting strategies ⭐

### **Common strategies**

1. **Fixed window**
2. **Sliding window**
3. **Token bucket**
4. **Leaky bucket**

Most production systems use **token bucket**.

💡 **Interview insight**

> Rate limiting protects *capacity*; throttling protects *fairness*.

---

## Throttling vs Rate limiting

| Rate limiting   | Throttling     |
| --------------- | -------------- |
| Hard limit      | Soft control   |
| Reject excess   | Slow down      |
| Protects system | Protects users |

---

## How do you **protect APIs from abuse**?

* Rate limiting
* Authentication
* IP whitelisting
* CAPTCHA (public APIs)
* Payload size limits
* Request validation
* WAF
* Monitoring & alerts

💡 **Senior insight**

> Abuse protection is defense-in-depth.

---

## API Gateway responsibilities

### **Typical responsibilities**

* Authentication & authorization
* Rate limiting
* Request routing
* SSL termination
* Logging & metrics
* API version routing
* Circuit breaking

💡 **Interview clarity**

> Gateways centralize cross-cutting concerns.

---

## When should an API be **asynchronous**?

Use async APIs when:

* Processing takes long
* Request may timeout
* High latency downstream
* Partial failures possible

Pattern:

```
POST → 202 Accepted → status endpoint
```

💡 **Senior rule**

> Synchronous APIs are for fast paths only.

---

# 9️⃣ ERROR HANDLING & RESILIENCE

---

## How do you handle **errors in REST APIs**?

### **Principles**

* Fail fast
* Fail clearly
* Fail consistently

Use:

* Proper HTTP status codes
* Structured error bodies
* Correlation IDs

---

## Global error handling best practices

* Centralized exception handling
* Standard error schema
* Mask internal details
* Log full context internally

💡 **Interview gold**

> Clients need clarity; servers need diagnostics.

---

## Client error vs Server error

| 4xx                   | 5xx            |
| --------------------- | -------------- |
| Client mistake        | Server failure |
| Don’t retry (usually) | Retry possible |

---

## Retrying failed requests — when safe?

### **Safe retries**

* Idempotent methods
* Transient failures (timeouts, 503)

### **Unsafe retries**

* Non-idempotent POSTs
* Without idempotency keys

💡 **Senior insight**

> Retry only when state guarantees allow it.

---

## Circuit breaker in REST APIs

### **Concept**

Stops calling a failing service to:

* Prevent cascading failures
* Allow recovery

States:

* Closed
* Open
* Half-open

💡 **Interview clarity**

> Circuit breakers protect systems, not services.

---

## Timeout handling

### **Best practices**

* Short client timeouts
* Server-side timeouts
* No infinite waits
* Fail fast

💡 **Senior rule**

> Timeouts define system boundaries.

---

## Partial failure handling

### **Scenarios**

* Bulk operations
* Fan-out calls

### **Approach**

* Return partial success
* Include per-item status
* Avoid all-or-nothing

💡 **Interview insight**

> Partial failure is normal in distributed systems.

---

## Designing APIs for **resiliency**

* Statelessness
* Idempotency
* Timeouts
* Retries
* Circuit breakers
* Graceful degradation
* Bulkheads

💡 **Senior framing**

> Resilience is a design property, not a library feature.

---

# 🔟 VERSIONING & EVOLUTION (Very Common)

---

## API versioning strategies ⭐

### **1️⃣ URL versioning**

```
/api/v1/users
```

**Pros**

* Simple
* Visible

**Cons**

* URL churn

---

### **2️⃣ Header versioning**

```
Accept-Version: v1
```

**Pros**

* Clean URLs

**Cons**

* Harder to debug

---

### **3️⃣ Media-type versioning**

```
Accept: application/vnd.app.v1+json
```

**Pros**

* REST-pure

**Cons**

* Complex
* Rarely adopted

💡 **Industry default**

> URL versioning.

---

## When should you **version an API**?

* Breaking response changes
* Removing fields
* Changing semantics
* Contract incompatibility

---

## How do you **deprecate APIs**?

* Mark deprecated in docs
* Add warning headers
* Provide migration path
* Set sunset dates
* Monitor usage

💡 **Senior insight**

> Deprecation is communication, not deletion.

---

## Breaking vs non-breaking changes

### **Non-breaking**

* Add optional fields
* Add new endpoints

### **Breaking**

* Remove fields
* Change field types
* Change meaning

---

## Schema evolution strategies

* Backward-compatible changes
* Additive changes
* Feature flags
* Versioned responses

---

## How do you **notify consumers of changes**?

* Release notes
* Email / Slack
* API dashboards
* Deprecation headers
* Contract tests

💡 **Interview clarity**

> APIs are products—communication matters.

---

## ✅ Final Interview-Ready Summary

### **Performance**

* Use HTTP caching correctly
* ETags reduce bandwidth
* Compress wisely
* Rate limit aggressively
* Gateways centralize concerns
* Async APIs prevent timeouts

### **Resilience**

* Standardized error handling
* Retry only idempotent operations
* Circuit breakers stop cascades
* Timeouts define boundaries
* Partial failures must be expected

### **Evolution**

* Version only when needed
* URL versioning is pragmatic
* Deprecate, don’t break silently
* Additive changes preserve compatibility
* Communication is part of API design

---

### 🔥 One-line senior takeaway

> **Scalable APIs are cache-friendly, resilient to failure, and designed to evolve without breaking consumers.**

Below is a **senior-level, interview-grade deep dive** into **REST security, real-world failure scenarios, and commonly missed conceptual traps**. This is exactly the level expected for **Senior Backend / Bank / FAANG interviews**.

---

# 1️⃣1️⃣ SECURITY (REST-SPECIFIC)

---

## What is **CORS**? ⭐

### **Concept**

CORS (Cross-Origin Resource Sharing) is:

> A **browser security mechanism**, not a server security feature.

It controls:

* Which origins can call your API **from browsers**

💡 **Key insight**

> CORS protects *users*, not APIs.

---

## Preflight request — **why it happens**?

### **When**

Browser sends `OPTIONS` before actual request if:

* Non-simple method (PUT, DELETE)
* Custom headers
* Non-standard content-type

### **Why**

* Browser asks server: *“Is this allowed?”*

💡 **Interview clarity**

> Preflight is browser negotiation, not server logic.

---

## How do you **prevent CSRF** in REST?

### **CSRF occurs when**

* Browser automatically sends cookies
* Attacker tricks user into making requests

### **Prevention**

* Use **stateless token-based auth**
* Avoid cookies for APIs
* Use CSRF tokens if cookies are required
* SameSite cookies

💡 **Senior insight**

> CSRF is a browser problem, not a REST problem.

---

## Why CSRF is **less common in token-based auth**?

Because:

* Tokens are stored client-side
* Browser doesn’t auto-send Authorization headers
* Attacker cannot inject headers cross-site

💡 **Interview gold**

> CSRF relies on ambient authority; tokens remove it.

---

## How do you **prevent SQL injection**?

### **Core defenses**

* Prepared statements
* ORM parameter binding
* Input validation
* Least-privilege DB access

💡 **Interview clarity**

> SQL injection is a coding bug, not a database flaw.

---

## How do you **prevent XSS in APIs**?

### **API-side**

* Return JSON, not HTML
* Set correct `Content-Type`
* Encode output when needed

### **Client-side**

* Escape user input
* Avoid eval-like behavior

💡 **Senior insight**

> APIs don’t execute JavaScript—clients do.

---

## How do you **validate input payloads**?

### **Best practices**

* Schema validation (JSON Schema)
* Field-level validation
* Reject unknown fields
* Length and range checks

💡 **Interview one-liner**

> Validation is a security boundary.

---

# 1️⃣2️⃣ REAL SCENARIO-BASED QUESTIONS (VERY IMPORTANT)

---

## 🟥 API is slow — how do you debug?

### **Step-by-step**

1. Measure latency (client vs server)
2. Check DB queries
3. Check external dependencies
4. Look at caching
5. Analyze payload size
6. Profile code paths

💡 **Senior rule**

> Never optimize without metrics.

---

## 🟨 Clients sending **duplicate requests** — solution?

### **Root causes**

* Network retries
* Mobile clients
* Timeouts

### **Fix**

* Idempotency keys
* Deduplication logic
* Proper retries
* Client SDK guidance

💡 **Interview insight**

> Duplicate requests are normal in distributed systems.

---

## 🟥 API backward compatibility broke — what went wrong?

### **Common mistakes**

* Removed fields
* Changed semantics
* Tight client coupling
* No versioning

💡 **Senior insight**

> Breaking clients is a contract violation.

---

## 🟥 API returning **500 intermittently** — approach?

### **Diagnosis**

* Correlate logs
* Check downstream services
* Look for timeouts
* Analyze load patterns
* Identify race conditions

💡 **Interview clarity**

> Intermittent failures are usually dependency-related.

---

## 🟨 Pagination causes **missing records** — why?

### **Root cause**

* Offset pagination + concurrent writes

### **Fix**

* Cursor-based pagination
* Stable sorting keys

💡 **Interview gold**

> Offset pagination is unsafe on mutable datasets.

---

## 🟥 How do you design API for **large file upload**?

### **Best approaches**

* Chunked uploads
* Pre-signed URLs
* Async processing
* Progress tracking

💡 **Senior insight**

> APIs should coordinate uploads, not carry them.

---

## 🟥 API timeout issues — how do you handle?

* Reduce processing time
* Async APIs (202 Accepted)
* Increase timeouts carefully
* Optimize downstream calls

💡 **Interview clarity**

> Timeouts define system limits.

---

## 🟨 How do you design APIs for **mobile clients**?

### **Principles**

* Minimize payload size
* Cache aggressively
* Use pagination
* Support retries
* Handle flaky networks

💡 **Senior insight**

> Mobile APIs must assume unreliable networks.

---

## REST vs **GraphQL** — when to choose REST?

### **REST**

* Simpler
* Cache-friendly
* Stable contracts

### **GraphQL**

* Flexible queries
* Avoid over-fetching
* Complex server logic

💡 **Interview stance**

> REST for stability, GraphQL for flexibility.

---

## ⭐ Explain a REST API you designed **end-to-end**

### **Strong answer structure**

1. Business problem
2. Resource modeling
3. HTTP semantics
4. Security
5. Performance
6. Versioning
7. Failure handling

---

# 🧠 COMMONLY MISSED BUT ASKED

---

## Difference between **REST and HTTP**

| REST                | HTTP                |
| ------------------- | ------------------- |
| Architectural style | Protocol            |
| Design principles   | Transport mechanism |
| Uses HTTP           | Independent of HTTP |

💡 **Interview one-liner**

> REST uses HTTP; HTTP doesn’t require REST.

---

## Can REST be **stateful**?

### **Technically yes, architecturally no**

Stateful APIs:

* Violate REST constraints
* Don’t scale well

💡 **Senior stance**

> Stateful REST is a contradiction.

---

## Why REST prefers **resources over actions**?

Because:

* Resources are stable
* Actions change over time
* HTTP methods already express actions

---

## Hypermedia-driven APIs — real usage?

### **Reality**

* Rare in practice
* Common in internal APIs
* Shows maturity

💡 **Interview clarity**

> HATEOAS is idealistic, not mandatory.

---

## API documentation best practices (OpenAPI)

* OpenAPI / Swagger
* Examples
* Error codes
* Versioning
* Changelogs

💡 **Senior insight**

> APIs without docs don’t exist.

---

## Backward compatibility pitfalls

* Enum changes
* Field removal
* Meaning changes
* Validation tightening

---

## API contract testing — why needed?

Because:

* Prevent breaking changes
* Catch regressions early
* Protect consumers

💡 **Interview gold**

> Contracts are promises; tests enforce them.

---

## ✅ Final Interview-Ready Summary

### **Security**

* CORS protects browsers
* CSRF mitigated via stateless auth
* SQLi prevented via parameterization
* XSS is a client problem
* Validation is mandatory

### **Scenarios**

* Slow APIs need metrics
* Duplicates require idempotency
* Pagination must be stable
* Large uploads should be offloaded
* Mobile APIs must assume failure

### **Concepts**

* REST ≠ HTTP
* REST prefers resources
* Statelessness enables scale
* GraphQL trades complexity for flexibility
* API contracts must be tested

---

### 🔥 One-line senior takeaway

> **REST APIs are contracts — security, performance, and evolution are not optional features.**

Got it 👍
Below is a **REST-focused interview question set** for **Senior Backend / Java developers (≈4 years)**, **purely about REST APIs**, **not Spring Boot internals**.
This is exactly what interviewers ask when they say *“Let’s talk about REST API design”*.

No overlap with Spring Boot theory.

---

# 🔥 REST API INTERVIEW QUESTIONS (FOCUS: DESIGN, HTTP, BEHAVIOR)

---

## 1️⃣ REST Fundamentals (Must Know)

1. What is REST?
2. REST vs SOAP ⭐
3. What does RESTful mean?
4. Is REST a protocol or an architectural style?
5. What are the core REST constraints? ⭐
6. What is statelessness in REST?
7. Why statelessness is important?
8. What is resource in REST?
9. Difference between resource and representation
10. What is HATEOAS?

---

## 2️⃣ HTTP METHODS (Very Common)

11. Difference between GET, POST, PUT, PATCH, DELETE ⭐
12. PUT vs PATCH — when to use which?
13. Can GET have a request body?
14. Is DELETE idempotent?
15. Which HTTP methods are idempotent? ⭐
16. Safe vs unsafe HTTP methods
17. When should POST be idempotent?
18. Why POST is not idempotent?
19. What happens if PUT is called twice?
20. Can POST be cached?

---

## 3️⃣ HTTP STATUS CODES (Interview Favorite)

21. Difference between 200, 201, 204 ⭐
22. When should you return 201?
23. 400 vs 401 vs 403 ⭐
24. 404 vs 410
25. 409 Conflict — real use cases ⭐
26. 422 Unprocessable Entity — when to use?
27. 500 vs 502 vs 503
28. When to return 202 Accepted?
29. Why not always return 200?

---

## 4️⃣ REST API DESIGN & URI NAMING (High Signal)

30. RESTful URI naming best practices ⭐
31. Plural vs singular resources
32. Verbs vs nouns in URLs
33. Nested resources — when to use?
34. Deep nesting problems
35. How to model relationships in REST?
36. Filtering vs searching vs sorting
37. Pagination — offset vs cursor ⭐
38. How do you design bulk APIs?
39. How do you design partial updates?
40. How do you expose actions in REST?

---

## 5️⃣ REQUEST / RESPONSE DESIGN

41. What should go in headers vs body?
42. What is Content-Type?
43. What is Accept header?
44. Content negotiation ⭐
45. JSON vs XML — tradeoffs
46. How do you design consistent API responses?
47. Error response design best practices ⭐
48. Should error messages be human-readable?
49. How do you version API responses?
50. How do you handle backward compatibility?

---

## 6️⃣ IDENTITY, IDEMPOTENCY & SAFETY

51. What is idempotency? ⭐
52. Why idempotency matters in REST?
53. How do you implement idempotency in POST?
54. Idempotency-Key header — how it works
55. How do retries affect APIs?
56. Duplicate request handling
57. How do you prevent double submissions?
58. Idempotent consumer vs idempotent API

---

## 7️⃣ AUTHENTICATION & AUTHORIZATION (REST-Centric)

59. Authentication vs authorization ⭐
60. Session-based vs token-based auth
61. Why REST APIs prefer stateless auth?
62. JWT vs opaque tokens
63. Where should token be sent?
64. What is OAuth2 (high level)?
65. Access token vs refresh token
66. Token expiration handling
67. How do you revoke tokens?
68. How do you secure REST APIs?

---

## 8️⃣ PERFORMANCE & SCALABILITY (Senior-Level)

69. How do you cache REST APIs? ⭐
70. HTTP caching headers (ETag, Cache-Control)
71. Conditional requests (If-None-Match)
72. Why ETag improves performance?
73. Gzip compression — pros & cons
74. Rate limiting strategies ⭐
75. Throttling vs rate limiting
76. How do you protect APIs from abuse?
77. API gateway responsibilities
78. When should API be asynchronous?

---

## 9️⃣ ERROR HANDLING & RESILIENCE

79. How do you handle errors in REST APIs?
80. Global error handling best practices
81. Client error vs server error
82. Retrying failed requests — when safe?
83. Circuit breaker in REST APIs
84. Timeout handling
85. Partial failure handling
86. Designing APIs for resiliency

---

## 🔟 VERSIONING & EVOLUTION (Very Common)

87. API versioning strategies ⭐

* URL
* Header
* Media type

88. Pros & cons of each versioning approach
89. When should you version an API?
90. How do you deprecate APIs?
91. Breaking vs non-breaking changes
92. Schema evolution strategies
93. How do you notify consumers of changes?

---

## 1️⃣1️⃣ SECURITY (REST-SPECIFIC)

94. What is CORS? ⭐
95. Preflight request — why it happens?
96. How do you prevent CSRF in REST?
97. Why CSRF is less common in token-based auth?
98. How do you prevent SQL injection?
99. How do you prevent XSS in APIs?
100. How do you validate input payloads?

---

## 1️⃣2️⃣ REAL SCENARIO-BASED QUESTIONS (VERY IMPORTANT)

101. API is slow — how do you debug?
102. Clients sending duplicate requests — solution?
103. API backward compatibility broke — what went wrong?
104. API returning 500 intermittently — approach?
105. Pagination causes missing records — why?
106. How do you design API for large file upload?
107. API timeout issues — how do you handle?
108. How do you design APIs for mobile clients?
109. REST vs GraphQL — when to choose REST?
110. Explain a REST API you designed end-to-end ⭐

---

## 🧠 COMMONLY MISSED BUT ASKED

111. Difference between REST and HTTP
112. Can REST be stateful?
113. Why REST prefers resources over actions
114. Hypermedia-driven APIs — real usage?
115. API documentation best practices (OpenAPI)
116. Backward compatibility pitfalls
117. API contract testing — why needed

---

