# Fault Tolerance, High Availability, Resilience, and Reliability in System Design

Another concept that often goes together with **high availability** is **fault tolerance**. These two terms are often used interchangeably. When people say a fault-tolerant system, they mean a highly available system, and vice versa. I often do it myself. However, if you look deeper into the literature, you will find people explaining the difference. I can't say that I agree with all the arguments, but I will explain the nuances and let you decide for yourself.

## Terminology Lesson

Before doing that, here's a short terminology lesson.

- **Error**: A mistake, usually made by software developers while writing programs.
- **Fault**: A flaw in a program, also known as a bug, that causes the system to behave incorrectly. An error may lead to a fault.
- **Failure**: The inability of the system to perform its required function. One or several faults may result in a system failure.

A **fault-tolerant system** knows how to isolate faults so that faults do not cause overall system failure.

## Fault Tolerance vs. High Availability

So, what is the difference between a **highly available** and a **fault-tolerant** system?

In short:

- A **highly available system** accepts the fact that downtime is possible and tries to minimize it.
- A **fault-tolerant system** has the goal of zero downtime.

It follows from this definition that fault tolerance is like an upgraded version of high availability. If the system is fault-tolerant, then it is also highly available, while the opposite is not necessarily true.

### Analogy: Car vs. Airplane

It may be easier to understand these nuances using a car and an airplane analogy.

- **Car**:
  - Has an extra tire.
  - In case of a tire failure, we can stop and quickly change it.
  - There is downtime, but it is short.
  - Think of a car as a **highly available system**.
- **Airplane**:
  - Cannot easily stop in case of a failed engine.
  - Other engines have to take over to ensure zero downtime (until the next stop).
  - An airplane is a **fault-tolerant system**.

## Designing Fault-Tolerant Systems

While designing a fault-tolerant system, we should apply all the same design principles and processes we mentioned previously for achieving high availability. The only difference is that achieving fault tolerance may require redundant resources beyond the level typically needed to achieve high availability, resulting in a higher cost of the system.

Now you know the difference between fault tolerance and high availability. I think that both terms can be used interchangeably. High availability is open-ended; there is no limit on how high we want the system's availability to be. When we say that the system is highly available, we imply that it knows how to tolerate faults. And when we say that the system is fault-tolerant, we imply it guarantees minimum downtime, which by definition means a highly available system.

## Introducing Resilience

I hope your head hasn't exploded yet. And if not, let me bring in the term **resilience**.

Systems that, in the face of faults, can provide and maintain an acceptable level of service are called **resilient systems**. Sounds almost the same as fault tolerance, doesn't it? Indeed. These two terms are also often used interchangeably. If you Google it, you can find sources that explain slight distinctions between these terms. But honestly, I don't want to tangle you with all these nuances—they're not that important. You will also come across the word **resiliency** in many sources, which is exactly the same as resilience. These two terms are variants of the same noun, so feel free to use either of them.

To ensure a system is resilient, we need faults to happen, as we want to continually exercise all the mechanisms introduced in the system for fault tolerance. This means we may want to deliberately generate faults—for example, by regularly killing random instances of a software service. There is a technique called **chaos engineering** that helps simulate and test various system failures.

### Chaos Engineering vs. Game Days

Chaos engineering may remind you of the **game day** process we mentioned earlier. They are indeed similar, but not the same.

- **Game Days**:
  - Mostly focused on the **team** and its actions.
  - Help the team build muscle memory on how to respond to events.
- **Chaos Engineering**:
  - Mostly focused on **system behavior**.
  - Goal is to automate experiments and run them continuously in production.

We'll return to this conversation later in the course when we discuss testing and operational excellence in distributed systems.

## Reliability

One more concept related to availability—and this is the last one, I promise—is **reliability**.

Simply put, **reliability** is:

- **High Availability** plus
- **Correctness** plus
- **Time**

**Correctness** means that the system returns the correct result, and **time** means that the system replies back within an acceptable time.

Both correctness and time are important here. You may have a system that is always available, but due to frequent data corruptions, it returns incorrect results, or the system is too slow and the response is no longer useful. In both such cases, although we have a highly available system, it's not reliable.

## Putting It All Together

Here's an interesting thing. In the previous video, we talked about high availability. In this video, we have discussed fault tolerance, which can be seen as a stronger version of high availability. We have then introduced resilience, which is almost the same as fault tolerance, and reliability, which is high availability plus correctness and time. All of these terms are very similar to each other.

However, you will hear people use several of these terms in one sentence. For example, someone might say that the system she has created is **reliable and resilient**. How is this possible? Isn't this a tautology? This happens because these terms, although similar, are not the same. They have slightly different connotations, and here they are.

### Different Connotations

- **High Availability and Reliability**:
  - Ability of a system to handle **known and expected failures**.
  - Examples: server crash, power outage, network problems.
  - By calling the system reliable, we highlight that it knows how to deal with these problems.

- **Resilience**:
  - Ability of a system to handle **unexpected failures**.
  - Examples: sudden surges in traffic, dependency failures.
  - By calling the system resilient, we highlight that it knows how to handle unexpected issues.

So, by calling the system **reliable and resilient**, people imply that their system is super reliable.

At the same time, many people choose not to distinguish between expected and unexpected failures. By calling the system **reliable**, they imply that the system can handle both. In other words, resilience is considered a necessary component of reliability.

## Conclusion

Okay, we are done with all this availability, reliability, and resilience. And I want to make it clear: we spend this much time talking about all these terms not because we are nerds who know all the nuances and want to sound smart. The key point is that all these concepts relate to availability, and when your interviewer asks you to design a highly available or fault-tolerant or resilient or reliable system, it is all about the same set of principles and best practices that we should be using. And we will cover the vast majority of them.

The next time you read or write a design document, try to pay more attention to how those terms are used.
