# Feature Toggling

### **Date:** <*ADR DATE HERE*>

### **Status:** DRAFT

## **Context**

In order to be able to release code safely as part of a CI/CD pipeline, it is necessary to have a feature management capability to externalise the ability to enable features.

This can be achieved by including feature toggles in code, and by versioning APIs to account for breaking changes.

The options available are:

- Environment Variables
- AWS AppConfig

## **Decision**

**AWS AppConfig**

What is the change that we're proposing and/or doing?

## **Further details**

Links to evaluations, previously done work, spike documentation, etc. 

## **Consequences**

What becomes easier or more difficult to do because of this change?