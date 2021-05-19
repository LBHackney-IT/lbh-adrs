#  Event-Driven Architecture SNS + SQS

### **Date:** 8th April 2021

### **Status:** TRIAL

## **Context**

When the microservices need to interact with each other (e.g. to sync common data), the preferable approach is to adopt an event-driven architecture. This approach guarantees loosely coupled services which can be run and deployed in isolation.

In order to implement an events-driven architecture we need to use a message broker which should be responsible for sending the messages.

In AWS, two options can be considered:

1. SNS (Simple Notification Service)

SNS implements pub/sub messaging which allow applications to send messages to multiple subscribers through push mechanisms.

2. SNS (Simple Notification Service) + SQS (Simple Queue Service)

SQS is a message queue service used by distributed applications to exchange messages through a polling model and can be used to decouple sending and receiving components. Using Amazon SNS and Amazon SQS together, messages can be delivered to applications that require immediate notification of an event, and also persisted in an Amazon SQS queue for other applications to process at a later time.

## **Decision**

**SNS (Simple Notification Service) + SQS (Simple Queue Service)**

By coupling SQS with SNS, the subscriber can receive messages at "their peace". It allows subscribers to be offline, tolerant to network and host failures. Although SNS has got a 4-phase retry policy, the message can be lost if the consumer is not available. Instead if the subscriber uses a queue, we are able to achieve guaranteed delivery.

## **Consequences**

- SQS allows messages partition/grouping which is useful to scale out the consumers
- Using both SNS and SQS requires additional work when setting up the infrastructure.
- Additional infrastructure cost for SQS, although that will be negligible compared to the benefit that it can bring.