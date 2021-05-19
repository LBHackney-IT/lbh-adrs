# Microfrontends Compositions

### **Date:** 30/03/2021

### **Status:** ACCEPTED

## **Context**

According to the [Microfrontend-Architecture ADR](https://github.com/LBHackney-IT/lbh-adrs/blob/feature/create-ADRs/Platform/Accepted/Microfrontend-Architecture.md), we are going to define micro-frontends by using a vertical splitting. 

In this way each micro-frontends has got its repository and each team manages the development and the deployment of it.


About the client-side micro-frontends compositions, there are two options to implement that:

1. Writing our own boilerplate code

Each micro frontend is included in the html page using a \<script> tag and each of those applications exposes a global function as its entry-point. The container application determines which frontend should be mounted and calls the relevant function to tell a micro frontend when and where to render itself.

2. Using a framework as Single SPA (https://single-spa.js.org/ )

Single SPA is a Javascript framework for frontend microservices. In practice, it applies a lifecycle to every application. Each app can respond to url routing events and must know how to bootstrap, mount and unmount itself from the DOM.           


## **Decision**

**Using Single SPA framework**

This framework adopts the same principle as we would implement our own boilerplate code, but the advantage is that we don’t have to build and document our own logic but instead we use this light framework supported by an open source community.

The other advantage is that the framework allows to compose also applications written in Angualar and Vue.js


## **Consequences**

- Small learning curve for understanding how the single micro-frontends communicate with the container app. 