# Frontend Tech Stack

### **Date:** 30th March 2021

### **Status:** ACCEPTED

## **Context**

Hackney has several frontends applications using different programming languages and frameworks:

- Angualr/JS
- Ruby
- React/TS

Hackney has got a  microservices architecture exposing APIs to be used by different consumers and so each frontend can be developed with its own programming language and framework. Anyway, unless an application requires a particular programming language, it’s still good to agree on a common language/framework for mtfh-T&L workstream. This will make it easier to shift developers from different teams. 

## **Decision**

**React with TS (Type Script)**

React with TypeScript is the most common framework/language used for the majority of Hackney frontend application, it’s the programming language that Hackney frontend developers are more familiar with, plus it has got the following well known advantages:

- It’s open source
- Easy to learn because of its simple design and less time worrying about the framework specific code
- Better user experience and very fast performance compared with other FE frameworks

## **Consequences**

No consequences as all frontends of each team are more familiar with React, plus, before the Cyber attack, Hackney started to migrate in React the old Angular and Ruby apps.