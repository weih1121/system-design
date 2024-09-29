## Introduction

When designing a system to achieve qualities we've just discussed, both software and hardware have to work together. Let me demonstrate this using a simple example.

## Evolution of a Startup's System

### Initial Deployment

Imagine that you are a startup founder. You build a minimum viable product (MVP), a web application. You deploy the application to the general purpose server that you bought in advance and installed in your home garage. The web application accepts HTTP requests from users all over the world, processes these requests, and stores data in a database.

### Identifying Bottlenecks

Initially, you deploy the database to the same physical server as the web application. But over time, you realize that your web application is CPU bound. It needs a lot of CPU resources to process all the requests, while your database requires a lot of memory and disk space. A single host with a lot of CPU, memory, and disk capacity is pricey.

### Optimizing Resources

But if you buy two servers, a compute-optimized one with high-performance processors, and a storage-optimized one with a lot of memory and disk space, it is cheaper and easier to scale up in the future. So you move the database to its own dedicated server. The setup worked really well for a while, until one day your dog accidentally turned off the application server. At that point, you decided to make your system more reliable.

### Increasing Reliability

So you bought a server rack. Then put and locked both your servers in the rack, away from your pets. Second, you bought one more application server and added it to the rack. Actually, you bought two servers: one application server and one more small server to act as a load balancer and distribute requests across application servers. To achieve data durability, you bought an additional database server and implemented data replication and backups. The setup worked really well for a while, until one day there happened a power outage in your area, and all servers died. At that point, you decided to further increase the reliability of your system.

### Geographic Distribution and High Availability

You took one of the application servers and the secondary database server and brought them to your friend's garage, several blocks away from your house. Now, if servers in your garage fail, your system remains available because servers in your friend's garage are running. That was smart. Although, it cost you additional expenses for one more server rack and the additional load balancer server. The setup worked really well for a while, until one day users from India said that enough is enough and they are no longer willing to accept poor performance of your system. Latencies for requests from India to the US were indeed high. At that point, you decided to call your friends in India and ask them to build exactly the same infrastructure you built on your own. You built in the US, so that India users are served by the system that is physically located in India. Nice. But what about users in South Africa, China, Australia, Brazil? They also expect high performance from your system.

### System Improvements

Actually, there are many things we can further improve in this small system. For example, this system is still not reliable. There is at least one point of failure—the database. Only one database server handles requests. The second database server only replicates data for durability. It doesn't serve requests from users. If the primary database server fails, the secondary server must somehow become primary. Later in the course, we will discuss how to solve this particular problem. For now, let me please not go too deep into details, as I hope my main point is clear.

## Building Highly Available Distributed Systems

To build highly available, highly performant, scalable, durable, secure distributed systems, we not only have to know software design principles and best practices, but we also need to understand how to build infrastructure for such systems. Fortunately, there are not so many things we should know as software engineers. Let's discuss a few key ones.

### Key Parameters of Servers

First, we have servers. There are four key parameters of a server:

1. **CPU**
2. **Memory Size**
3. **Disk Size**
4. **Network Bandwidth**

For every component of the system we design, we choose servers based on what resources the component needs the most. Out of these four, CPU is the easiest parameter to scale, while network I.O. is typically the hardest to scale.

### Server Racks

Next, we have server racks. Multiple servers are mounted within a server rack. It is convenient to place and maintain servers in a rack, as servers are physically easier to reach, examine, and manipulate. It also simplifies cooling and increases security, as servers can be locked in place in a rack. Each rack typically has its own network and power source.

**Chart 1: Comparison of Server Key Parameters**

| Parameter       | Scalability Ease | Description                        |
|-----------------|-------------------|------------------------------------|
| CPU             | High              | Easiest to scale                    |
| Memory          | Medium            | Needs scaling based on demand      |
| Disk            | Medium            | Needs scaling based on storage needs|
| Network Bandwidth | Low              | Hardest to scale, requires high bandwidth and low latency connections |

### Design Considerations for Server Racks

Regarding racks, the most important thing is that system designers usually don't care much about racks, but sometimes they do. Specifically, we may want all our servers to live in different racks, so that when one rack dies, we have spare hardware in other racks. This increases the availability of the system. Or vice versa, we may want all our servers to be placed as close to each other as possible in a single rack. We need these servers to talk to each other, and we want to minimize latency of communication. In this case, we trade performance over availability.

### Data Centers and Availability Zones

Next, we have data centers. A data center is a building that has lots and lots of racks. The data center has independent power, cooling, and physical security. It's like your house garage from the startup example, but is much more expensive. It is possible for the whole data center to be connected to the power supply system. But it's not completely unavailable at some point. For example, due to power outages, lightning strikes, tornadoes, earthquakes. Do you remember what we did with our garage when there was a power outage? Right, we moved half of our servers to the friend's garage. Exactly the same idea applies to data centers. And it has a cool name: **Availability Zone**.

An availability zone is one or more discrete data centers with independent power and networking. Availability zones help to ensure high availability and scalability. Instead of keeping all our system's hardware in a single data center, servers are distributed across multiple data centers. When there are availability issues in one data center, or not enough servers available, we can rely on other data centers in the availability zone.

### Regions and Global Deployment

A group of isolated and physically separate AZs within a geographic area is called a **region**. Remember in the startup example, we called our India friends to build a copy of the system in India. In region terms, U.S. and India represent two separate regions. In reality, though, a single country may have multiple regions. For example, U.S. East Coast region and West Coast region. Typically, there are at least two availability zones in each region. They are not too far away from each other within a radius of 100 kilometers. AZs in a region are interconnected with high bandwidths and low latency networking. Network latency between AZs is less than 2 milliseconds.

### Summary of Global System Architecture

Let's summarize what we have discussed:

- We have **regions** spread all over the world.
- Each region consists of one or more **availability zones**, where each availability zone consists of one or more **data centers**.
- Each data center contains lots of **racks**, and multiple **servers** are mounted within each rack.
- There are several different types of servers:
  - Some are optimized for compute-intensive workloads, and they have many CPU cores.
  - Some are optimized for memory-intensive workloads, and they have a lot of RAM.
  - There are servers optimized for high local disk throughput, for both reads and writes.

### Deployment Strategies for Non-Functional Requirements

When designing the system to address non-functional requirements, you should keep in mind where and how we are going to deploy it.

- **Worldwide Scale**: Deploy a copy of our system to every region.
  - Improves performance, as the system is physically closer to users.
  - Increases availability, as even if the whole region is down, we can forward user requests to other regions.
  - Improves scalability, as there is more hardware available for allocation.

- **Within a Region**: Deploy the system to multiple availability zones.
  - Further increases availability and scalability of the system.
  - Increases durability, as we can replicate data quickly between zones.

- **Within a Data Center**: Deploy the system to multiple availability zones.
  - If availability is critical, deploy two servers in different racks.

- **Component Deployment**: Deploy different components of the system, like various microservices, to servers optimized for the workload of each component.

## Comparative Charts

**Chart 2: System Deployment Hierarchy**

```mermaid
graph TD;
    A[Regions] --> B[Availability Zones]
    B --> C[Data Centers]
    C --> D[Server Racks]
    D --> E[Servers]
    E --> F[Microservices]
