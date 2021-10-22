# Workflow Processes

### **Date:** 22/10/2021

### **Status:** DRAFT

## **Context**

There is a need at Hackney to be able to define workflow processes that will enforce an order of steps that a Housing Officer/Staff Member will need to take in order to complete that process.

The options for managing and maintaining workflow processes vary in complexity, from:

### Off-the-shelf workflow solution

  - Provides the option to build complex, fully customisable workflows
  - there will be a need to provide training on the use of this software
  - there will be a need for someone with technical expertise to support and maintain it
  - there will most likely be one-off or ongoing fees around licencing and support
  - more difficulty integrating this into MMH

### Build it ourselves

  - can use the existing Playbook API as a foundation, along with all the knowledge and documentation around that
  - will integrate nicely with MMH and other workstreams
  - can use existing in-house developer talent to build and maintain this

**Build our own state machine**

  - will require perhaps complex engineering challenge to build and support this

**Use Stateless**

  - already tackled the engineering effort to develop a state machine
  - very extensible, as it is just the framework for a state machine
  - integrates nicely with external data stores
  - documentation exists online

### Decision

**Build it ourselves - Use Stateless**

Implement an API that uses the Stateless nuget package in order to manage process state, therefore saving the need to build a bespoke state machine.

## **Further details**

- [Stateless - Project Overview](https://nblumhardt.com/2016/11/stateless-30/)
- [Stateless - Github Repository](https://github.com/dotnet-state-machine/stateless)
- [Stateless - Community Support from Scott Hanselman](https://www.hanselman.com/blog/stateless-30-a-state-machine-library-for-net-core)

## **Consequences**

By favouring building our own process API and underlying process engine, we can start with a simple implementation and introduce complexity using the in-house knowledge and experience of Hackney developers. By building it on top of the existing Playbook API, we can adhere to the pre-existing architectural principles in place, and for which there is already extensive documentation.