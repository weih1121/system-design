# Performance in System Design: Latency and Throughput

When people hear about **performance**, they usually think of either the time required to process something or the rate at which something is processed. It could be both. Let's clarify this.

- When you press the play button on a video, you expect the content to be delivered to your device as quickly as possible. A non-functional requirement for the video hosting service is to ensure a **short download time**, even when network conditions are bad. So, it is **time** that matters.
- But when you decide to like or dislike a video, you do not care about the time it takes to complete this operation—whether your reaction will be counted immediately or several minutes later. Nonetheless, we expect the system to accurately handle all reactions coming from all users. In this case, what matters is **how many reactions the system can handle in a given time period**. In other words, the **rate** of the processed reactions matters.

The terms **time** and **rate** are too broad. In software engineering, we use the terms **latency** and **throughput** instead. Let's talk more about these two terms.

## Latency

When a client makes a request, the request first travels across the network, then arrives at the server where it is processed, and the response then comes back to the client. The total time it took to respond to a request is called the **response time**.

- **Response Time** = **Network Delay** (time on the wire in both directions) + **Service Time** (processing time on the server)

We could break down the formula further to include:

- The time required to read data from a socket on the server side.
- The time the request spends in a queue waiting for processing.

But for our discussion, we'll keep it simple.

### Different Interpretations of Latency

People mean different things by **latency**:

- Some mean **network delay** and call it **network latency**.
- Others mean **service time** and sometimes specify **server-side latency**.
- Some refer to **response time** as latency and may specify **client-side latency**.

Because **latency** can mean network delay, service time, response time, or something else, it's important to understand the context. If unsure, talk to your interviewer or team to clarify this.

### Measuring Latency

Latency is never a constant number. Different requests, even from the same client, will have different network delays, processing times, and response times.

There are two common ways to measure latency:

1. **Average Latency**:
   - Calculated by summing individual request latencies and dividing by the total number of requests.
   - Easy to calculate but can be misleading because it doesn't reflect users' experiences accurately.

2. **Percentiles**:
   - Sort all requests in a given time interval by their latency.
   - **P50 (Median)**: 50% of requests have latency less than or equal to this value.
   - **P99**: 99% of requests have latency less than or equal to this value.
   - We focus on high percentiles like P99 to improve the experience for users who suffer the most.

### Optimizing Latency

As engineers, our goal is to investigate high latencies, identify root causes, and try to reduce **tail latencies**. But how much should we reduce latency?

- If P99 latency of 3 seconds is too high, is 2 seconds acceptable? 1 second?
- Lowering latency further may require significant effort.
- We need to optimize the chosen percentile (e.g., P99) to a reasonable degree (e.g., 1 second) and stick to this number.
- We can then promise this number to clients as part of an **SLA** (Service Level Agreement).

## Throughput

**Throughput** is the rate at which something can be processed.

- Examples:
  - The number of requests a web server processes in one second.
  - The number of queries a database handles in one minute.
  - The number of network packets that arrive at their destination successfully in one hour.

The higher the throughput, the better the performance.

### Increasing Throughput

There are two options to increase throughput:

1. **Decrease Latency**:
   - Less time needed to process each request means more requests can be processed.

2. **Scale the System**:
   - **Vertical Scaling**: Add more resources to existing machines.
   - **Horizontal Scaling**: Add more machines to the system.

### Examples

- **Parallel Processing**:
  - Split a big file into chunks.
  - Process each chunk on separate worker machines in parallel (MapReduce paradigm).

- **Message Queues**:
  - If one consumer can't keep up, add more consumers.
  - If multiple consumers can't read from the same queue, create several queues and distribute messages.

- **Database Sharding**:
  - Split database into multiple instances (shards), each storing a subset of data.
  - Distribute write operations across shards.

- **Caching Systems**:
  - Replicate data across several cache instances.
  - Allow each replica to serve read requests.

## Bandwidth

While talking about throughput, it's important to mention **bandwidth**:

- **Bandwidth** is the maximum rate of data transfer across a given path, usually measured in bits per second.
- **Analogy**:
  - Think of bandwidth as a tube.
  - Think of throughput as water flowing through the tube.
    - If the tube is full, throughput equals bandwidth.
    - If the tube is half full, throughput is half the bandwidth.

When trying to increase system throughput, remember about bandwidth. Sometimes, we just need a bigger tube to achieve higher throughput, especially when scaling network resources.
