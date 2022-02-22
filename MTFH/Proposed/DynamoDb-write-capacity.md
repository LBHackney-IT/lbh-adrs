# DynamoDB Write Capacity

### **Date:** 22th February 2022

### **Status:** PROPOSED

## **Context**

One write capacity unit represents one write per second for an item up to 1 KB in size. If you need to write an item that is larger than 1 KB, DynamoDB must consume additional write capacity units. Transactional write requests require 2 write capacity units to perform one write per second for items up to 1 KB. The total number of write capacity units required depends on the item size. For example, if your item size is 2 KB, you require 2 write capacity units to sustain one write request per second or 4 write capacity units for a transactional write request.


for the data migration purpose since there are a lot of records per entity, we need to increase the write capacity unit to increase the migration speed

for now for 50 write capacity the speed is 25 records per seconds

the main problem is in the production and staging environment which we have over than 100,000,000 records for transactions which will take a lot of time to migrate the all data,

by write capacity unit = 1000 the speed would be 250 recs per seconds in data migration and it would be 4.6 days' in average to migrat all transactions into the Production or staging environment

**Entities which needed to have change in write capacity**

1. Charges Entity
2. Accounts Entity
3. Transactions Entity

**Proposed write capacity units for Dev, Staging and Prod environments**

1. 500 for Charges Entity
2. 500 for Accounts Entity
3. 2000 for Transactions Entity

