# Part 3: Prompt Preparation
## Task 3.1: Comprehensive Prompt Documentation

### 3.1.1 Repository Context

The aiokafka repository provides an asynchronous Kafka client implementation for
Python applications. It is built using Python’s asyncio framework and is designed
to allow non-blocking interaction with Apache Kafka. This library supports both
producing and consuming messages while integrating with event-driven Python
applications.

The users of aiokafka are backend developers and system engineers who work with
distributed systems and real-time data streams. It is especially useful for
developers who build high-throughput services such as event-processing pipelines,
log aggregation systems, streaming analytics platforms, and microservices that rely
on Kafka for message-based communication. Since aiokafka is asynchronous, it is
commonly used in applications where scalability and efficient use of resources are
important.

aiokafka works in the domain of event-driven and stream-based communication in
distributed systems. Apache Kafka is widely used to handle large volumes of data
across multiple producers and consumers, and managing offsets, partitions, and
message flow correctly is an important challenge. aiokafka simplifies interaction
for Python developers by providing an API that hides most protocol complexity while
still allowing fine-grained control over consumer and producer behavior when needed.



### 3.1.2 Pull Request Description

This pull request introduces two new consumer API methods: seek_to_beginning and
seek_to_end. These methods allow a Kafka consumer to directly reposition its offsets
to the earliest available messages or to the latest messages in a topic. Before
this change, users who needed similar behavior had to manually manage offsets or
rely on indirect mechanisms, which made the process more complex and error-prone.

The main requirement for these changes is to improve usability and align aiokafka
more closely with common Kafka consumer patterns. In many real-world cases,
developers need to restart consumption from the beginning of a topic for
reprocessing or skip historical data and consume only new messages. Without
dedicated APIs, users had to write extra code themselves because the library did not
provide specific support for these actions.

Previously, offset control was possible but not directly exposed through simple,
intent-driven methods. With the introduction of seek_to_beginning and seek_to_end,
this functionality becomes clearer and easier to use. The new behavior allows
developers to express their intent directly through the consumer API, reducing
ambiguity and improving code readability. This change does not remove or modify
existing APIs, it only adds new functionality while keeping older behavior intact.


### 3.1.3 Acceptance Criteria

✓ When `seek_to_beginning` is called, the consumer should start reading from the
  earliest available offset for all assigned partitions.

✓ When `seek_to_end` is called, the consumer should begin consuming only new messages
  produced after the call.

✓ The new seek methods should function correctly across topics with multiple
  partitions.

✓ Existing consumer behavior should remain unchanged if the new APIs are not used.

✓ The implementation should handle seek operations without causing runtime errors
  during active consumption.

### 3.1.4 Edge Cases

- Calling `seek_to_beginning` on a topic that currently contains no messages.
- Invoking `seek_to_end` before the consumer has fully completed partition
  assignment.
- Handling seek operations during partition rebalancing events.

### 3.1.5 Initial Prompt

You are working with the aiokafka repository, which provides an asynchronous Kafka
client for Python built using asyncio. The library is used by developers to build
scalable, event-driven systems that rely on Kafka for message streaming and
communication. The goal is to update the aiokafka client so that managing Kafka
message offsets becomes easier and more intuitive for developers.

Implement support for two new consumer methods: seek_to_beginning and seek_to_end.
These methods should allow users to reposition the consumer’s offsets to the
earliest or latest available messages for the currently assigned partitions. The
implementation should integrate smoothly with the existing consumer workflow and
preserve non-blocking behavior.

The implementation must satisfy the following acceptance criteria: calling
seek_to_beginning should cause the consumer to read from the earliest available
offsets, while calling seek_to_end should cause it to consume only new messages.
These methods must work correctly across multiple partitions and must not break
existing applications that do not use the new features.

Edge cases must be considered, including empty topics, calls made before full
consumer initialization, and situations where partition rebalancing occurs. The
implementation should handle these cases carefully without raising unexpected
exceptions or leaving the consumer in an inconsistent state.

Finally, ensure that appropriate test cases are added or updated to validate the
new behavior. Tests should confirm correct offset positioning, verify backward
compatibility, and cover all identified edge cases. The final solution should
improve developer experience while maintaining correctness and stability.

