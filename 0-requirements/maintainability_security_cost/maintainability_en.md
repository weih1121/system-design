Since the moment the product is launched and until its end of life, we need to maintain the product, which involves activities like identifying bugs and fixing them, adding new features, improving performance, increasing test coverage, writing documentation, and many more. Companies that take maintainability seriously have a long list of questions you need to answer before launching the product. I am referring to the operational readiness review process that we mentioned earlier. These questions, or better said, recommendations, accumulate best practices for achieving operational excellence. The goal is to educate teams and identify operational gaps in the system. And when all major issues are resolved, we are ready to launch the product. The main, and at the same time the tricky point, is that we should know these operational practices. We need to incorporate them into the system. We need to integrate them into the system at the design stage.

Okay, enough talking about abstract things. Let's get down to some specifics. During an interview, we should expect questions related to the following aspects of maintainability.

**1. Failure Modes and Mitigations**

- If some component fails, what happens to the rest of the system?
- How does the system handle network partitions?
- How do we want the system to handle network partitions?

**2. Monitoring**

- How do we monitor the health of the system?
- In case of a system failure, how do we know what exactly is broken?

**3. Testing**

- How to test each individual component?
- How to test data flows end-to-end?

**4. Deployment**

- How to deploy regular changes safely?
- How to roll back the changes quickly?

This is by no means a complete list of maintainability-related topics. But if we know at least these four, we are set well for a meaningful discussion with the interviewer. A million-dollar question, then, is how to learn all these topics. Each one is a book of its own. Easily, we will take a pragmatic approach. In this course, we build a reference architecture of a distributed messaging system. We will take the reference architecture, and I'll show how to apply these four topics to the real system. For each of these topics, we will focus on concepts you will often find in practice.

**Security** is another very broad area. Secure systems include three fundamental attributes, often referred to as the CIA triad.

- **Confidentiality:** Data is protected from unauthorized viewers.
- **Integrity:** Data is not corrupted or lost, and only authorized viewers can modify data.
- **Availability:** Authorized users have access to resources when they are needed.

In practice, all this means that our system has to protect itself from unauthorized access and security attacks. So, popular interview-related topics include identity and permissions management, who can access the system, who can access what in the system, how to implement authentication and authorization in the system.

**Infrastructure Protection:**

- Is the system protected from DDoS attacks?
- Is it protected from other common attacks, such as signal injection or cross-site scripting?
- Should we use a web application firewall or an API gateway to implement protection?

**Data Protection:**

- How to protect data at rest?
- How to protect data in transit?

The theoretical portion of security is huge. There are many books on the topic. But in practice, when designing a system, we typically operate with a relatively small set of key concepts and best practices. Therefore, we will employ the same pragmatic approach as for maintenance. We will take the reference architecture and I will show how to implement key security mechanisms in a distributed system.

The last non-functional requirement in our list is **cost optimization**. When designing a system, we should try to estimate its cost. At a high level, this cost consists of three parts:

1. **Engineering Cost:** The effort needed to design, implement, test, and deploy the system.
2. **Maintenance Cost:** The effort needed to maintain the system going forward.
3. **Resource Cost:** Hardware and software.

Should we expect to be asked about any of these three costs during an interview? Let's see.

To properly estimate the cost of engineering, we need to create a breakdown of tasks. As detailed as we can. This is both time-consuming and requires thorough understanding of the system. In a system design interview, we have neither enough time nor deep understanding of all the system details. We need to understand the system design process. So we should not expect questions related to engineering cost during the interview.

As for the cost of maintenance, there may be such questions. Specifically, questions on how to reduce maintenance cost. Which typically means automation. Automation of monitoring, testing, deployments, configuration management, security patches, and so on. We will cover this topic separately when discussing maintainability.

As for the cost of resources, we should expect such questions either. The cost of maintenance. The interviewer may ask us to optimize the cost of resources. To see what trade-offs we are going to make. Let's elaborate on this. We need both hardware and software resources to run the system. Hardware resources include all the physical machines, hardware load balancer devices, networking devices. We need to run the system. The interviewer may ask us to do capacity estimation. To evaluate how many machines we might need to process data. Or how many disks we might need to store data. We then multiply the result number by the price of a single machine. And get the approximate total hardware cost.

Software resources include all the commercial software we use in the system. Nowadays, most of this software is provided by public cloud services. Actually, it doesn't matter much who provides the software. A public cloud provider or a service team next door at your company. We can treat both similarly from the cost estimation perspective. Whether it is a managed database service provided by AWS or by the in-house service team makes little difference. There is no free lunch. We need to pay in both cases. In case of cloud providers, this is real money we pay. In case of companies' proprietary solution, this is typically virtual money. When one organization owes money to another organization inside the company. But this is still money. And it contributes to the total cost of the system. Therefore, we should count it as well.

And when we use managed services, there are three main parameters that influence the cost:

1. The number of requests we make to the service.
2. The number of bytes we transfer, either in or out.
3. The number of bytes we store.

If we put all this together, we can write down the following formula:

The total cost of resources equals the cost of hardware resources plus the cost of requests made plus the cost of bytes transferred plus the cost of bytes stored.

Please get me right. This is not the exact formula. If you ask me where are the resources we pay per hour, like virtual machines in the cloud or load balancers. Let's include them into the hardware cost. We can also include the cost of different licenses into the formula. But I will not. My goal is to show how system design influences the cost. And although this formula may not include all the expenses, it serves my purpose very well.

Let's see. When we want to increase availability of our system, we need to add redundant hardware. The hardware cost goes up. And the total cost of resources goes up. To increase durability, we need to replicate data. And store several times more bytes. The storage cost goes up.

At the same time, there are many cost reduction techniques. **Elasticity**, for example, allows us to release resources when we no longer need them, reducing hardware cost. We can use **long polling** and **request batching** to reduce the number of requests made to the service. **Data compression** helps to decrease the number of bytes we transfer over the network. To reduce storage cost, we can introduce two storages: **hot** and **cold**. Hot storage represents frequently used data that must be accessed fast. For example, we can use DynamoDB as a hot storage. While cold storage doesn't require fast access and mostly represents archived and infrequently accessed data. For example, we can use S3 as a cold storage. Cold storages are typically less expensive. These are just a few examples.

As with maintainability and security, let me use the pragmatic approach here as well. We will take the reference architecture and I'll show different system design techniques for cost optimization on a real system, including how to do back-of-the-envelope calculations for capacity estimation.

As you may see, maintainability, security, and cost optimization are all about knowledge. Knowledge of best practices and various optimization tricks. In interviews, we don't have to reinvent the wheel when faced with these requirements. We need to learn these things in advance. And when asked, pull the specific knowledge out of the pocket.
