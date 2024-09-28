# Determining Non-Functional Requirements

Determining non-functional requirements at the beginning of the interview is really important. We already know this. But here comes a problem: there is a long list of potential non-functional requirements—dozens of them. Are we expected to understand and remember all of them? **No**. Let's talk about several of the most popular ones.

Chances are high that the interviewer will ask you to focus on two or three requirements from this list. Along the way, I will remind you of several important terms. Speaking with your interviewer or your teammate at work using industry terms is always a plus.

## High Availability

There are several ways the term **availability** can be defined. Availability is commonly used to define the system uptime—the percentage of time the system has been working and available. For example, **99% availability** means that the system was unavailable about **3.65 days a year**.

In practice, though, you can often find availability measured as a success ratio of requests—the number of successful requests divided by the total number of requests in the system. **99% availability** in this case means that one request out of 100 fails. And it doesn't matter why the request has failed:

- Maybe due to a bug on the server side.
- Maybe the response hasn't returned in time.
- Or maybe there was a network outage and the request had never reached the server in the first place.

The latter example is interesting, as it immediately shows that availability can be different depending on the perspective. In case of a network failure, the availability drops for clients, as the system is unavailable for them. But the server might claim 100% availability, as the system was up and running all the time.

So **perspective matters**. And what we should truly care about as systems engineers is the **client's experience**, not the server's perspective.

When designing a system for which availability is important, we aim to **maximize availability**—make it as high as possible. But what does high availability really mean? Is 98% high enough? Or 99%? Or must it be 100% for the system to be considered highly available?

Actually, none of this. Of course, **100% availability is impossible**. I mean, it is possible in theory, but in practice, achieving 100% availability over arbitrary large time intervals leads to very complex designs and high costs for such systems. No matter how hard we try, there will be a sequence of hardware and software faults accompanied by human errors that will lead to a system failure.

### Designing for High Availability

It's not about a specific number, but about **system architecture and process**. The system must be designed, implemented, tested, and maintained with high availability in mind. Only then can we claim a system to be highly available.

Imagine the following situation: we created a web application and deployed it to a **single production server**. The server and network were healthy for the whole week, meaning that all requests have been successfully processed during the entire time. The availability of such a system is 100%, right? Yes. But is the system highly available? **No**. If the single server goes down, the whole system becomes unavailable. And who knows when it will be available again?

So **high availability involves both architecture and process**. This means that in order to create a highly available system, we first need to know and apply a number of **system design principles**. The good news is that these principles boil down to a limited number of specific system design concepts. We just need to know these concepts and incorporate them into the system architecture at the design stage.

Additionally, we need to establish and follow a **process**, or rather a set of processes, that help ensure high availability of the system over time.

Why do we need these processes? Why is architecture not enough? Because **systems are not static**. They change. We will add new features to the system, find and fix bugs, the load on the system may increase. We have to upgrade software and deploy security patches. Therefore, we need processes that help us safely make regular changes to the system while maintaining high availability.

## Principles for Designing Highly Available Systems

So what are these principles that help design highly available systems?

1. **Build redundancy into the system to eliminate single points of failure.**
   - Involves understanding topics such as regions and availability zones, fallback mechanisms, data replication, high availability pairs.

2. **Ensure the system can switch from one server to another without losing data.**
   - Involves DNS, load balancing, reverse proxies, API gateways, peer and service discovery concepts.

3. **Protect the system from atypical client behavior.**
   - Examples: too many requests, requests that are too expensive, requests that cause servers to crash.
   - Mechanisms: load sharing, rate limiting, shuffle sharding, cell-based architecture.

4. **Protect the system from failures and performance degradation of its dependencies.**
   - Adopt design patterns: timeouts, circuit breakers, bulkheads.
   - The system must know when and how to retry failed requests to its dependencies, and understand conditions under which retries are no longer safe.

5. **Detect failures as they occur.**
   - Monitoring at all levels of the system architecture.
   - Knowing what has failed and where to isolate the problem and recover quickly, ideally automatically.

## Processes to Ensure High Availability

We will quickly go over the necessary processes that the system must follow in order to consider itself highly available.

- **Change Management Process**: Ensure that all code and configuration changes are reviewed and approved by someone other than the author of the code.

- **Testing**: Create and regularly exercise tests to validate that newly introduced changes meet functional and non-functional requirements.

- **Automated Deployment Pipeline**: Deploy changes to a production environment frequently, quickly, and safely. The pipeline must be able to handle all the changes and roll back changes quickly when errors are detected.

- **Resource Monitoring and Scaling**: Monitor system utilization and add resources in a timely manner to meet growing demand.

- **Disaster Recovery Plan**: Create a plan to recover the system quickly in the event of a disaster—for example, a natural disaster or a large-scale technical failure.

- **Regular Failover Testing**: Regularly test failover to disaster recovery systems.

- **Post-Incident Analysis**: After an incident has happened (and they will happen), perform a post-incident analysis to establish the root cause of the failure and identify preventive measures.

- **Operational Readiness Review**: Prior to launching any major new feature and thereafter regularly (e.g., once a year), conduct an operational readiness review for the system to evaluate its operational state and identify gaps in operations.

If all the design principles are applied, and the processes are established, we can say that our system is highly available. Almost. All that remains is to validate this on a regular basis using a procedure known as **game day**. During a game day, we simulate a failure or event such as a sudden high load on the system, various dependency failures, and so on. We test system and team responses to simulated events.

## Team Culture

The last very important ingredient for building highly available systems is a **team**, rather a **team culture**. It's hard to enforce process discipline without a good team culture.

I mentioned before that for highly available systems, a high uptime value is not a goal; it's a byproduct. Meaning that a highly available system will have several nines availability because it is designed, implemented, and maintained as such. But honestly, I'm being a little sly when I say that the number is not the goal.

When building a system, we need to guarantee a minimum availability value for clients. To claim without proof that our system is highly available is not fair, to say the least. We need to commit to a certain number. Such a number is called a **Service Level Objective (SLO)**, and it can be a single value or a range of values. And the commitment between us (service providers) and clients of our system is called a **Service Level Agreement (SLA)**.

For example, Amazon DynamoDB promises **four nines** availability (99.99%). And in the event DynamoDB doesn't meet this commitment, AWS compensates customers. All public cloud service providers follow this rule.
