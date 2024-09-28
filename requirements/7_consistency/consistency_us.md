# Consistency in Distributed Systems

Consistency is probably one of the most misunderstood concepts in distributed systems. And I see at least two reasons for this.

First, the term **consistency** is highly overloaded. Engineers working with relational databases, and more specifically transactions, know about the **ACID** acronym, where **C** stands for **Consistency**. Engineers working with NoSQL databases have heard about the **BASE** acronym, where **E** also stands for consistency, but this time the word **Eventual** is followed by it. Engineers working with Cassandra database know that consistency can be **tunable**. Many have heard of the **CAP theorem**, and there is some consistency there as well.

Do all these terms refer to the same notion of consistency? **No.**

- **ACID consistency** describes **transaction consistency**, and it guarantees that database constraints are not violated when the transaction is executed. For example, when transferring money between two accounts, the total amount must not change.

All three terms refer to data replication and consistency between multiple copies of data. And these three terms are not equal either. Each of them implies a different system behavior. We'll elaborate more on this a bit later.

The second problem is that consistency is complicated. Try to read the wiki page about consistency models. Thank you, but no. We need to know and understand nuances of consistency. So I'll try to distill the key concepts and focus on those.

Please take a comfy seat and let me tell you a story.

## A Short History

In the old days, as in the early 2000s, the life of an engineer was simple. A single instance of a relational database was enough to handle traffic for many popular websites. I may exaggerate a little. But it was so long ago that history became legend.

It is easy to write applications and test them when there is a single copy of data. When you write to the database and then read from it, you always know what value to expect.

## The Rise of Data Replication

Internet traffic was growing fast, and data replication became a necessity for popular websites, especially social networking services. And the primary reason for this was not durability, but **read scalability**. As social media websites are read-heavy, and each replica can serve read requests independently. Increased availability and durability are two other nice benefits of replication.

Now, in the presence of multiple copies of data, we have two choices:

1. Require the distributed system to act as before, when there was a single copy of data, which means all copies are always in sync with each other.
2. Abandon this idea as being obsolete and accept a new norm—that copies of data can be out of sync for some reasonable period of time. In other words, accept the fact that there can be several different copies of data.

If we choose the first option, we can claim that our system provides **strong consistency**. With the second option, our system provides **weak consistency**.

Back in old days, people were smart, and they thought that terms like strong and weak are too vague. How strong? How weak? What if later we come up with some consistency type that is stronger than weak consistency, but weaker than strong consistency? How should we name it then? Medium consistency?

So they decided to give specific names to different consistency types. Moreover, they decided to define a set of rules for each consistency type—rules that define the order of updates in the system, and when these updates become visible to users. Such set of rules is called a **consistency model**. And in fact, people invented many consistency models.

Let's mention some popular ones.

## Consistency Models

### Linearizability

The first one is **linearizability**. This is the strongest consistency model that can be achieved in distributed systems. Let me be clear here: in theory, there is even a stronger consistency model called **strict consistency**, but the strict consistency model is purely theoretical and cannot be implemented in practice. So linearizability is the strongest model that can be implemented in practice.

**Linearizability** means that after the update completes, all clients, when they read data, get back the updated value. Although there are multiple copies of the data in the system, the linearizable system creates the illusion of a single copy.

When you hear someone talk about strong consistency in the context of distributed systems, they most likely mean linearizability. But it's better to ask them to confirm.

Linearizability is a great model. It's easy to reason about. We use linearizability in systems that track some balance—bank account balance, online shop product inventory, airline tickets, and so on. When there is a change to the balance, we want all clients to have the same view.

But linearizability has two big drawbacks:

1. **Performance**: Linearizability is slow.
2. **CAP Theorem**: The famous CAP theorem tells us that in case of a network partition (which means a network fault), we can choose either linearizability or availability. We cannot have both.

I need to emphasize that the CAP theorem has many nuances, and it deserves a lot of attention. For now, I want you to remember two things about the CAP theorem:

- **C** in CAP means **strong consistency**, and more specifically, **linearizability**.
- We only need to choose between consistency and availability in case of a **network partition**. If the network is reliable, we don't need to choose between them. Both availability and strong consistency can be satisfied.

Because of these two drawbacks, many systems prefer to drop linearizability and use a weak consistency model, such as **eventual consistency**.

### Eventual Consistency

**Eventual consistency** means that if there are no additional updates made to the object, eventually all reads will return the latest written value of that object, after all replicas are synchronized.

Eventual consistency is a weak guarantee, as it doesn't specify when exactly replication will complete. If there are no failures in the system, replication will happen quickly, in a matter of milliseconds or seconds. But when issues happen in the system, some replicas may fall behind, and they will keep returning the older value when a read request hits them, while other replicas return the new value.

Eventual consistency can be way faster than linearizability, especially in cases when replicas are geographically distributed. Another advantage is that we don't need to sacrifice availability. We can have both availability and eventual consistency in case of network partitions.

But, as with everything, the eventual consistency model has drawbacks.

#### Drawbacks of Eventual Consistency

Eventual consistency may cause a lot of confusion. For example:

- **Disappearing Comments**: You read comments under a blog post. Initially, there is only one comment and you see it. A bit later, after refreshing the page, you no longer see any comments. The only comment suddenly disappeared.
  - **What happened?** Initially, comments were returned by one replica. After the refresh, another replica serves the read request, and the second replica doesn't know about the comment yet.

- **Delayed Updates**: You write a comment and press the send button. The page refreshes, but your comment is not on the page. After several more page refreshes, you finally see your comment.
  - **What happened?** Your comment hasn't propagated to all replicas. The replica you read from doesn't have the comment yet.

- **Out-of-Order Updates**: After re-reading your first comment, you decide to add another comment to clarify some details. You press the send button, the page refreshes, and you see both your comments on the page, but the order is wrong. Your first comment follows your second comment, and this completely messes up the logic of your message.
  - **What happened?** The database system is partitioned. There are several shards that process writes in parallel, each with its own set of replicas. Your first comment was processed by one shard, while your second comment was processed by another shard. Even though the first shard got the first comment earlier than the second shard, the second shard completed replication faster. Now, when we read comments, the system assumes that the second comment is older, since it was created first on the replica. A similar problem can also occur if two shards have different clock times.

After seeing this, you close the website and promise yourself to never open it again.

## Beyond Eventual Consistency

I could go on, but let me please stop right here and remind you that people were smart back in old days. They created consistency models that helped to solve all the mentioned problems. These consistency models are stronger than the eventual consistency model but weaker than linearizability.

- **Monotonic Reads**: Solves the problem where data seems to disappear and reappear.
- **Read Your Writes**: Ensures that you always see your own updates.
- **Consistent Prefix Reads**: Ensures that data is seen in a consistent order.

Later in the course, we will discuss how to implement these models, including, of course, eventual consistency and linearizability.

It is important to note that none of these three models is stronger than the other two. On the slide, they may look ordered by strength, but this is not the case. I just couldn't think of a better graphic to show this. All we can claim is that these three models are stronger than eventual consistency and weaker than linearizability.
