# Microfrontend Architecture

### **Date:** 30th March 2021

### **Status:** ACCEPTED

## **Context**

Hackney has got several frontends built by different teams and some of those are combined in a single application like Single-View using a “deep-link” approach. 
Although this allows the different teams to build and deploy their own applications independently, this approach has got several disadvantages:

- Bad User Experience due page reload when switching between applications
- Code duplication for common components like header, footer, navigation etc…
- Duplication of common libraries (react, redux etc)

A better way to integrate several small frontends in a single application is to use the micro- frontends approach. Micro-frontends offer the benefit of more independent teams, autonomous deployment and faster development.

There are several ways to adopt a micro frontends architecture,  generally speaking there is a micro frontend for each page in the application and a container application which:
- Renders common page elements such as headers and footers
- Manage common operations like authentication and navigation 
- Brings the various micro frontends in a single page and tells each micro frontend when and where to render itself

There are two options to define micro-frontends:

1. Horizontal

It allows multiple micro-frontends per page. It means we have to split a view in multiple parts, which may or may not be owned by the same team. The challenge is to ensure that these different parts have a cohesive look and feel. 

2. Vertical

It allows one micro-frontends per business domain and only one micro-fronted is loaded in a page. Adopting this approach means that we will look at the same problem from a business rather than a technical point of view. In this way, generally speaking, each business domain will be assigned to a team.


## **Decision**

**Define Micro-frontends using a Vertical splitting**

This splitting allows each team to become domain experts, despite how many views the domain is composed of, and can operate with full autonomy. 

By having vertical splitting, the best approach to composite the micro-frontends is using the client-side composition. With this pattern, a container app (an application shell) loads micro-frontends from a CDN (or directly from the origin), often with JavaScript or an HTML entry point of the micro-frontends.

The advantage of using vertical splitting and client-side composition is that frontend developers don’t need to learn a new thing to build micro-frontends, by having one micro-frontends loaded at time, it’s like building a SPA (Single Page Application). It does not heavily fragment the implementation, and each team truly owns a specific area of the application.


## **Consequences**

- A Shared component library is required to help to keep the visual consistency across micro-frontends
- Need to make sure that developers keep the communication between micro-frontends as little as possible. A common way is to use the routes.
- Frontends developers need to be aware how the logic used by container application to load and unload the different micro-frontends
