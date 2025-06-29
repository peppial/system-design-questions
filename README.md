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



---

15. Design a global content delivery and caching system like CloudFlare or AWS CloudFront
You need to design a CDN that serves static and dynamic content globally with high performance and reliability. The system should handle:

- Billions of requests per day across 200+ edge locations
- Smart routing to optimal servers based on latency and load
- Multi-layered caching (browser, edge, origin)
- Real-time cache invalidation across all edge nodes
- DDoS protection and traffic anomaly detection
- Support for both static assets and dynamic API responses

Which cache consistency and invalidation strategy would you implement?
- A. Event-Driven Cache Invalidation - Use message queues (Kafka/SQS) to broadcast invalidation events to all edge nodes when content changes, with eventual consistency.
- B. Time-Based TTL with Conditional Requests - Set TTL on cached content and use ETags/Last-Modified headers for conditional requests to origin, allowing stale-while-revalidate.
- C. Hierarchical Cache Purging - Implement a tree-like purge propagation system where regional clusters coordinate invalidation before pushing to edge nodes.
- D. Versioned Content with Immutable Caching - Version all content URLs and cache immutably, requiring application-level coordination for content updates but eliminating cache invalidation complexity.

<details><summary><b>Answer</b></summary>
<p>

---

### ✅ **Correct Answer: B**  

---
#### 1. **Balanced Performance vs Consistency**
- Provides excellent cache hit rates while maintaining reasonable consistency
- Allows serving stale content during origin failures (high availability)
- Conditional requests minimize bandwidth usage when content hasn't changed

#### 2. **Proven at Scale**
- Used successfully by major CDNs (CloudFlare, AWS CloudFront, Fastly)
- HTTP standards-based approach (ETags, Last-Modified, Cache-Control)
- Well-understood by developers and operations teams

#### 3. **Network Efficiency**
- Conditional requests (304 Not Modified) save significant bandwidth
- Stale-while-revalidate allows serving cached content while updating in background
- Reduces load on origin servers compared to always-fresh approaches

#### 4. **Operational Simplicity**
- No complex message queue infrastructure required
- Self-healing - caches naturally refresh based on TTL
- Easy to debug and monitor using standard HTTP tools

#### Key Components:
- **Multi-layer TTL**: Different TTL values at each cache layer
- **ETag/Last-Modified**: For efficient conditional requests
- **Stale-while-revalidate**: Serve stale content during updates
- **Cache-Control headers**: Fine-grained control over caching behavior

### Why Other Options Fall Short

#### Option A (Event-Driven) Issues:
- Complex infrastructure (message queues, event ordering)
- Network partitions can cause inconsistency
- Difficult to handle millions of invalidation events
- Single point of failure in messaging system

#### Option C (Hierarchical Purging) Issues:
- Adds latency to invalidation process
- Complex tree topology management
- Still requires reliable messaging between layers
- Doesn't solve the fundamental scale problem

#### Option D (Versioned/Immutable) Issues:
- Requires application changes for all content
- Complex URL management and routing
- Difficult to implement for dynamic content
- High storage costs for maintaining versions

---

</p>
</details>



---

15. Design a distributed search engine like Elasticsearch or Google Search
You need to design a search engine that can index and search across billions of documents with sub-second query response times. The system should handle:

- Real-time document indexing and updates
- Complex queries with filters, sorting, and aggregations
- Auto-complete and typo tolerance
- Distributed storage across multiple data centers
- High availability with no single point of failure
- Support for both structured and unstructured data

Which indexing and query distribution strategy would you implement?
- A. Master-Slave Replication with Round-Robin Query Distribution - Single master node handles all writes and updates indices, multiple read replicas serve queries with simple round-robin load balancing.
- B. Consistent Hashing with Shard-Based Partitioning - Partition documents across multiple shards using consistent hashing, replicate each shard across multiple nodes, and distribute queries to all relevant shards.
- C. Geographic Partitioning with Regional Masters - Partition data geographically with regional master nodes, route queries to nearest region first, with cross-region fallback for comprehensive results.
- D. Lambda Architecture with Batch + Stream Processing - Separate batch layer for bulk indexing historical data and speed layer for real-time updates, merge results at query time using a serving layer.
<details><summary><b>Answer</b></summary>
<p>

---

### ✅ **Correct Answer: B** - Consistent Hashing with Shard-Based Partitioning

### Why This is the Best Choice

#### 1. **Horizontal Scalability**
- Documents distributed evenly across shards using consistent hashing
- Easy to add/remove nodes without full rebalancing
- Query load distributed across all shards in parallel
- Linear scaling of both storage and compute capacity

#### 2. **Fault Tolerance & High Availability**
- Each shard replicated across multiple nodes (typically 2-3 replicas)
- No single point of failure - system continues operating if nodes fail
- Automatic failover to replica shards when primary fails
- Data locality preserved during failures

#### 3. **Query Performance**
- Parallel query execution across all relevant shards
- Results merged and ranked at coordinator node
- Sub-second response times achievable through parallelization
- Efficient for both simple term queries and complex aggregations

#### 4. **Real-World Proven**
- Used by Elasticsearch, Solr, and other production search systems
- Well-understood operational patterns and troubleshooting
- Mature tooling for monitoring and management

#### Key Components:

**Shard Distribution:**
- Use document ID hash % num_shards for even distribution
- Consistent hashing ring for dynamic shard management
- Virtual nodes to handle heterogeneous hardware

**Replication Strategy:**
- Primary-replica model per shard
- Async replication for indexing performance
- Read from any replica for query load balancing

**Query Execution:**
1. Parse and optimize query at coordinator
2. Scatter sub-queries to all relevant shards
3. Execute in parallel across shard replicas
4. Gather partial results and merge/rank globally
5. Return top-K results to client

### Why Other Options Fall Short

#### Option A (Master-Slave) Issues:
- **Single point of failure**: Master node bottleneck
- **Limited write scalability**: All updates through one node
- **Poor query distribution**: Round-robin doesn't consider data locality
- **Replication lag**: Consistency issues between master and slaves

#### Option C (Geographic Partitioning) Issues:
- **Incomplete results**: Queries may miss relevant documents in other regions
- **Complex routing logic**: Determining which regions to query
- **Network latency**: Cross-region queries for comprehensive results
- **Uneven load**: Some regions may have much more data

#### Option D (Lambda Architecture) Issues:
- **Complexity**: Managing three separate systems (batch, speed, serving)
- **Consistency challenges**: Merging batch and real-time results
- **Operational overhead**: Multiple technologies and data pipelines
- **Query latency**: Additional merge step at serving layer

---

</p>
</details>

---

16. Design a real-time multiplayer game backend (like Fortnite or PUBG)
You need to design a system that handles 100 million concurrent players across thousands of game matches. The system must provide:

Ultra-low latency (sub-50ms) for game state updates
Cheat detection and prevention
Player matchmaking and lobby management
Real-time voice chat and messaging
Game state persistence and replay storage
Global leaderboards and statistics

Which architecture would you choose for handling real-time game state synchronization?
- A. Authoritative Server with Client Prediction - Central game server maintains authoritative state, clients predict movements locally and reconcile with server updates using rollback/replay mechanisms.
- B. Peer-to-Peer with Elected Host - Players connect directly to each other, one player acts as authoritative host, with automatic host migration when the current host leaves.
- C. Lockstep Deterministic Simulation - All clients run identical game simulation, synchronized input commands are broadcast to all players, ensuring identical game state across all clients.
- D. Hybrid Regional Servers with Edge Computing - Authoritative servers in each region with edge nodes for input collection, using predictive algorithms to compensate for network latency.
- 
<details><summary><b>Answer</b></summary>
<p>

---

### ✅ **Correct Answer: A** - (Authoritative Server with Client Prediction)
This is the industry standard because it provides the best balance of security, performance, and scalability. The server maintains the authoritative game state preventing cheating, while client prediction ensures responsive gameplay despite network latency. When conflicts occur, the client rolls back to the server state and replays actions. This approach is used by most successful competitive multiplayer games like CS:GO, Valorant, and Overwatch because it's cheat-resistant and handles network issues gracefully while maintaining fair gameplay for all players.

Option B (P2P): Host can cheat easily, no central validation. Host disconnection breaks the game.
Option C (Lockstep): One laggy player slows down everyone. Perfect determinism is nearly impossible to achieve.
Option D (Hybrid Edge): Massive complexity and cost for minimal benefit over proven client-prediction methods.


---

</p>
</details>

---

17. Design a distributed financial trading system like NYSE or NASDAQ
You need to design a high-frequency trading platform that processes millions of orders per second with strict regulatory compliance. The system must handle:

Sub-microsecond order matching and execution, ACID transactions for all financial operations, Real-time market data feeds to thousands of clients, Audit trails and regulatory reporting, Risk management and circuit breakers, Multi-market arbitrage opportunities

Which order matching and execution architecture would you implement?
- A) Single-Threaded Event Loop with Memory-Mapped Files - Process all orders sequentially in a single thread to avoid locking overhead, use memory-mapped files for ultra-fast persistence and recovery.
- B) Lock-Free Multi-Threading with Ring Buffers - Multiple threads processing different instrument types concurrently, using lock-free data structures and ring buffers for inter-thread communication.
- C) Actor Model with Message Passing - Each trading instrument handled by separate actors, all communication through immutable messages, with supervision trees for fault tolerance.
- D) Blockchain-Based Distributed Ledger - All trades recorded on a private blockchain for immutability and transparency, with consensus mechanisms ensuring all nodes agree on order execution.

<details><summary><b>Answer</b></summary>
<p>

---

### ✅ **Correct Answer: A** -  (Single-Threaded Event Loop with Memory-Mapped Files)
Option A is the correct choice because financial exchanges prioritize deterministic, ultra-low latency over raw throughput - a single-threaded approach eliminates context switching overhead, lock contention, and ensures predictable order processing sequence required for regulatory compliance, while memory-mapped files provide nanosecond-level persistence without syscall overhead, which is exactly how real exchanges like CME and NASDAQ operate their core matching engines.

Option B introduces non-deterministic ordering between threads and complex synchronization overhead that adds microseconds of jitter. Option C adds message serialization/deserialization overhead and unpredictable actor scheduling that's incompatible with microsecond requirements. Option D is completely unsuitable - blockchain consensus takes seconds/minutes while trading requires microsecond execution, plus the computational overhead would make high-frequency trading impossible.
</p>
</details>

