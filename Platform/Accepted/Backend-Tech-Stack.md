#  Backend Tech Stack

### **Date:** 30th March 2021

### **Status:** ACCEPTED

## **Context**

Hackney has several backend applications using different programming languages and frameworks:

- Ruby 
- Node Js
- .NET Core/C#

Although in a microservices architecture each service can be developed with its own programming language and framework, unless a service requires a particular programming language, it’s still good to agree on a common language/framework for mtfh-T&L workstream. This will make it easier to shift developers from different teams/services. 

## **Decision**

**.NET Core 3.1/C#**

.NET Core/C# is the most common framework/language used for the majority of Hackney APIs, it’s the programming language that Hackney developers are more familiar with, plus it has got the following well known advantages:

- It’s open source
- It’s cross platform
- - It’s the top-performing web frameworks
- C# is the most versatile programming language

We are not using .NET 5.0 as .NET 3.1 is the latest LTS version of .NET core supported by AWS Lambda. 

## **Consequences**

It’s not possible to reuse code/modules from application written in a different framework than .NET C#.
