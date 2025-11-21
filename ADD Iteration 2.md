Iteration 2: Identifying Structures to Support Primary Functionality


Step 2: Establish Iteration Goal by Selecting Drivers

The Goal of Iteration 2  is to refine and redefine the architectural structures that support the primary functionality of the AI-Powered Digital Assistant Platform (AIDAP). Since the system must help provide an interface and retrieve information from multiple institutional sources. And now this iteration focuses on refining the architectural elements required  to support the  following key use cases:

UC-1: ask a question 
UC-2: View personalized dashboard
UC-5: Manage integrations and policies 


Step 3: Choose One or More Elements of the System to Refine

Elements of the system to decompose:
We select the AI Processing and Integration Layer of AIDAP as the element to decompose. This subsystem is responsible for handling the platform’s primary interactions, including interpreting user queries, retrieving data, and generating responses for students, faculty and administrators. It supports the main use cases identified for this iteration (UC-1: Ask a Question, UC-2: View Personalized Dashboard, and UC-5: Manage Integrations and Policies).

This subsystem is also the central point through which AIDAP interacts with many external university systems, such as LMS, registration system, calendars, and mail services. 
Include in this file the 7 steps for Iteration 2.


Step 4: Choose One or More Design Concepts that Satisfy the Selected Drivers
For this iteration, we can select a few design decisions that can be refined in the following table:

### Design Decisions and Rationale

| **Design Decisions and Locations** | **Rationale and Assumptions** |
|------------------------------------|--------------------------------|
| **Domain model analysis to identify core AIDAP entities (logical view)** | A domain model is used to create and capture the main entities needed for UC-1, UC-2, UC-5, like (Students, course, exam, assignment, integration, policy). A domain model is required first, before decomposing the system’s functionality, to prevent missing data. |
| **Identify Domain Objects that map to the primary use cases** | UC-1, UC-2, and UC-5 each require specific functional building blocks or, in this case, elements: *QueryHandler, DashboardManager, IntegrationManager*. Extracting these domain objects ensures each use case is supported by dedicated functionality. |
| **Decompose Domain Objects into Layer-Specific Components** | This follows the architecture selected in iteration 1; the domain objects must be broken down into components across the presentation layer, backend API layer, AI processing layer, and integration layer. This aligns with FCAPS patterns by mapping domain objects to modules. |
| **Use Centralized Access Layer for Authentication / SSO** | All these use cases (UC-1, UC-2, UC-3) require very secure access to institutions and their systems. A centralized Authentication layer enforces SSO consistency and avoids security duplication. |
| **Web Framework for UI (React, Angular, etc.)** | Supports an interactive dashboard (UC-2), which would implement a smooth conversational interface, including chat windows, history widgets, and a taskbar. |
| **OAuth2 / OpenID Connect (SSO)** | Useful for administrators (UC-5). Works best with administrator connectors and policy management. Allows AIDAP to authenticate users using the university’s SSO and ensures secure access to LMS, Calendar, and Registration services. |


Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces
In this step, many of the elements only mentioned in a general manner in Iteration 1 are viewed through and refined further through this table:



