
# Change SQS architecture setup to ensure no data loss

### **Date:** 3rd August 2022

### **Status:** PROPOSED

## **Context**

As part of our EDA infrastructure setup, we utilise AWS SNS and AWS SQS. Each consumer (listener), has its own dedicated SQS queue that is subscribed to one or multiple SNS topics. All SQS queues also have an associated dead-letter queue. However, currently there are no established procedures around monitoring, alerting and ensuring there is no data loss for any published messages. 
Several areas of concerns have been identified: 
- No delivery status logging for SNS topics to its consumers.(Raised as a separate proposed ADR)
- False positives (messages for event types we ignore on purpose) end up in the dead-letter queues. 
- The dead-letter queues are currently not monitored so messages get lost after 4 days (default retention period) resulting in potentially our services becoming out-of-sync. 

The current listener setup includes a switch statement that determines which messages (based on event type) should be processed. If an event type should not be processed, it triggers the ‘default’ switch statement case, which throws an exception. The exception thrown at the end of the Lambda function execution results in SQS considering the message as ‘failed to process’. This has the following implications:
- As the SQS queue type we use in FIFO, the message is retried a total of 3 times, and any new messages are placed in a queue until the retries have finished and the message is moved to the dead-letter queue. This is a delay that should be avoided as the message was intentionally ‘ignored’ so it should not be retried. 
- The dead-letter queue contains multiple messages, most of which are ones that are intentionally ignored and should not be moved to the dead-letter queue as they are not needed. A cluttered dead-letter queue makes any investigation of genuine failures more difficult. 

## **DECISION**

Diagram detailing the proposed architecture can be found [here](https://lucid.app/lucidchart/856b9081-2492-4ae3-bdb4-89e4e48cdea8/edit?viewport_loc=71%2C-275%2C3208%2C1557%2C0_0&invitationId=inv_defd6334-37ce-477b-bad3-0449fa2c5c35#).

**Amend existing listeners and base-listener template.**

The main switch statement determines the event type and use case of the message being processed, and which messages should be ignored. The ‘default’ case should be amended to no longer throw an exception, but instead just log that a message will not be processed due to event type. This will ensure the successful execution of the Lambda function and result in SQS deleting the message from the queue as it will be marked as processed. 

**Add additional logging and alerting**

As a result of the above proposed change to listeners, the dead-letter queues will only contain messages, which genuinely failed to be processed. An alarm should be created to send a notification in the event that a message is placed in a dead-letter queue so an engineer can investigate.

The order of messages is important as a data change to a record might no longer be relevant (e.g. if a subsequent change was made). The person investigating would need to manually assess if the change described in the body of the message in the dead-letter queue is still applicable - if it is, the data record should be updated. The investigation needs to conclude by manually deleting the message from the dead-letter queue to prevent any further alarms for items that have already been reviewed. 

In the future, this process would be iterated to introduce automation where appropriate.

**Further mechanisms to ensure no data loss**

Existing SQS queues are set up with a retention period of 4 days (default). This can be increased to a maximum of 14 days. As there might be occasions when a message in a dead-letter queue cannot be reviewed within the retention period (for example, the people supporting the application are on long annual leave or a notification gets missed), an additional mechanism needs to be put in place to ensure no data loss for any messages that are not successfully processed. 

Implementation steps to ensure no data loss:
- CloudWatch alarm triggered every 3 days that monitors if there are any messages present in a given dead-letter queue.
- If there is one or more messages, send an SNS notification which performs two actions - 
    1) notifies the relevant team providing support for that part of the system 
    2) triggers a Lambda function
- The Lambda function collects all messages from the dead-letter queue, stores them in S3 and purges the queue so the number of messages in the queue always reflects the number of new messages that failed to be processed. 

## **Consequences**

1. Amending the existing listeners and base-listener template would ensure that no messages that are intentionally ignored are moved to the dead-letter queue, improving visibility over genuine cases of failures.
2. Alerting when new messages are added to the dead-letter queue would provide the team members supporting that part of the system with a notification that something has gone wrong so they can ensure
    1. Changes are applied to records (if still applicable) so our systems remain in sync
    2. Issues with the listener setup are identified early and resolved.
3. Implementing further mechanisms would ensure no data loss by storing any messages that were not successfully processed in a long-term storage service so engineers can investigate and have the data available to them even at a later stage.   
