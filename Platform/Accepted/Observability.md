# Observability - Logging 

### **Date:** 30th March 2021

### **Status:** ACCEPTED

## **Context**

Providing rich logging information will make it easier to investigate issues without making use of intrusive approaches (i.e: debug, memory dump), also making visible the behaviour of services by using monitoring tools to extract and/or query these logs.

The idea is to utilize services offered by AWS as they are comprehensive and can operate at scale with minimal administrative overhead.


## **Decision**

**AWS x-ray**

AWS X-Ray is an AWS managed service that provides the functionality to debug and analyze distributed applications.


## **Further details**

[More on what X-Ray is](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html)

X-Ray provides an end-to-end request view - it will show you the full trace for an API invocation, including any other components/services it invokes.

The tool is used for identifying the root cause to an issue, discovering performance bottlenecks and seeing real-time data regarding high latency requests.

AWS X-Ray collects logs and makes use of a Service Map to visualize the dependencies and calls to other services made in an API request. 

X-Ray can be used to identify API requests. This is useful to identify if any of the implemented API endpoints is currently not monitored for availability.


## **Consequences**

- Provides a way to collect logs and metrics, for all services an API integrates with, including databases or other APIs.
- Visualizes the collected data for easy analysis of the data to help identify bottlenecks, error root causes and performance issues. 
- X-Ray needs additional C# configuration to capture metadata and trace downstream calls.
