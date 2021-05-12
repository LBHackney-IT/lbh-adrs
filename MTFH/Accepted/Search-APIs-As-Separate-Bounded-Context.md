# Search APIs as separate Bounded Context

### **Date:** 30th March 2021

### **Status:**  ACCEPTED

## **Context**

There are use cases for advanced search functionalities (free text search, partial matching, wildcards, filters etc) against entities from different bounded contexts (e.g. search people, assets, tenures etc). For this kind of functionality querying a classic database (either SQL or NoSQL) does not fit the purpose, instead a search index is more appropriate. AWS ElasticSearch is the technology of choice as a search index for Hackney. AWS CloudSearch has been excluded as it’s not actively supported by the AWS team, plus it’s not available in the London Region.

Two options have been considered:

1. Search endpoint inside each domain bounded context

With this option the bounded context would have its database for the classic CRUD operations and its search index where to retrieve the list of entities via the advanced search functionalities provided by ElasticSearch. This option is more appropriate if it’s required to search the same fields or a subset of fields from the domain entities.

2. Searches bounded context exposing a search endpoint per each domain

With this option the domain bounded context does not have the search index but a search endpoint, with its index, is being added to the “search bounded context”. In this way the searches bounded context has got a single AWS Elastic Search with multiple indexes, one per domain which requires search functionality.  This option is more appropriate if it’s required to search also for fields that are related to the domain entity but those belong to different bounded contexts. 

## **Decision**

**Searches bounded context exposing a search endpoint per each domain**

This option has been chosen because the search for person and assets requires to search/filter/display data which belongs to other domains. For example, when searching for a person it’s required to filter per tenure type, display some tenure information, display assets full address etc. So, some of the person related fields do not belong to the Person platform API which is supposed to be used across the entire Hackney and not just for housing. Instead, with a separate search service we can aggregate related fields from different bounded contexts.

The search indexes will be kept in sync via events raised by the relevant platform APIs.

In this way we can have a search for “Housing domain” which allows to search for a person by augmenting the entity with housing related information, things like asset address, tenures etc. With the same logic, in the future we might have another search service for “social care domain” where the person can be augmented with social care related fields. Both searches will have the same Person platform API as one of master data. 

## **Consequences**

- Cheapest infrastructure option as we do not need to create an AWS ElasticSearch cluster for each domain. 

- At first glance you might not expect to have a search as standalone bounded context. But at the same time this search service is commonly being used as an aggregator and it is not against any principle of a microservices architecture.
