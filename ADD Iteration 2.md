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

### Architectural Elements, Responsibilities, and Interfaces

| **Architectural Element** | **Responsibilities** | **Interfaces** |
|---------------------------|----------------------|-----------------|
| **ChatController / QueryHandler (ClientController + Business layer, UC-1)** | Receives the user’s question from the Chat UI, validates the session, and builds the context of the query (course, deadline, students). Forwards it to the AI Orchestrator and formats the AI’s answer for the UI. | **Provides:** processQuery(userId, message) <br> **Requires:** AIOrchestrator, StudentDataService, CourseDataService, AuthenticationService |
| **DashboardAggregator / DashboardManager (Client Controller + Business layer, UC-2)** | Orchestrates viewing of the personalized dashboard. Requests schedule, assignments, exams, GPA, and notifications from backend services and merges them into a single student dashboard. | **Provides:** getDashboard(userId) <br> **Requires:** RegistrationAdapter, StudentDataService, LMSAdapter, AnalyticsService |
| **Integration Manager / SyncManager (Integration Layer, UC-5)** | Central point for managing connections to LMS, Calendar, Registration, and Email systems. Stores and validates integration configurations. | **Provides:** configureIntegration(systemId, config), testConnection(systemId), runSync(systemId) <br> **Requires:** LMSAdapter, RegistrationAdapter, MailAdapter, CalendarAdapter |
| **AI Orchestrator (AI Pipeline Layer / Business layer, UC-1, UC-2)** | Executes the AI request pipeline and performs all AI pipeline steps for consistent AI behavior across UC-1 and UC-2. | **Provides:** runLLM(promptContext) <br> **Requires:** LLMModel, PromptTemplateStore, PolicyRulesEngine |
| **AuthenticationService / AuthenticationClient (SSO / OAuth2, all UC)** | Handles secure login, token validation, and user-role extraction for students, lecturers, and admins. Propagates authenticated identity to downstream services. | **Provides:** validateToken(token), getUserRole(token) <br> **Requires:** University SSO Endpoint, OAuth2 Endpoint |
| **Logging and Monitoring Module (Data / Analytics Storage, UC-5 + ops)** | Records API calls, AI latency, sync failures, and integration errors. Supports maintainability and UC-5 monitoring tasks. | **Provides:** recordEvent(event), getLogs(filter) <br> **Requires:** TimeSource, StorageBackend |
| **LMS / Calendar / Registration / Email Adapters (Adapter Layer UC-2, UC-5)** | Converts AIDAP internal requests into LMS / Calendar / Registration / Email API calls. Enables UC-2 (dashboard) and UC-5 (integration management). | **Provides:** fetchCourses(userId), fetchExams(userId), fetchEvents(userId), sendAnnouncement(msg) <br> **Requires:** External LMS, Calendar, Registration, and Email API endpoints |



Step 6: Sketch Views and Record Design Decisions
The layered diagram in Iteration 1 is on a high-level view, so many modules are generalized. In Iteration 2, these modules are defined concisely and given a more specific functionality:

![Diagram](https://i.imgur.com/j6IaRCX.png)







