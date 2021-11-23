# Create consolidated Google group for managing MMH access

### **Date:** 9th November, 2021

### **Status:**  In-Trial

## **Context**

Currently, access to Manage my Home (MMH) service is managed by Google groups - people are added to a Group related to their Hackney team (e.g. income-team) and the Google group is added to the list of allowed groups for accessing both the MMH frontend and backend APIs.

The number of allowed Google groups is continuously increasing and is introducing additional complexity and maintenance overhead for the following areas:

1. Front end - each time a new Google group needs to be allowed to access the application, the front end needs to be redeployed as it maintains the list of allowed groups in an environment variable. This introduces delays and adds complexity to the process.

2. Back end APIs - similarly to the front end, each time a new Google group needs to be added to the list of groups allowed to access the service, a DynamoDB table needs to be updated to allow the front end to invoke our APIs. The API authentication mechanism is user-based and the logged in user must belong to at least one group allowed to access the API.

3. We are at a stage with an increasing need for permissions management within the service. Currently people who can access the service are allowed to perform all actions that it implements. Having a high number of groups will make future permissions management more complicated and will require continuous maintenance.

The following solution is considered to remove the issues described above:

Create several Google groups specifically for accessing MMH. The number of groups is dependent on the number of roles identified within the service. For example, there might be three groups:
mmh-officer-access
mmh-manager-access
mmh-read-only-access

By introducing Google groups specific to the service, future access will be provided by adding individuals to the Google group relevant to the type of access they should have. 

Management of those Google groups will be given to the PO and the project team, thus reducing the dependency on other Hackney staff members, external to the project team, to add individuals to team-specific Google groups. Moreover, this will provide greater visibility over who is part of the given Google group and provide better control over managing access to the service.
This approach is already successfully followed in other projects in Hackney like Repairs.

## **Decision**

**Introduce project specific Google groups for accessing the service**

This option has been chosen because it will reduce the maintenance of access to the MMH service by introducing the following benefits:

1. Front end will not need to be redeployed each time a new individual needs to be given access to MMH as the individual will be added to a Google group already recognised by the front end configuration as allowed to access.

2. Back-end APIs access will not need to be updated frequently for the same reason described in point one.

3. Role-based Google groups reduce the complexity of any permissions management solution that will be based around Google groups as there will be less Google groups mapping to roles to maintain. 

## **Consequences**

- Easier access management and possibly less complex permissions management solution to be implemented.

- The need to redeploy the front end and update the APIs access mapping frequently will be removed. 


