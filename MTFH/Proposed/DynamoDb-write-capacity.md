# DynamoDB Write Capacity

### **Date:** 22th February 2022

### **Status:** PROPOSED

## **Context**

One write capacity unit represents one write per second for an item up to 1 KB in size. If you need to write an item that is larger than 1 KB, DynamoDB must consume additional write capacity units. Transactional write requests require 2 write capacity units to perform one write per second for items up to 1 KB. The total number of write capacity units required depends on the item size. For example, if your item size is 2 KB, you require 2 write capacity units to sustain one write request per second or 4 write capacity units for a transactional write request.


for the data migration purposes since there are a lot of records per entity, we need to increase the write capacity unit to increase the migration speed

for now for 50 write capacity the speed is 25 records per seconds

the main problem is in the development, production and staging environments which we have over than 100,000,000 records for transactions which will take a lot of time to migrate the all data,

by write capacity unit = 1000 the speed would be 250 recs per seconds in data migration and it would be 4.6 days' in average to migrate all transactions into the Production or staging environment

## **Decision**

Due to this restriction the main decision is increasing the write and read capacities for the mentioned entities below because of speed requirement in data migration and charges apportionment for estimate and actuals.

**Entities that needed to have change in write capacity**

1. Charges Entity
2. Accounts Entity
3. Transactions Entity

**Proposed write capacity units for Dev, Staging and Prod environments**

1. 500 for Charges Entity
2. 500 for Accounts Entity
3. 1000 for Transactions Entity for development
4. 2000 for Transactions Entity for Staging and Production

**Entities that needed to have change in read capacity**

1. Charges Entity

**Proposed reade capacity units for Dev, Staging and Prod environments**
For head of charges apportionment purposes we need to read all charges data then the read capacity unit should be increased here as well.

1. 500 for Charges Entity

## **Further details**

| Entity Name | Environment | Number of records | Max Row Size(Byte) | Proposed New Write Capacity | Proposed New Read Capacity 
| Number or records/second after the change | Estimated Time to move all the records from IFS -> FFS | The required AWS package size for each proposed change
|------|------|---------|---------|------|------|---------|---------|---------|
| Charges | Dev | 60,000 | 201 B | 500 | 500 | 500/Sec | 2m | ? |

## **Consequences**
Due to some cost according to these increases, it is possible to decrease these parameters after migration and development, and re-enable them when need is required, despite the apportionment requirement can happen 1 or 2 times in years.