# Finance Compnent Library

### **Date:** 15th February 2022

### **Status:** ACCEPTED (To be presented to a TDA session to look at highlighted consequences)

## **Context**

The Finance component library is a set of reusable components in the finance domain with the intent to reuse these when building out the new Manage Arrears application.

The component library can be located [here](https://61f0267fc8e670003a291a88-urjhjsfyvz.chromatic.com/?path=/story/blocks-headerblock--normal)

Two questions hav arisen regarding continued development of this component library.

- Whether or not we want to model Manage Arrears after the Final Finance Solution or with the wider design being utilised in the micro frontend space.
- Whether or not we want to maintain a separate component library from the main Hackney Design System

If we continue to build out finance with the current design it will continue to evolve with a design different to anything build via the Micro Frontend boiler plate, the general standard for new Frontend projects moving forward.

The finance component library uses the Hackney Design system but extends it to provide some service specific components such as React implementations of the Button and Alert elements. It also introduces some components build specifically for the Next.js framework such as the Left Menu that contains more specific behaviour implementation. This prompted the separate library with a view to extend this to Manage Arrears.

## **Decision**

**Continue to Model Manage Arrears after the Finance Solution Design**

This option has been chosen for a number of reasons:

1. So that the application can continue to be built at pace in the Next.js framework
2. So users in the finance space have a consistent representation of the applications they use

**Complete the Finance Component Library**

This option has been chosen for a number of reasons:

1. The use of shared components will mean a consistent design in Manage Arrears
2. This will also mean faster development once the library is in place

## **Consequences**

This approach does mean an amount of technical debt could be incurred

- At an appropriate time and with stakeholder agreement the design of the Finance applications could be updated to align more closely with the design utilised by the Micro Frontends for more global consistency
- If the Finance applications are updated there could also be opportunity to update and merge the Finance component library with the Hackney Design system
