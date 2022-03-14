# Indexing Transaction and account data into elastic index after data migration

### **Date:** 14th March 2022

### **Status:** PROPOSED

## **Context**

The indexing of Transactions and Accounts data is a functional requirement with regards to providing data for the All Transactions UI screen.
Currently the indexing is performed by the Housing-Listener which handles new Transaction created events and indexes them in Elastic Search.

However this process is very slow and is proceeding at a rate of approx 500,000/day.

| Environment | Number of Transaction Items | Time to Index|
|-------------|-----------------------------|--------------|
| Dev/Staging | 3 million | 6 days |
| Production | 100+ million | 20 days |

The reason for this low velocity is the reliance of the FIFO mechanism of the SQS, the need to raise an event for each record indexed.

This problem can be resolved by bypassing the need to rely on the SQS messaging infrastructure.
We propose that we separate the bulk index lambda function, and position it in the housing domain.
The reason for preparing this lambda in the housing domain is because of the Elastic non distributable node.

## **Decision**

By creating a new lambda function in the housing domain to bulk index the Transactions and Accounts data from IFS into Elastic index,
overcomes the timeout issue by utilising.
The step function will be needed to handle lambda execution.

## **Further details**

The step function consists of two lambda functions to index Transactions and Accounts, with the ability to overcome the timeout issue as well.
This lambda function will pick data from IFS and index them into the Elastic index.

## **Consequences**
With these changes we will have a high speed possibility to index all Transactions and Accounts migrated data.