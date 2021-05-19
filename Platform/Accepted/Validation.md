# Standard for API request validation

### **Date:** 14th May 2021

### **Status:** ACCEPTED

## **Context**

Validation is an important function within APIs, to ensure that data that is submitted via API calls is properly checked to ensure it meets the requirements set by the business.

We will look at two options for validation:

- **Data Annotations**
  
  This involves "annotating" each class model with specific validation, such as

    - `[Required(ErrorMessage = "This field is required")]`

    - `[MaxLength(20)]`

  There are a number of issues with this approach:
    - Validation is scattered throughout the codebase as attributes on data model classes
    - Testing is not straightforward
    - Error messages are part of the compiled code and it is not possible to decouple this, e.g. to allow for customisable error messages
    - Does not allow for conditional validation

- **Fluent Validation**
 
  Fluent Validation solves a number of the issues that DataAnnotation cannot be solved by Data Annotations. It:
    - Is easy to configure with minimal, unintrusive setup in `Startup.cs`
    - Lives outside of the data model classes
    - Very easy to test validation in isolation
    - Error messaging can be externalised using dependency injection
    - Allows for chaining of validators and conditional validation
    - Has a lot of built-in validation already (*if **x** exists, then **y** must also exist*)

## **Decision**

**Fluent Validation**

Fluent Valdation is widely used, offers a lot of flexibility and allows for a clean, customisable and testable approach to validation.


## **Further details**

- [Data Annotations](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/validation?view=aspnetcore-5.0)

- [Fluent Validation ](https://fluentvalidation.net/)

## **Consequences**

By using Fluent Validation, API requests can be validated in one place, instead of having validation defined on attributes in classes that are spread around the code base. It also means that testing of validation can be vastly simplified, and validation messaging can be externalised from the codebase.