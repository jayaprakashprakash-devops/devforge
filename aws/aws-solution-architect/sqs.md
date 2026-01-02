# Amazon Simple Queue Service (SQS) - Overview

## What is Amazon SQS?

Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables decoupling and scaling of microservices, distributed systems, and serverless applications. SQS ensures reliable message delivery and allows components to communicate asynchronously.

---

## SQS Queue Types

| Queue Type | Description | Throughput | Delivery |
|-----------|------------|-----------|---------|
| **Standard Queue** | High-throughput queue, best-effort ordering | Unlimited messages per second | At-least-once delivery (duplicates possible) |
| **FIFO Queue** | First-In-First-Out queue, exactly-once processing | 300 messages/sec without batching, 3,000 messages/sec with batching | Exactly-once delivery, preserves order |

---

## Message Features

| Feature | Description |
|--------|------------|
| **Message Delay** | Postpone delivery of new messages (0–15 minutes). Can be applied to the entire queue or individual messages. |
| **Visibility Timeout** | After a message is received, it becomes invisible to other consumers for a specified duration to prevent double processing. |
| **Dead-Letter Queue (DLQ)** | Stores messages that could not be processed successfully after a configurable number of attempts. |
| **Message Retention Period** | Messages can be retained in the queue from 1 minute up to 14 days. |
| **Message Batching** | Send, receive, or delete up to **10 messages per batch** to improve throughput and reduce cost. |
| **Long Polling** | Consumers can wait for messages to arrive instead of returning immediately, reducing empty responses and API calls. |
| **Encryption (SSE)** | Messages can be encrypted at rest using server-side encryption (SSE-SQS) or AWS KMS keys. |

---

## Throughput & Limits

| Queue Type | Maximum Throughput | Notes |
|-----------|-----------------|------|
| Standard Queue | Unlimited | Can scale horizontally; duplicates possible |
| FIFO Queue | 300 messages/sec per queue (without batching) | With **batch operations**, up to 3,000 messages/sec per queue |
| Batch Operations | 10 messages per API call | Applies to SendMessageBatch, ReceiveMessage, DeleteMessageBatch |

---

## Typical Use Cases

- Decoupling microservices or application components
- Offloading background processing tasks
- Handling asynchronous workflows
- Buffering requests to ensure smooth traffic spikes
- Handling retries and failures via Dead-Letter Queues

---

## Key Points

1. **Standard Queues** are ideal for **high-throughput, unordered messages**.
2. **FIFO Queues** are ideal when **message order and exactly-once processing** are critical.
3. **Dead-Letter Queues** improve reliability by isolating unprocessed messages.
4. **Batch operations** increase efficiency by reducing the number of API calls.
5. **Long polling** reduces costs by decreasing empty responses.
6. Maximum message retention is **14 days**.
7. Maximum message size is **256 KB per message**.

---

## References

- [Amazon SQS Documentation](https://docs.aws.amazon.com/sqs/)
- [Amazon SQS FAQs](https://aws.amazon.com/sqs/faqs/)

