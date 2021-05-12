# "First Choice" Datastore for a Microservice

### **Date:** 30th March 2021

### **Status:** ACCEPTED

## **Context**

Although each microservice should decide the most appropriate datastore, generally speaking, it would be good to choose a “first choice” datastore. There are two options considered in Hackney:

1. AWS PostgreSQL (relational database)
2. AWS DynamoDB  (NoSQL/Document database)


## **Decision**

**AWS DynamoDB**

DynamoDB is the fully managed NoSQL db in AWS. A NoSQL database is the “first choice” of datastore for the following reasons:
- A microservice is quite small so that the majority of the times we do not need complicated relations models.
- NoSQL has been designed for horizontal scalability.
- For most cases APIs don’t need to support a lot of queries on different entity’s properties.


## **Consequences**

- Speed up the development as developers do not need to worry about DB migration and object-relational impedance mismatch  
- Small learning curve for Hackney developers who get used to work with SQL databases 
