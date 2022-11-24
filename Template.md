# Housing Search API New Endpoints - Finance Domain Usage

### **Date:** 22 November 2021

### **Status:** DRAFT 

## **Context**

From a Housing Finance Domain perspective we need to apply service charges accross all the Properties which can be done by an Estate level or Block level.
In order to apply all the property charges Housing Finance is going to consume Housing Search API Asset Search Endpoint.

Currently in ES Search Query we are Querying by PageNumber*PageSize and then skipping the previous pages to get the required page number of search result. 
Elsastic Search has a limitation of fetching 10,000 records in one time, due to that if we use pagination also we cannot get Page Number 11 or more with fixed Page Size of 1000.
TO overcome this ES Search limitation we need to built one endpoint which will use some kind of different query criteria in Elastic Search (Search_after) to get results of page number which  comes more than 10,000 records and so on until the last page number.

Also we have a requirement to apply service charges to all the assets belongs to a Block, in order to apply the charges for all the child assets we need to fetch all the child assets based on a parent asset Id. In Order to fetch the child assets the Elastic Search Asset Index needs to be updated by the 'rootAsset' and 'parentAssetIds' from the AssetInformation API. 
## **Decision**

1.Building a new endpoint to fetch assets page by page until it finished with the last matching record:
/search/assets/all

2. Build a new endpoint to fetch all the child assets based on parentAssetId and asset type
  /search/assets/childassets?parentAssetId={parentAssetId}&assetType={assetType} 

## **Further details**
Search API Specification is modified with the changes highlighted in comments.
https://docs.google.com/document/d/1fSaz58qA5REdcqPiReh2yyeQYOueoFKekzivaVJVTdg/edit#

All Changes related to Assets can be found here
https://docs.google.com/document/d/1_4uV_kPfZkpxQbNjE_IXrbLZ3t-NCpA8/edit

## **Consequences**

For example, if we have a total matching search Record count is 27,000 fixed page size is 1000 it will be easier to fetch assets more than page number 10 and so on until the last page number.
We can also identify all the child assets related to a parent Asset Id.
