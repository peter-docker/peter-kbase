---
type: policy
version: 17
title: "Commercial Support Service Levels"
summary: "Docker EE is validated and supported to work in specific operating environments as outlined in the Docker Compatibility Matrix, adhere to the Docker Maintenance Lifecycle, and is supported within the defined Docker Scope of Support and Docker Commercial Support Service Levels."
dateCreated: "Fri, 01 Apr 2016 18:14:25 GMT"
dateModified: "Wed, 23 Aug 2017 20:58:30 GMT"
dateModified_unix: 1503521910
upvote: 8
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
---

## Product Subscriptions with Support

**[Docker Enterprise Edition](https://www.docker.com/enterprise-edition "https://www.docker.com/enterprise-edition") i**s a subscription of software, support, and certification for enterprise dev and IT teams building and managing critical apps in production at scale. Docker EE provides a modern and trusted platform for all apps with integrated management and security across the app lifecycle, and includes three main technology components: the Docker daemon (fka "Engine"), Docker Trusted Registry (DTR), and Docker Universal Control Plane (UCP). Docker EE is validated and supported to work in specific operating environments as outlined in the [Docker Compatibility Matrix](/Policies/Compatibility_Matrix "Compatibility Matrix"), adhere to the [Docker Maintenance Lifecycle](/Policies/Maintenance_Lifecycle "Maintenance Lifecycle v2"), and is supported within the defined [Docker Scope of Support](/Policies/Scope_of_Support "Scope of Support") and **Docker Commercial Support Service Levels**. Refer to the [Subscription Services](https://www.docker.com/subscription-services "https://www.docker.com/subscription-services") or the [End User Subscription Agreement](https://www.docker.com/docker-software-end-user-subscription-agreement "https://www.docker.com/docker-software-end-user-subscription-agreement") for more information. To view the latest updates and upgrade instructions, visit the release notes for [daemon](https://docs.docker.com/docker-trusted-registry/cs-engine/release-notes/release-notes/ "https://docs.docker.com/docker-trusted-registry/cs-engine/release-notes/release-notes/"), [DTR](https://docs.docker.com/docker-trusted-registry/release-notes/release-notes/ "https://docs.docker.com/docker-trusted-registry/release-notes/release-notes/"), and [UCP](https://docs.docker.com/ucp/release_notes/ "https://docs.docker.com/ucp/release_notes/").

These commercial support levels are in effect only for the Docker EE product family. They are not in effect for other Docker products such as Docker Community Edition, Docker Cloud, Docker Hub, Docker Compose, and similar software and services.

|                  | **Basic Support** | **Business Day Support**                     | **Business Critical Support** |
|------------------|-------------------|----------------------------------------------|-------------------------------|
| Coverage Hours   | n/a               | 9:00am - 6:00pm local time<sup>1</sup> Monday - Friday | All hours for P1, otherwise 9:00am - 6:00pm local time<sup>1</sup> Monday - Friday |
| Initial Response Time SLA<ul><li>Urgent (P1)</li><li>High (P2)</li><li>Normal (P3)</li><li>Low (P4)</li></ul> | No SLA, commercially reasonable "best effort" | <ul><li>2 business hours</li><li>4 business hours</li><li>12 business hours</li><li>12 business hours</li></ul> | <ul><li>2 hours (when initiated by phone)</li><li>4 business hours</li><li>12 business hours</li><li>12 business hours</li></ul> |
| Contact Methods                        | Customer service web form only | Web, Email, & Phone | Web, Email, & Phone |
| Number of Incidents                    | Unlimited                      | Unlimited           | Unlimited           |
| Number of Support Contacts<sup>2</sup> | 1                              | 4                   | 8                   |

Notes:

1. *"Local time" is defined by the timezone associated with the location shown on the Sales Order.*
2. *"Support contact" is a single individual, named in advance, who is authorized to contact Docker Technical Support to make use of Docker Support Services*

### Support Case Priority Definitions for Docker EE Products only
| **Case Priority** | **Severity Definition** |
|-------------------|-------------------------|
| Urgent (P1) | Any incident which causes a *full production outage* involving Docker EE, UCP, and/or DTR. |
| High (P2)   | Any incident which causes *high impact* to production services or severe impact to *non-critical business operations*. Usually operations are functional but operating in a degraded state and there is no known way to overcome the impact. |
| Normal (P3)    | Any incident which causes *moderate impact* to business operations. Usually operations are only minimally degraded or fully functional (product usage questions or impact to planned deployments). |
| Low (P4) | Any incident which causes *low or no impact* to business operations. Usually a general inquiry or recommendation for product enhancement. |

### Remote Support
Docker Global Support utilizes many tools to assist our enterprise customers, including remote support sessions (screen shares). To maximize the benefit of a remote session, we ask that any requested session be scheduled via an active support case. Remote sessions are normally scheduled for 60 minute durations with a maximum of 90 minutes per session. At the end of the scheduled session the Technical Support Engineer will review next steps and planned follow-up activities with you.

### P1 Procedure
For the best service on Priority 1 issues we recommend:
1. Ensure the product involved in the full production outage is covered by a commercial support subscription. Such products are typically Docker EE, UCP, and DTR.
2. [Open a case](https://support.docker.com/ "https://support.docker.com/") to provide the relevant technical details.
3. After the case is open, call Docker Technical Support at +1-888-214-4258 (toll-free in the USA) or +1-204-318-3889 (outside the USA).
4. During the call, provide your name, email, and phone number. Inform the operator that you have opened a Priority 1 incident and provide the case number.

The case will then be routed to the appropriate Docker Support engineer who will contact you as appropriate.


## Technical Account Management Subscriptions

[Docker's Technical Account Managment (TAM) service](https://www.docker.com/docker-support-services#/services "https://www.docker.com/docker-support-services#/services") is an additional subscription layered on top of the underlying product subscriptions. The TAM service provides a customized guidance and support experience with direct access to an experienced Docker Technical Account Manager. TAM customers:
* Continue to receive direct technical support in accordance with the support service level corresponding to their Docker product subscriptions
* Can contact their TAM directly during local business hours
* Receive TAM support in accordance with their TAM subscription allocation


## Products Without Commercial Support

If a product is not mentioned explicitly in the section above, only Community Support is provided via documentation, knowledge base, community slack, and/or community forums. Products in this category include all open source software and services such as Docker Cloud.
