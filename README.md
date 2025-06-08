<div align="center">
  <h1>System Design Questions</h1>
</div>

<div align="center">
Test your system design skills.

Answers are provided in collapsible sections below each question — just click to expand. Have fun, and good luck!

Any comments and suggestions are more than welcome.
</div>

---

<p><div align="center">This repo is created similary to Lydia Hallie famous repo
  <a href="https://github.com/lydiahallie/javascript-questions">javascript-questions</a>.</div><div align="center">Credits to her for the great idea and hard work! :heart: 
</div>

</p>


---

1. You are designing a globally distributed messaging platform (like WhatsApp or Slack) that ensures message delivery guarantees, low latency, and eventual consistency across regions. What is the most appropriate architecture approach to ensure scalability, fault tolerance, and minimal message loss in the face of regional outages?

- A. Use a centralized message broker in a primary region with failover replicas in other regions and synchronous replication.
- B. Use a distributed message queue (e.g., Kafka with geo-replication) combined with local storage and asynchronous replication between regions.
- C. Deploy a monolithic message server with high availability in a single data center, using CDN for read optimization.
- D. Rely on client-side message caching and retries only, without any backend persistence to reduce infrastructure complexity.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: B
The correct answer is B because it provides a balance between **low latency**, **fault tolerance**, and **scalability**. By using a distributed message queue like **Kafka with geo-replication**, each region can handle **local writes** quickly while asynchronously syncing messages across other regions. This ensures **eventual consistency** and avoids service disruption during **regional outages**. Unlike centralized or monolithic approaches, it **scales horizontally** and supports **global usage** efficiently.


</p>
</details>



---

2. You're designing a multi-tenant SaaS platform used by thousands of enterprise customers, each with their own data and usage patterns. You need to ensure data isolation, efficient resource usage, and easy scaling as the customer base grows.
What is the most scalable and maintainable architecture approach?

- A. Deploy a dedicated application and database instance for each tenant (one stack per tenant).
- B. Use a shared application and a single shared database with tenant data identified by a tenant ID.
- C. Use a shared application with a pooled database model, partitioned by tenant ID, and apply row-level access control.
- D. Use a serverless architecture where each tenant triggers isolated functions that access a common database without tenant-specific logic.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: C
**The correct answer is C** because it offers the best balance between **scalability**, **cost-efficiency**, and **data isolation**. A shared application with a **pooled database model** (also known as multi-tenant database with shared schema) allows you to support many tenants without duplicating infrastructure, while **row-level access control** ensures each tenant’s data is kept secure. It’s easier to manage than fully isolated deployments (A) and more scalable than a single shared database without proper partitioning (B). **Serverless without tenant-aware logic (D)** risks data leaks and lacks fine-grained control.


</p>
</details>


---

3. You’re building a high-traffic e-commerce website that needs to handle millions of product views per day and support real-time inventory updates. Which approach best balances read performance, data consistency, and system scalability?

- A. Use a single relational database for both reads and writes, scaling vertically.
- B. Use a relational database for writes and a distributed caching layer (e.g., Redis or Memcached) for reads with cache invalidation on updates.
- C. Use a NoSQL database with eventual consistency for both reads and writes to maximize scalability.
- D. Use multiple relational databases sharded by product ID without caching.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: B
**Option B is best** because it uses a relational database for **accurate writes** and a distributed cache for **fast reads**, with cache invalidation ensuring data freshness.  
Option A relies on **vertical scaling**, which limits performance at high traffic.  
Option C sacrifices **consistency for scalability**, risking stale inventory data, while option D shards the database but lacks caching, leading to potential **read bottlenecks**.

</p>
</details>


---

4. You need to design a real-time collaborative document editing platform (like Google Docs) that supports concurrent edits from thousands of users worldwide with low latency and strong consistency. Which approach best handles concurrent edits and ensures conflict resolution without data loss?

- A. Use a centralized locking mechanism where only one user can edit a document section at a time.
- B. Use Operational Transformation (OT) algorithms to merge concurrent edits in real time across clients.
- C. Use a last-write-wins (LWW) strategy where the most recent update overwrites previous ones.
- D. Use periodic snapshots with offline editing and manual merge resolution on conflict.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: B
**Option B is best** because **Operational Transformation (OT)** allows multiple users to edit a document simultaneously by transforming and merging their changes in real time, preserving the intent of each edit and ensuring strong consistency without data loss. Other options either limit concurrency (A), risk overwriting changes (C), or delay collaboration with manual merges (D).

---

### What is **Operational Transformation (OT)**?  
OT is an algorithmic technique that **transforms concurrent edits** from different users so they can be applied in a consistent order on all clients, resolving conflicts automatically and enabling smooth, real-time collaboration.


---

### How OT works (high-level):

1. Each client sends an operation (e.g., insert character at position 5).
2. The server receives concurrent operations from multiple clients.
3. The server transforms these operations relative to each other to maintain consistency.
4. The transformed operations are broadcast back to all clients.
5. Each client applies the operations in a way that preserves the users’ intentions.

---

### Example Scenario:

- User A inserts "X" at position 5.
- User B deletes a character at position 3.
- Both operations arrive at the server nearly simultaneously.

Without OT, applying these operations in different orders on clients can lead to inconsistent documents. OT transforms the operations so that each client applies them correctly, ensuring the final documents are identical.

</p>
</details>

---

5. You are tasked with designing a global distributed database system that supports strong consistency, high availability, and partition tolerance across multiple data centers. Which of the following strategies best achieves this balance?

- A. Use a leader-follower replication model with synchronous replication across all data centers.
- B. Use a consensus protocol like Paxos or Raft to replicate data synchronously among data centers.
- C. Use asynchronous replication with eventual consistency and conflict resolution.
- D. Use a fully decentralized gossip protocol without strict coordination.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: B
---

### Summary:  
Option **B** (consensus protocols like Paxos or Raft) is the best choice because it ensures **strong consistency** and **high availability** by requiring agreement from a majority of nodes before committing data, even across multiple data centers. Other options either sacrifice consistency (C, D) or suffer from high latency and availability problems (A).

---

### Examples of databases using consensus protocols:  
- **etcd** (uses Raft)  
- **CockroachDB** (uses Raft)  
- **Google Spanner** (uses a Paxos variant called TrueTime for global consistency)  
- **Apache ZooKeeper** (uses Zab, a protocol similar to Paxos)

</p>
</details>

---

6. You're designing a rate limiter for a high-traffic API that must support distributed environments with multiple application instances. Which of the following approaches best ensures accuracy, scalability, and resilience?

- A. Use an in-memory token bucket algorithm in each API server instance.
- B. Use a central Redis store with the leaky bucket algorithm and atomic operations.
- C. Use a distributed log system like Kafka to queue and throttle requests sequentially.
- D. Use client-side rate limiting with static thresholds and no coordination.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: B 
**"Use a central Redis store with the leaky bucket algorithm and atomic operations."**

---

### **Explanation:**

- **A. In-memory token bucket per instance**  
  - ❌ Fast but not accurate in a distributed environment. Each instance tracks its own limits, leading to inconsistencies.

- **B. Central Redis with leaky bucket + atomic ops**  
  - ✅ Ensures **global rate limiting** by centralizing the limit logic.
  - Redis supports **atomic operations** (`INCR`, `EXPIRE`, `SETNX`, etc.), enabling **precise control** over request flow.
  - Redis can be scaled with clustering and is often used in production for **distributed rate limiting**.

- **C. Distributed log (Kafka)**  
  - ❌ Overkill and not ideal for enforcing rate limits. Introduces latency and complexity without improving accuracy.

- **D. Client-side static limits**  
  - ❌ No central coordination. Clients can bypass or incorrectly implement limits, leading to abuse or overload.

</p>
</details>

---

</p>
</details>
