# Manage Arrears Application Scope

### **Date:** 26th April 2022

### **Status:** Proposed

## **Context**

The Manage Arrears application (MAA) is used by the Manage Arrears team to help manage working with tenants in arrears. This includes the ability to print out rent statements to provide to these tenants.

There has arisen a new use case to be able to print rent statements for any tenant, whether or not they are in arrears. Tenants often attend the service centre requesting these statements. As there is currently no readily available service to do this, it has been recommended to broaden the scope of MAA to include providing rent arrears for tenant regardless of whether or not they are in arrears.

This effectively changes the scope of the application beyond providing data for arrears cases but it would reduce the overhead involved in the current manual methods of providing statements.

The current options available:

- Continue as is. This would mean continued difficulty in generating statements for residents but does not change the scope of the application.

- Allow rent statements to be produced for non-arrears cases. This would mean easier production of statements but means the scope of the application would change. It would also mean the application accessing data on cases that are not arrears cases.

A supporting message from a Manage Arrears team manager:

> Happy to clarify that we need to generate statements for all rent accounts wether they are, or have been in credit.

> This is a routine service request from tenants in person at the HSC, over the phone and via email. This is met not only by Income Officers, but Agents in our Call Center and Customer Services teams who have access to MAA.
> Many thanks

> Ian Clark

> Team Manager

## **Decision**

**Extend the scope of the MAA application to include printing rent statements for accounts not in arrears**

This option has been chosen for a number of reasons:

1. It reduces the overhead of having to manually produce rent statements for residents
2. It has been advised that there is little or no risk involved in extending the scope of the application this way

## **Consequences**

This approach does mean that data not covered in the initial scope of the application will now be available. However, it has been advised that the risk involved is negligible.
