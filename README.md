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

###### 1. You are designing a globally distributed messaging platform (like WhatsApp or Slack) that ensures message delivery guarantees, low latency, and eventual consistency across regions. What is the most appropriate architecture approach to ensure scalability, fault tolerance, and minimal message loss in the face of regional outages?

- A. Use a centralized message broker in a primary region with failover replicas in other regions and synchronous replication.
- B. Use a distributed message queue (e.g., Kafka with geo-replication) combined with local storage and asynchronous replication between regions.
- C. Deploy a monolithic message server with high availability in a single data center, using CDN for read optimization.
- D. Rely on client-side message caching and retries only, without any backend persistence to reduce infrastructure complexity.


<details><summary><b>Answer</b></summary>
<p>

#### Answer: B**
The correct answer is B** because it provides a balance between **low latency**, **fault tolerance**, and **scalability**. By using a distributed message queue like **Kafka with geo-replication**, each region can handle **local writes** quickly while asynchronously syncing messages across other regions. This ensures **eventual consistency** and avoids service disruption during **regional outages**. Unlike centralized or monolithic approaches, it **scales horizontally** and supports **global usage** efficiently.


</p>
</details>
