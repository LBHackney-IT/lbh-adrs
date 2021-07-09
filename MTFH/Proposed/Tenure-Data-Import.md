# Tenure Data Import

### **Date:** 9th July 2021

### **Status:**  PROPOSED

## **Context**

The tenure data that currently exists in Universal Housing (UH) in the `tenagree` database table can be divided into two distinct categories:

- Master Tenure Record
  - These records represent true tenures - i.e. the association between a person and a property
- Everything else
  - These records are store in tenagree as this was a convenient location, however they are not a tenure as they do not have an association with a property - they are an "account" associated with a Master Tenure Record

For the purpose of viewing a tenure in Manage My Home, the only category that is relevant is a Master Tenure Record.

When importing the data as part of the the development of Manage My Home, we have considered two options:

1. Import the data as it is currently stored in UH, and mark anything that isn't a Master Tenure Record as a "child" of a Master Tenure Record
2. Import only Master Tenure Records
    - everything else stored in `tenagree` should be imported according to a separate data model

## **Decision**

**Import only Master Tenure Records**

This option has been chosen for a number of reasons:

1. we are importing legacy data and have the ability to correct any inconsistency at this point
2. If we import the data as-is, we will be persisting the incorrectly modelled data
3. if we import the data as-is, we will need to either:
   - display it when viewing the list of tenures associated with a person/property
   - hide it when viewing the list of tenures associated with a person/property, therefore increasing complexity of the delivered solution

## **Consequences**

There will be a need to import the data that is not categorized as a Master Tenure Record separately, but this will ensure that the tenure data that is imported is correct. For the non-imported data, this will be modelled at a Data meetup prior to import.