| Use Case | Description | Linked Requirements |
|----------|-------------|---------------------|
| **UC-1: Ask a question** | The student asks a question via chat or voice to AIDAP, such as “When is my next exam?” or “What are my upcoming assignments?” | R1, R4, R5, R6, RS1, RS5, RS7, RS8, RS10 |
| **UC-2: View personalized dashboard** | The student requests that AIDAP provide an overview of their personalized schedule and academic status, including their GPA and exam schedules. | R1, R2, R3, RS2, RS3, RS5, RS9, RS10, RS12 |
| **UC-3: Post-course announcement** | The lecturer requests AIDAP to send an announcement to all students enrolled in the course. | R1, R3, R4, RL2, RL8, RA3 |
| **UC-4: Schedule automated reminder** | The lecturer requests AIDAP to set an automated reminder at a specific time for assignments, labs, meetings, etc. | R3, RL4, RL5, RS2 |
| **UC-5: Manage integrations & policies** | The administrator connects AIDAP to the campus institute's systems, such as email, websites, and the registration system. | R3, R8, RA1, RA2, RA3, RA5, RD2 |
| **UC-6: Operate & monitor the platform** | The system maintainer monitors AIDAP by checking uptime, logs, CPU latency, and other relevant metrics. | R7, RA6, RM1, RM2, RM4, RM6, RM7 |
| **UC-7: Sync & recover with data sources** | The system ensures consistency between AIDAP and external systems by syncing every 15 minutes. | R3, R6, RD1, RD2, RD3, RD4, RM5 |


## Quality Attributes

| **ID** | **Quality Attribute** | **Scenario** | **Associated Use Case(s)** |
|:--:|:--|:--|:--|
| **QA1** | Performance | When a student submits a query, AIDAP responds within 2 seconds for 95% of requests. | UC-1, UC-2 |
| **QA2** | Reliability | When a system node fails during normal usage, auto-failover restores the system within 5 minutes, maintaining 99% uptime. | UC-6, UC-7 |
| **QA3** | Scalability | When concurrent user count spikes, the load balancer scales AI and web components, keeping latency increases under 20%. | UC-1, UC-2, UC-3 |
| **QA4** | Security | When unauthorized access is detected, access is blocked immediately, the event is logged, and the admin is alerted. | UC-1, UC-3, UC-5, UC-6 |
| **QA5** | Usability | When a first-time student completes a query within 3 minutes without assistance, the interface follows UI conversational guidelines. | UC-1, UC-2 |
| **QA6** | Interoperability | When the system detects API changes, it adapts using a compatibility layer and completes synchronization within 10 minutes. | UC-5, UC-7 |
| **QA7** | Modifiability | When a new model is deployed, the system integrates it with zero downtime; deployment takes < 1 hour. | UC-6 |
| **QA8** | Observability | When a query exceeds 2 seconds, an automatic alert is generated. The maintainer is notified within 60 seconds and logs are stored for diagnostics. | UC-1, UC-6 |

## Constraints

| **ID** | **Constraint** |
|:--:|:--|
| **CON-1** | The system shall authenticate all users through Ontario Tech University's SSO before granting access to AIDAP functions and services. |
| **CON-2** | AIDAP shall integrate only with approved systems (Calendar, LMS, Registration, Email) through secure APIs to maintain consistency and security. |
| **CON-3** | All student and lecturer data shall be encrypted in transit and at rest, complying with university security policies. |
| **CON-4** | The system shall retain interaction data for at least 30 days to support personalized dashboards and analytics. |
| **CON-5** | AIDAP shall maintain an uptime of 99.5%, with automatic failover and recovery within 30 seconds of failure. |
| **CON-6** | The system shall respond to user queries within 2 seconds on average under normal load conditions. |

## Concerns

| **ID** | **Concern** |
|:--:|:--|
| **CRN-1** | Decide which AI frameworks to use (e.g., ChatGPT API, Whisper, PyTorch, etc.) and how to fine-tune or retrain them safely over time. |
| **CRN-2** | Design AIDAP for easy maintenance and clean AI model updates, allowing new features or bug fixes to be deployed without disrupting users. |
| **CRN-3** | Establish proper security and privacy controls that comply with Ontario Tech University’s data protection and access policies. |
| **CRN-4** | Determine how AIDAP will integrate with Ontario Tech’s existing systems (LMS, registration, calendar, email) while maintaining data consistency across all users. |





Iteration 1: Establishing an Overall System Structure

Step 1: Review Inputs
The inputs that are to be reviewed are in the following table:


## Iteration Driver Summary

| **Category** | **Details** |
|:--|:--|
| **Design Purpose** | The purpose is to establish a stable, secure, and scalable high-level architecture for AIDAP that integrates with university systems while meeting the primary drivers. |
| **Primary Functional Requirements** | From all of the use cases, the primary ones are:<br>• **UC-1**: Because it affects many design decisions early on.<br>• **UC-2**: Because it supports multiple user roles.<br>• **UC-5**: Because it ensures AIDAP acts as a federated system for real institutional data. |
| **Quality Attribute Scenario** | The primary quality attributes are as follows:<br><br>  
**Scenario Table:**<br><br>  
| Scenario ID | Importance to Customer | Difficulty of Implementation |  
|:--:|:--:|:--:|  
| QA1 | High | Medium |  
| QA2 | High | Low |  
| QA3 | High | High |  
| QA4 | High | High |  
| QA7 | Medium | High |  
 |
| **Constraints** | All of the constraints are needed for this iteration. |
| **Concerns** | All of the concerns are mentioned in this iteration. |




Step 2: Establish Iteration Goal by Selecting Drivers
The selected drivers to be reviewed in this iteration are:
QA1: Performance
QA2: Reliability
QA4: Security
CON-1: Must authenticate via Ontario Tech SSO before access
CON-2:Integrates ONLY with approved university systems via secure APIs
CON-3: All data encrypted in-transit and at rest
CRN-3: Proper security and privacy controls aligned with university policies
CRN-4: Smooth integration with LMS, registration, calendars, email


Step 3: Choose One or More Elements of the System to Refine
Because the initial iteration is to establish the overall structure of the system, the target for refinement in this iteration is the entire AIDAP system.
![alt text](https://i.imgur.com/xxxxxx.png)
