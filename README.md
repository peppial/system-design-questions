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

7. You are designing a global messaging system that must guarantee message ordering, exactly-once delivery, and low latency across multiple geographic regions. Which approach is most appropriate?

- A. Use a distributed message queue with at-least-once delivery and client-side deduplication.
- B. Use a consensus protocol-based log (like Apache Kafka with exactly-once semantics).
- C. Use a simple pub-sub system without delivery guarantees for higher performance.
- D. Use a relational database to store and query messages synchronously.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: B 
---

### **Explanation:**

- **A. Distributed queue with at-least-once delivery and client deduplication**  
  - ❌ Guarantees delivery but not exactly-once semantics or strict ordering. Client-side deduplication adds complexity and potential errors.

- **B. Consensus protocol-based log (Kafka with exactly-once semantics)**  
  - ✅ Ensures **exactly-once delivery** via distributed transactions and atomic offset management.  
  - Maintains **strict message ordering** within partitions.  
  - Provides **low latency** and strong durability across geographic regions.  
  - Widely used for systems requiring strong guarantees.

- **C. Simple pub-sub without delivery guarantees**  
  - ❌ Sacrifices ordering and delivery guarantees for performance; unsuitable for critical reliability needs.

- **D. Relational database synchronous storage**  
  - ❌ Not optimized for high-throughput messaging; synchronous writes lead to high latency and poor scalability.
</p>
</details>

---

8. You’re designing a high-frequency trading platform where latency must be minimized and data consistency is critical. Which architecture component is most suitable for processing and reacting to incoming trade data in microseconds?

- A. Use a traditional relational database for trade processing and lookups.
- B. Use a distributed message queue with eventual consistency.
- C. Use an in-memory, event-driven stream processor with lock-free data structures.
- D. Use periodic batch jobs to evaluate and respond to trade data.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: B 
**"Use an in-memory, event-driven stream processor with lock-free data structures."**

---

### **Explanation:**

- **A. Traditional relational database**  
  - ❌ Too slow for microsecond-level latency. Disk I/O and transaction overhead make it unsuitable for real-time trading.

- **B. Distributed message queue with eventual consistency**  
  - ❌ Not ideal for strict consistency or ultra-low latency. Messages may arrive out of order, and delays are unacceptable in HFT.

- **C. In-memory, event-driven stream processor with lock-free data structures**  
  - ✅ Offers **extremely low latency** by processing data directly in memory.  
  - **Lock-free data structures** (like ring buffers) avoid contention and allow high-throughput, low-latency operations.  
  - Used in systems like **LMAX Disruptor**, this approach is optimized for high-speed decision making in trading.

- **D. Periodic batch jobs**  
  - ❌ Completely unsuitable. Batch jobs introduce delays and are not reactive.

---

### **What is a Ring Buffer?**

A **ring buffer** (or **circular buffer**) is a fixed-size, circular data structure that stores elements in a contiguous block of memory. When it reaches the end, it wraps around to the beginning, overwriting old data if necessary.

#### Key Properties:
- **Fixed size**: Pre-allocated to ensure predictable memory usage and performance.
- **Two pointers**: A **write pointer** for adding data and a **read pointer** for consuming data.
- **Wrap-around behavior**: When the end of the buffer is reached, it continues from the start, forming a circular structure.

#### Why it’s used in high-performance systems:
- Supports **lock-free concurrent access** for one producer and one consumer, minimizing contention.
- Provides **low latency** and high throughput due to memory efficiency and sequential access.
- Commonly used in **real-time systems** like high-frequency trading platforms, audio processing, and network packet buffers.
</p>
</details>

---

9. You're building a globally distributed application that must ensure strong consistency for reads and writes, even across regions. Which architectural approach is best suited to achieve this requirement?

- A. Use a distributed database with leaderless replication (e.g. Cassandra).
- B. Use eventual consistency with conflict resolution at read time.
- C. Use a distributed database with a single leader and follower replicas.
- D. Use a quorum-based write and read system with tunable consistency.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: C
---
**"Use a distributed database with a single leader and follower replicas."**

---

### **Explanation:**

- **A. Leaderless replication (e.g. Cassandra)**  
  - ❌ Prioritizes availability and partition tolerance (AP from CAP theorem). It uses **eventual consistency**, which does not guarantee **strong consistency** across regions.

- **B. Eventual consistency with read-time conflict resolution**  
  - ❌ Acceptable for some use cases, but **not suitable** when strict ordering and consistency are required across regions.

- **C. Distributed database with a single leader and follower replicas**  
  - ✅ Guarantees **strong consistency** by directing all writes through a **central leader**, ensuring a **single source of truth**.  
  - Follower replicas are kept in sync through replication, and reads can be configured to go to the leader for strong consistency.  
  - Commonly used in systems like **Google Spanner** or **CockroachDB (in certain configurations)**.

- **D. Quorum-based read/write system**  
  - ⚠️ Can be **tuned for stronger consistency**, but requires careful configuration. Without strict enforcement, it may still allow inconsistency under some failure modes.
---

</p>
</details>

---

10. You're designing a log analytics platform that must ingest terabytes of log data per day, support near real-time querying, and allow for high write throughput. What is the most appropriate architecture to support this use case?

- A. Use a traditional relational database with indexing on log fields.
- B. Use a time-series database with write-optimized storage and downsampling.
- C. Use a distributed search engine like Elasticsearch with write buffering and sharding.
- D. Use a key-value store with eventual consistency and background processing for queries.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: C
---

### ✅ **Correct Answer: C**  
**"Use a distributed search engine like Elasticsearch with write buffering and sharding."**

---

### **Explanation:**

- **A. Traditional relational database with indexing**  
  - ❌ Not suitable for high write throughput or large-scale log ingestion. Indexing slows down inserts, and scaling is limited.

- **B. Time-series database with downsampling**  
  - ⚠️ Works well for **metrics** and **time-bound events**, but **less efficient** for complex text-based log search and filtering.

- **C. Distributed search engine (e.g., Elasticsearch)**  
  - ✅ Designed for **high write throughput**, **real-time indexing**, and **powerful querying** (e.g., full-text, filters, aggregations).  
  - Supports **sharding**, **replication**, and **write buffering**, which makes it ideal for log analytics at scale.  
  - Used by systems like the **ELK Stack (Elasticsearch, Logstash, Kibana)**.

- **D. Key-value store with eventual consistency**  
  - ❌ Fast for simple lookups, but not designed for advanced queries or log analytics use cases.
 

</p>
</details>

---

11. You are designing a video streaming platform (like Netflix or YouTube). What is the most effective strategy to ensure low-latency playback and high availability for users across the globe?

- A. Store all video content in a central data center with powerful bandwidth
- B. Use a peer-to-peer (P2P) network to distribute video content among users
- C. Use a global CDN (Content Delivery Network) with caching and edge servers
- D. Stream directly from origin servers using multi-threaded download techniques


<details><summary><b>Answer</b></summary>
<p>

#### Answer: C
**"Use a global CDN (Content Delivery Network) with caching and edge servers."**

---

### **Explanation:**

- **A. Central data center with high bandwidth**  
  - ❌ Centralized architecture introduces **high latency** for users far from the data center and is vulnerable to **single point of failure**.

- **B. Peer-to-peer (P2P) distribution**  
  - ⚠️ Reduces load on central servers but introduces **latency variability**, **security concerns**, and **inconsistent quality**, especially with sparse peers.

- **C. Global CDN with caching and edge servers**  
  - ✅ Best option for **low latency** and **high availability**.  
  - CDNs cache video segments at **edge locations near users**, reducing latency and server load.  
  - Supports **adaptive bitrate streaming** and **failover**, ensuring smooth user experience.

- **D. Streaming directly from origin servers using multi-threading**  
  - ❌ May improve speed but doesn't solve **latency** or **scalability** across global regions.


</p>
</details>


---

12. You are designing a distributed caching system for a high-traffic web application. Which strategy helps to avoid cache stampede (thundering herd problem) when many clients request the same missing key simultaneously?

- A. Use simple cache expiration and let all requests hit the backend when expired
- B. Implement cache locking or "request coalescing" to allow only one request to fetch the data
- C. Increase cache TTL to a very long duration
- D. Use consistent hashing to distribute keys evenly


<details><summary><b>Answer</b></summary>
<p>

---

### ✅ **Correct Answer: B**  
**"Implement cache locking or 'request coalescing' to allow only one request to fetch the data."**

---

### **Explanation:**

- **A. Simple cache expiration with all requests hitting backend**  
  - ❌ Causes a **cache stampede**, where many requests overwhelm the backend simultaneously.

- **B. Cache locking/request coalescing**  
  - ✅ Ensures that when a cached item expires or is missing, **only one request** fetches the data from the backend, while others wait for the cache to be refreshed.  
  - This reduces backend load and prevents spikes in traffic.

- **C. Increase cache TTL to a very long duration**  
  - ⚠️ Reduces cache misses but risks serving **stale data** and reduces cache freshness.

- **D. Use consistent hashing**  
  - ❌ Helps distribute keys evenly across cache nodes but doesn’t directly address cache stampede.

---

</p>
</details>



---

13. You are designing a distributed database system. Which consistency model provides the best balance between availability and performance for a globally distributed system that can tolerate slightly stale reads?

- A. Strong consistency
- B. Eventual consistency
- C. Linearizability
- D. Causal consistency

<details><summary><b>Answer</b></summary>
<p>

---

### ✅ **Correct Answer: D**  
**"Causal consistency"**

---

### **Explanation:**

- **A. Strong consistency**  
  - ❌ Guarantees the latest value for every read but sacrifices **availability** and **latency**, especially in global systems.

- **B. Eventual consistency**  
  - ⚠️ Offers high availability and performance, but allows **out-of-order reads** with no guarantees about operation dependencies.

- **C. Linearizability**  
  - ❌ A strict form of strong consistency that maintains real-time order of operations; **poor performance** in geo-distributed systems.

- **D. Causal consistency**  
  - ✅ Provides a **balance** by preserving the order of **causally related operations** while allowing concurrent, non-conflicting updates to be seen in different orders.  
  - Offers better **performance and availability** than strong models while maintaining logical consistency.

---

</p>
</details>



---

14. You're architecting a distributed caching system for an e-commerce platform that handles 100,000 requests per second during peak traffic. The system needs to maintain cache consistency across multiple data centers, handle cache warming efficiently, and provide sub-millisecond latency. Cache misses should not cause cascading failures. Which design strategy would be most effective?

- A. Implement a two-tier cache architecture with Redis Cluster as L1 cache and Memcached as L2, use write-through caching with synchronous replication across data centers, and implement circuit breakers with exponential backoff for cache miss handling.
- B. Deploy a multi-layer cache hierarchy with browser cache, CDN, application-level cache using Hazelcast, and database query cache. Use cache-aside pattern with eventual consistency, implement bloom filters to reduce unnecessary cache lookups, and use consistent hashing with virtual nodes for distribution.
- C. Create a single global cache using Apache Ignite with persistent storage, implement strong consistency using Raft consensus algorithm, use predictive cache warming based on machine learning models, and employ bulkhead pattern to isolate cache failures.
- D. Use a write-behind caching strategy with Apache Kafka as the message backbone, implement custom cache invalidation using database triggers, rely on client-side caching with HTTP ETags, and use distributed locks with Zookeeper for cache coordination.

<details><summary><b>Answer</b></summary>
<p>

---

### ✅ **Correct Answer: B**  

---

### **Explanation:**

- **Multi-layer hierarchy** provides optimal latency at each level (browser < CDN < application < database)
- **Cache-aside with eventual consistency** balances performance with data freshness for e-commerce scenarios
- **Bloom filters** significantly reduce cache miss penalties by avoiding unnecessary lookups
- **Consistent hashing with virtual nodes** ensures even distribution and handles node failures gracefully
- **Hazelcast** provides excellent performance and built-in distribution capabilities

The other options have critical issues:

- **Option A:** Synchronous replication creates latency bottlenecks  
- **Option C:** Strong consistency conflicts with sub-millisecond requirements  
- **Option D:** Write-behind strategy with distributed locks adds complexity without addressing the core scalability needs

**Hazelcast** is an in-memory data grid platform that provides distributed caching and computing capabilities. Using Hazelcast for application-level caching enables applications to store frequently accessed data in memory across a cluster of nodes, improving performance and scalability.

Key Features

- **Distributed Cache:** Hazelcast distributes cache data across multiple nodes, ensuring high availability and fault tolerance.
- **In-Memory Storage:** Data is stored in RAM, enabling low-latency access compared to disk-based caches.
- **Scalability:** Hazelcast automatically manages data partitioning and replication, allowing seamless scaling by adding or removing nodes.
- **Consistent Hashing:** Ensures even distribution of data and helps in smooth handling of node failures.
- **Near Cache:** Local cache on each application node for ultra-fast reads, reducing network calls.
- **Data Structures Support:** Supports maps, queues, sets, and more, enabling flexible caching strategies.
- **Integration:** Easy to integrate with Java applications and frameworks like Spring.


---

</p>
</details>

