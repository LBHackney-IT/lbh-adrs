# WAF Implementation

### **Date:** 2nd March 2022

### **Status:** PROPOSED

## **Context**

As we have now embraced a cloud-based infrastructure, one of the challenges that we face is ensuring that it is as secure and as robust as possible. Because of this we are continually exploring ways of reducing our attack surface and increasing our security posture.

In collaboration with our security team we have proactively identified possible vulnerabilities. In order to address these, AWS WAF (Web Application Firewall) will be introduced to all public facing web applications.

AWS WAF brings with it a number of benefits, including

- Agile protection against web attacks - The WAF rules propagate in under a minute so we can quickly respond and adapt to merging secuirity issues.
- Save time with managed rules - we can quickly get up and running with AWS managed rules and build on them with our own custom rules saving time and effort.
- Improved web traffic visibility - We also benefit from near real time visibility of our web traffic which would give us insight into what rules and alerts are needed.
- Ease of deployment and maintenance - It is a simple process to get AWS WAF set up and begin receiving the protection benefits.

AWS WAF is intended to protect against exploits that may affect availability, compromise security or consume excessive resources. It can be deployed on CloudFront distributions, Load Balancers or API Gateway. The majority of our applications are deployed either via a CloudFront distribution or API Gateway; these will be the areas the additional WAF service will be applied.

## **Decision**

**Implement WAF on our CloudFront and API Gateway services**

This option has been chosen for a number of reasons:

1. To enable additional protection for internet facing services and reduce our attack surface.
2. To gain insight into the internet traffic coming to our applications.

## **Consequences**

This additional service does mean an increase in cost for our services, although it is relatively small. However, we think that the added protection benefit is worth the small cost.
