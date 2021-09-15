#  Integration with MMH-derived services (Assets API / Housing Search API)

### **Date:** 15th September 2021

### **Status:** ACCEPTED

## **Context**

The repairs context has evolved naturally in the context of Modern Tools for Housing, it's parent programme, as a separate discrete service. Since the cyber attack, this has been developed in isolation from the other MTFH services, sharing only a data store. The time has come to integrate Repairs with the services previously developed by Manage my Home (formerly Tenants & Leaseholders). We will be starting with two integration points:

1) The Housing Search API during the property search flow
2) The Assets API during the raise a repair flow

Two options for integration were discussed:

## Option 1: Integrate at the API level with Assets API and Housing Search API

Property search flow:
1. User enters search screen and enters prop address
2. API call to property search API
3. Displayed results as usual with lookup to work order history via propref
4. User selects the correct address + enters property details page
5. API call to Assets API with the ID of the clicked record to retrieve full property details

### Advantages:
- Aligned with Hackney’s current best practice
- The least effort out of the “do something” options
- Journey similar to existing journeys in T&L and Finance - use same API approach
- No data duplication
- Consolidating the search views is much easier to achieve in future
- Makes it easier if in the future we need additional property data as assets service is source of truth

### Disadvantages:
- Tightly coupled services (if Assets is down, RepairsHub is down)
- Driving further down the “API hops” route
- Lose out on EDA advantages for this integration specifically

### Variation Option 1a

Integrate NextJS frontend with the Property Search API and Assets API

### Advantages:
- Less hops
- Higher maint cost for dependant APIs
- Does not increase API hops

### Disadvantages:
- Far higher complexity in JS
- Not aligned with current future Hackney standards

## Option 2: Maintain local store of propref/tenure type and subscribe to tenure updates via event subscription hub

When an asset (that is a property) is updated, Assets Service will publish a message to the relevant SQS queue
Repairs Hub is subscribed to the queue and receives this notification
Repairs Hub maintains its own (subset of) property data
User requests call this local store

### Advantages:
- Loose coupling (Assets down = Repairs OK)
- Aligns with Hackney’s “vision” or future service design (EDA)
- Performance++ and Availability++
- Data is optimised for the view

### Disadvantages:
- Probably highest in terms of developer effort
- Not an absolute guarantee of data freshness (if Asset service is down)
- Redundancy of data over multiple services (out-of-sync)
- Pure replication of data
- Different approach to other workstreams
- More maintenance
- Stepping away from possible consolidation of services in the future (e.g. share same search / property page)

## **Decision**

Option 1a is the chosen approach - Housing Search API and Assets API will be called directly from the Frontend



## **Consequences**

Discussed in the advantages / disadvantages section above