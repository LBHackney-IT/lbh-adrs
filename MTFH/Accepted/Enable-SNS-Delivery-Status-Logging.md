
# Enable SNS delivery status logging for EDA implementations

### **Date:** 3rd August 2022

### **Status:** ACCEPTED

## **Context**

As part of our EDA infrastructure setup, we utilise AWS SNS and AWS SQS. Each consumer (listener), has its own dedicated SQS queue that is subscribed to one or multiple SNS topics. All SQS queues also have an associated dead-letter queue. However, currently there are no established procedures around monitoring, alerting and ensuring there is no data loss for any published messages. 
Several areas of concerns have been identified: 
- No delivery status logging for SNS topics so its consumers.
- False positives (messages for event types we ignore on purpose) end up in the dead-letter queues. (Will be raised as a separate proposed ADR)
- The dead-letter queues are currently not monitored so messages get lost after 4 days (default retention period) resulting in potentially our services becoming out-of-sync. (Will be raised as a separate proposed ADR)

## **Decision**

**Enable SNS delivery status logging and create an alarm for each failed delivery.**

Diagram of the proposed architecture can be found [here](https://lucid.app/lucidchart/856b9081-2492-4ae3-bdb4-89e4e48cdea8/edit?viewport_loc=71%2C-275%2C3208%2C1557%2C0_0&invitationId=inv_defd6334-37ce-477b-bad3-0449fa2c5c35).

AWS SNS provides a feature that logs the status of the delivery to each consumer of a given SNS topic per message. 

The feature by default logs all failed deliveries and has a configurable rate of success deliveries to log. Example values include recording 0%, 50% or 100% of successful deliveries. 

By enabling the feature, in an event of an issue, developers will be able to investigate if the consuming SQS queue ever received the message from SNS and thus be able to narrow down the investigation to SNS configuration or to the Lambda function code (listener) that is triggered each time SQS receives a message.

In addition, an alarm should be set up to periodically monitor if any logs with the pattern ‘FAILURE’ have been recorded in the log group, indicating that a message has failed to be delivered. 

## **Consequences**

Enabling SNS delivery status logging would help developers investigate and narrow down the cause to a failed event. This will be especially useful for cases when the listeners have not captured any logs of processing the message. 
