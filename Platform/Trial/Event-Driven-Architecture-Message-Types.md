# Event Driven Architecture - Message Types

### **Date:** 14th May 2021

### **Status:** TRIAL

## **Context**

Alongside the decision to adopt an event driven architecture, there is a need to define what an event will look like. There are several options for events:

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

**Hybrid Approach**

Ideally, we should use thin events wherever possible,as this reduces the complexity around sharing events, updating consumers with new versions of events, etc. However, there are some instances where a thin event might not be possible - notably when updating an activity audit log with details of what has changed. Therefore, the best solution would be to have a thin event that contains an optional message body to hold a specific payload.

## **Decision**

**Hybrid Approach** 

The easiest solution is to use a hybrid approach, with consumers gathering the data they need using API calls. This has the benefit of reducing the need for event versioning, and event payloads that grow in size over time.

The event payload will be:

```
{
  "id": "8e648f3d-9556-4896-8400-211cb1c5451b",
  "eventType": "personCreated",
  "sourceDomain": "person",
  "sourceSystem": "personAPI",
  "version": "v1",
  "correlationId": "f4d541d0-7c07-4524-8296-2d0d50cb58f4",
  "dateTime": "2021-05-17T11:59:57.25Z",
  "user": {
    "id": "ac703d87-c100-40ec-90a0-dabf183e7377",
    "name": "Joe Bloggs",
    "email": "joe.bloggs@hackney.gov.uk"
  },
  "entityId": "45c76564-2e38-48f3-bb31-6bab2fef8623",
  "eventBody": {
    "oldData": {
      "optionalProperty1": "Property value",
      "optionalProperty2": "Property value",
      "optionalProperty3": "Property value"
    }
  }
}
```

## **Consequences**

Using a hybrid approach means that it is up to the consumer to retrieve the data it needs to perform its task, but additional data can be sent in the event body if necessary. Whilst this will lead to additional calls to APIs to gather data, it means that:
- data will always be up to date
- event payload size is kept small
- less of a necessity to create new versions of events
- it is stil possible to provide additional data in the payload if necessary