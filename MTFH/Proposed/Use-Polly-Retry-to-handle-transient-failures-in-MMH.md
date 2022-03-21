# Use Polly Retry to handle transient failures of API calls in MMH listeners

### **Date:** 21st of March, 2022

### **Status:**  Proposed

## **Context**

Manage my Home has successfully implemented an EDA approach to handle updates to related entities. As part of this approach, Listeners (Lambda functions responsible for capturing and processing events and updating the relevant data entity with any data changes)
use API calls to retrieve data from data sources, where the data needed does not belong to the data domain, which the Listener is responsible for. For example, the Person Listener will use an API call to retrieve Tenure information for any changes to a tenure, 
but will use a direct DynamoDB query to retrieve/update Person data.

As the Listeners perform API calls, a proposal to start using a librabry called 'Polly 'for retrying API calls has been made. We have completed a succesfull [spike](https://docs.google.com/document/d/1gLwslJdTZKBGn-uZcWDsxNOkV6ucO051CacIJQTnaks/edit) to test out the functionality.

The benefit of using such approach is that it will retry API calls in Listeners, providing more resiliance to our architectural approach of handling events.

Polly is the recommended by Microsoft .NET library for handling retries. 

## **Decision**

**Start using Polly Retry for MMH listeners**

Start using the Polly .NET library to do API retries in case of a failure for improved resiliance.

This will only be used for GET API requests. 

## **Consequences**

- This change is a technical improvement, rather than introducing a new feature.
- It will introduce resilience and transient-fault handling capabilities into our Listeners.



