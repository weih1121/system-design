# Defining Functional Requirements in System Design

When writing a design document, we typically capture **functional requirements** in the form of **use cases**, where each use case describes one or several behaviors of the system.

**Example:** When a user does this, the system does that.

We do pretty much the same in an interview. Our goal as an interviewer is to identify one, two, maybe three functions of the system, and for the rest of the interview, focus only on this small set of use cases.

If we are lucky, the interviewer might enumerate all the functions for us. For example:

- **Let's design a YouTube-like system, specifically video upload and video view functions.**

If we are not so lucky, we will hear only the first part of the problem statement:

- **Let's design a YouTube-like system.**

It's our job now to try to define specific functions. How can we do this? Let me share with you a few practical tips.

## Practical Tips for Defining Functions

In general, to define functional requirements for a system, we need to identify **who is going to use the system and how**.

During an interview, this is reasonably easy to do for systems we know and use. For example, when challenged to design a system like:

- YouTube
- Facebook
- Twitter
- Some messenger
- Google web services such as Gmail

But this can be quite hard for systems we know little about—systems we don't interact with directly. For example:

- A rate-limiting system
- A fraud prevention system
- A content delivery network

For such systems, it can be challenging to understand what the input is and what the output is.

In both cases, whether we are familiar with the system or not, to define functional requirements, we should **start with the customer and work backwards**.

## Starting with the Customer and Working Backwards

Let's try to define the functional requirements by starting with the **customer**. We determine the number of customers—by customers, we mean users or clients of the system we need to design. This can be:

- People
- Digital devices
- Other systems

By the way, **working backwards** is a product development approach popularized by Amazon. The idea is this:

> Before developing a product, a product team writes a document where they clearly define the problem, intended customers of the product, and how customers will benefit from it.

So let's clarify with the interviewer who the customers are.

### Examples:

- **YouTube**:
  - Customers who upload videos
  - Customers who watch videos
- **Rate-limiting system**:
  - Customers are other systems, such as web services looking for overload protection

## Understanding How Customers Use the System

Now that we know who the customers are, we need to clarify **how they use the system**.

### Rate-Limiting System Example:

- Every time a request is made to a web service, the web service calls the rate-limiting system and receives a response as to whether the request should be served or not.

### YouTube Example:

- **Content Creators**:
  - Upload videos
  - Create posts
  - View analytics
  - And many more
- **Viewers**:
  - Search for videos
  - Watch videos
  - Leave comments
  - And a lot more

Each of these functions is a separate big system on its own. The interviewer knows this and will tell us what specific functions we should focus on when we ask.

## Dealing with Ambiguity in Interviews

If you wonder why there are all these mind games—why can't the interviewer define clear functional requirements for the candidate beforehand—you know the answer already.

- The interviewer wants to see how we **deal with ambiguity**, and more specifically, how we analyze a big obscure problem and how we reduce complexity by breaking the problem down into manageable pieces.

The amount of ambiguity depends on the job level we apply for:

- For **entry-level positions**, we should expect zero to minimal ambiguity.
- For **senior-level positions**, we should expect a lot more ambiguity in the problem statement.

## Expressing Functional Requirements as APIs

Sometimes, and this is not uncommon, the interviewer may verbally define functional requirements and ask us to express those requirements in the form of an **API**, typically **RPC** or **REST**.

For example, the interviewer defines multiple different use cases like a list, and our task is to convert the list into functional requirements. The interviewer will then convert these use cases into REST APIs, like this.

Sometimes the whole interview is to:

- Identify functional requirements
- List key functions of the system
- Create corresponding APIs

To do this, we need to know **RPC and REST API guidelines and best practices**. So be prepared. We have a separate chapter in the course where we cover all this stuff.

## Conclusion

Defining functional requirements is a crucial step in system design, both in documentation and during interviews. By starting with the customer and working backwards, we can effectively identify the key functions of the system. Understanding how to deal with ambiguity and express requirements as APIs is essential, especially for higher-level positions. Continuous learning and preparation on topics like RPC and REST APIs will greatly benefit you in your system design journey.
