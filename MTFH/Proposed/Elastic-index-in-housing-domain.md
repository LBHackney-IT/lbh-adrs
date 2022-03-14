# Indexing Transaction and account data into elastic index after data migration

### **Date:** 14th March 2022

### **Status:** PROPOSED

## **Context**

It is needed to index all migrated data about Transactions and Accounts into an elastic index with regards to the requirement in some screens such as All Transaction screen. Currently the indexing is run by a housing-listener, this listener listens to new transactions inserted and will index it into the Elastic index. 

for this purpose there are two solution
1. Sending message to housing-listener one by one after migration
The problem in this solution is, it is not reliable because of the low velocity and FIFO mechanism of the SQS.
2. Using the bulk index lambda function in the housing domain, the reason for preparing this lambda in the housing domain is because of the Elastic non distributable node.

## **Decision**

preparing a possibility to define new lambda function in housing domain to bulk index the Transactions and Accounts data from IFS into Elastic index, this function pick bunch of records from IFS in a loop and index them into ES and for overcome on the timeout issue the step function will be needed to handle lambda execution.

## **Further details**

The step function consists of two lambda functions to index Transactions and Accounts, with the ability to overcome the timeout issue as well.
This lambda function will pick data from IFS and index them into the Elastic index.

## **Consequences**
With these changes we will have a high speed possibility to index all Transactions and Accounts migrated data.