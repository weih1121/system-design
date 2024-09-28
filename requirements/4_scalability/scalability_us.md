# Scalability and Elasticity in System Design

The next quality is **scalability**. Scalability is the property of a system to handle a growing load. A load can be defined as:

- The number of requests per second
- The volume of incoming or outgoing data
- The number of concurrent connections
- Or something else

We scale systems **vertically** and **horizontally**:

- **Vertical scaling (Scaling Up)**: Adding more resources (CPU, memory, storage) to a single machine.
- **Horizontal scaling (Scaling Out)**: Adding more machines to the system.

You may also hear the term **shared-nothing architecture** to describe horizontally scalable systems.

## Vertical vs. Horizontal Scaling

Scalability is hard, and when designing a system, we should think about scalability in advance.

On one hand, the choice seems simple—we should prefer horizontal scaling. We can keep adding machines to such systems as load increases, while vertical scaling is limited. At some point, the load may surpass the biggest available machine. High-end machines are often very expensive; even the second or third most powerful machine may still be too pricey.

On the other hand, horizontal scaling comes with its own set of challenges:

- **Service Discovery**: Clients need to discover service machines.
- **Request Routing**: We have to solve the request routing problem because we need to ensure requests from the same client go to the same machine, which is common for systems that store client state.
- **Maintenance**: We have to maintain multiple machines.

Because of this additional complexity, it's okay for some systems to start with a single machine, scale it up until it becomes too expensive, and then migrate to a horizontally scalable solution.

### Example: Relational Database

A relational database is a good example. We typically start with a single database instance and scale it vertically up to a limit. It is possible to scale relational databases horizontally using sharding to scale writes and replication to scale reads. Later in the course, we will discuss all the details.

**My point is that everything has trade-offs in system design. Please don't assume that we only scale systems horizontally these days.**

This is especially true in interviews when candidates often mistakenly believe that high scalability—which often implies horizontal scaling—is a default requirement for every system nowadays. No, it's not. The rule of thumb is to talk to the interviewer to clarify scalability needs.

## Scalability vs. Availability

You might argue, and it's quite reasonable, that in a single-machine solution, it's often not scalability that is a primary concern but availability. The only machine will fail one day. Therefore, any system we design has to be distributed and horizontally scalable. This allows us to have multiple redundant machines in the system, which increases availability.

**Good point.** But the truth is, high availability can be achieved using just a single machine—for example, using a **high-availability pair**, also known as an **active-passive setup**. We will talk more about this while discussing load balancers.

Horizontal and vertical scaling can be used in conjunction. Let's say we have a horizontally scalable system, and we want to keep the number of machines small, as it is easier to maintain such a system. As the load increases, we replace the existing machines with more powerful ones. When scaling up any further becomes too expensive, we get back to horizontal scaling.

## Introducing Elasticity

A concept that very often goes together with scalability is **elasticity**. Elasticity is the ability of a system to acquire resources as it needs them and release resources when it no longer needs them.

Although the terms look similar, elasticity is not the same as scalability.

### Daily Load Example

Here's an example:

Let's say the request distribution for our system in any single day looks like the following—a normal distribution. The number of requests goes up in the morning, peaks, and goes back down. This is a very common traffic pattern; many systems have it.

- We need **N machines** to handle the load at night when the request rate is low.
- We need **2N machines** to handle the load during the day when the request rate is high.

If our system is **elastic**, we can start with N machines at night and keep adding machines throughout the day as the load increases. After the peak load has passed, we can gradually decrease the total number of machines.

This is what elasticity is about:

- **Grow or shrink infrastructure resources over short time intervals to adapt to workload changes.**
- **Save money** by not over-provisioning resources.

A non-elastic system would require us to always have at least 2N machines all the time. Elasticity helps to reduce costs.

### Annual Load Example

Now let's look at the annual load. Our system is becoming more and more popular, and the load grows over time. Several times throughout the year, we need to scale up or scale out to accommodate growth.

This is what scalability is about:

- **Provision resources in an incremental manner over longer time intervals.**
- Scalability is about long-term, strategic needs.

It also follows from these considerations that **scalability is required for elasticity**, right? If the system is not scalable, we cannot quickly provision and deprovision resources, which means that we cannot make the system elastic.

## Manual vs. Automatic Scaling

Scaling can be performed **manually** or **automatically**. Automatic scaling is also known as **auto-scaling**, and it's one of the cornerstones of cloud computing. We will talk more about auto-scaling in the course and even discuss a high-level design of an auto-scaling system.

Thank you.
