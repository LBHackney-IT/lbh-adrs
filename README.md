# Architecture Design Records (ADRs) - HackIT how to guide

## Context

Previously technical decisions were captured as part of spike documentation that was kept in project specific google drive. They were not open for other projects to review, adopt and adapt. Often, new developers were not aware of decisions as they were not aware of where to look for documentation. 

Going forward we agreed to create ADRs and add them to a single github repo[Link] to ensure that we have enough documentation, that all decisions are kept in the same location that is easy to find and to document how and why a decision was reached within a codebase. This will also achieve governance and uniformity around all projects. Also ADRs help to give context around the decisions that were taken so that we can revisit them. Other benefit of ADR will help in  improved onboarding for new developers, improved agility when handing over project ownership between external team to internal or vice version due to unseen circumstances or changed needs, and improved alignment across the teams regarding best practices

## Definition

An architecture decision record (ADR) is a document that captures an important architectural decision made along with its context and consequences of adopting the decision.

## User Needs

As a **developer** I need

- To understand the decision made so that I can proceed with code writing.
- - To easily find a history of decisions previously made so I can be aware of what has already been agreed and follow it

As a **solution** architect I need 

To have a summary of a technical decision agreed so that when people wish to review it, they can see the summarized outcome of a certain discussion and evaluations
- Links to further evaluations included in the ADR so I can read further information that I may need to understand a given technical decision

As a **Data architect** I need 

- To understand where I can find a list of technical decisions related to data so I can understand the reasoning behind a given implementation
- To have easy access to technical decisions regarding data so I can ensure I am following the established standards when working on data components 

As a **Cyber silver member** I need

- A summarized version of technical decisions being made so I can understand the reason behind them
- Technical documentation written in a clear and concise way so I can understand a technical decision even if I am not familiar with a given technology

As a **support engineer** I need

- Easy way to find documents outlining technical decisions so I can have a better understanding of a system implement it and use that knowledge to support the system more effectively

## When to write an ADR?

An ADR should be written whenever a decision of significant impact is made; it is up to each project team to align on what defines a significant impact. They can be:

1.   Backfilling a decision which was made previously.
2.   Proposing large changes to a solution/spike.
3.   Proposing no/small changes for a spike
4.   Proposing changes that differ from the overall agreed standard across our current ecosystem.

## How to start using ADRs

Decision identification:

- How urgent and how important is Architecture Decision?
- Design methods and practices can assist with decision identification and decision making.
- Ideally maintain a decision to-do list which aligns with the service to-do list.

Decision making:

- Group decision making via Community of practices or project team workshops to validate the findings can help in decision making.
- Better informed decision via ADRs which are available openly and people can collaborate on it.

Decision enactment and enforcement:

- ADs are used in software design; hence they have to be communicated to, and accepted by, various stakeholders of the services that fund, develop, consume and operate it.
- Architecturally evident coding styles and code reviews that focus on architectural concerns and decisions are two related practices.
- ADs also have to be (re-)considered when modernizing a software system in software evolution.

Decision sharing (optional):

- Many ADs recurring across various project development..
- Experiences from other projects and reusable components could enforce knowledge management strategy and contribute towards our emerging Community of practices meetups such as Data and Architecture etc.
- Dependency matrix evaluation.

## **Template for Lightweight ADRs**

In each ADR file, write these sections:

### **Title**

A short sentence to recap the decision taken.

### **Date**

### **Status**

What is the status, such as proposed, accepted, rejected, deprecated, superseded, etc.?

### **Context**

What is the issue that we're seeing that is motivating this decision or change?

### **Decision**

What is the change that we're proposing and/or doing?

### **Further details**

Links to evaluations, previously done work, spike documentation, etc. 

### **Consequences**

What becomes easier or more difficult to do because of this change?

## Revisit a past decision

If we revisit a past decision it needs to be a new ADR. This new ADR will supersede the previous decision.We will need to mark the relationship between the two ADRs:
- in the old ADR, indicates it’s superseded by the new one (add a link).
- in the new ADR, reference the old ADR that is superseded (add a link too)

## Appendix

| Acronym | Term                                    |
| ------- | --------------------------------------- |
| AD      | Architecture decision                   |
| ADL     | Architecture decision log               |
| ADR     | Architecture decision record            |
| AKM     | Architecture knowledge management       |
| ASR     | Architecturally-significant requirement |


Resource referred :

https://github.com/joelparkerhenderson/architecture_decision_record

