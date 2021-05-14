# Event Driven Architecture - Message Types

### **Date:** 14th May 2021

### **Status:** PROPOSED

## **Context**

Alongside the decision to adopt an event driven architecture, there is a need to define what an event will look like. There are two options for events:

- **Thin Events**

  A thin event consists of the minimum amount of data that is required that will allow a subscriber to retrieve everything it needs. This normally consists of an ID with which to make an API call back to the source publisher to gather the data it needs.

  The benefits of thin events are:
  - The payload is small in size
  - Data is always up to date as it is retrieved at the point of consumption
  - If calls to APIs fail due to unavailability of APIs, the message can be replayed
  - Very little need for event versioning

  The downsides are:
  - Consumers need to make API calls to gather the data they need

- **Fat Events**

  A fat event contains all the data necessary for any subscriber to be able to perform its job. 

  The benefits of fat events are:

  - all the data needed for consumer processing is present in the event
  - no need to make any API calls to retrieve data

  The downsides are:

  - Event payload could grow to be quite big
  - Data present in the payload may no longer be required by any consumer
  - It is difficult to version events easily (and multiple versions of the same event may need to be sent for backwards compatibility)

## **Decision**

**Thin Events**

The easiest solution is to use thin events, with consumers gathering the data they need using API calls. This has the benefit of reducing the need for event versioning, and event payloads that grow in size over time.

## **Consequences**

Using thin events means that it is up to the consumer to retrieve the data it needs to perform its task. Whilst this will lead to additional calls to APIs to gather data, it means that
- data will always be up to date
- event payload size is kept small
- not much need to create new versions of events