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

Figure 4: Refined Layered Diagram for AIDAP System


From the layered diagram, we can note the responsibilities of these elements in the following table: 

### Architectural Elements, Responsibilities, and Interfaces

| **Architectural Element** | **Responsibilities** | **Interfaces** |
|---------------------------|----------------------|-----------------|
| **ChatController / QueryHandler (ClientController + Business layer, UC-1)** | Receives the user’s question from the Chat UI, validates the session, and builds the context of the query (course, deadline, and students) before forwarding it to the AI Orchestrator. It receives the AI answer and formats it for the Chat UI. | **Provides:** processQuery(userId, message) <br> **Requires:** AIOrchestrator, StudentDataService, CourseDataService, AuthenticationService |
| **DashboardAggregator / DashboardManager (Client Controller + Business layer, UC-2)** | Orchestrates the personalized dashboard flow. Requests schedule, assignments, exams, GPA, and notification data from backend services; merges them into a single dashboard for student viewing. | **Provides:** getDashboard(userId) <br> **Requires:** RegistrationAdapter, StudentDataService, LMSAdapter, AnalyticsService |
| **Integration Manager / SyncManager (Integration Layer, used in UC-5)** | Central point for managing connections to LMS, Calendar, Registration, and Email systems. Stores and validates integration configurations. | **Provides:** configureIntegration(systemId, config), testConnection(systemId), runSync(systemId) <br> **Requires:** LMSAdapter, RegistrationAdapter, MailAdapter, CalendarAdapter |
| **AI Orchestrator (AI Pipeline Layer / Business layer, UC-1, UC-2)** | Executes the request pipeline and performs all AI pipeline steps to ensure consistent AI behavior for UC-1 and UC-2. | **Provides:** runLLM(promptContext) <br> **Requires:** LLMModel, PromptTemplateStore, PolicyRulesEngine |
| **AuthenticationService / AuthenticationClient (SSO / OAuth2, all UC)** | Handles secure login, token validation, and role extraction for students, lecturers, and admins. Passes authenticated identity to downstream services. Required for UC-1, UC-2, and UC-5. | **Provides:** validateToken(token), getUserRole(token) <br> **Requires:** University SSO Endpoint, OAuth2 Endpoint |
| **Logging and Monitoring Module (Data / Analytics Storage, UC-5 + ops)** | Records API calls, AI latency, sync failures, integration errors, and general system events. Supports maintainability and UC-5 monitoring tasks. | **Provides:** recordEvent(event), getLogs(filter) <br> **Requires:** TimeSource, StorageBackend |
| **LMS / Calendar / Registration / Email Adapters (Adapter Layer, UC-2, UC-5)** | Converts internal AIDAP requests into external LMS / Calendar / Registration / Email API calls. Enables UC-2 dashboards and UC-5 integration management. | **Provides:** fetchCourses(userId), fetchExams(userId), fetchEvents(userId), sendAnnouncement(msg) <br> **Requires:** External LMS, Calendar, Registration, and Email API endpoints |


 
The following sequence diagrams for UC-1, UC-2 and UC-5 were created with the purpose of defining interfaces:

UC-1: Ask a question

![Image 1](https://i.imgur.com/JXR4jTt.png)

### System Methods Table

| **Element** | **Method Name** | **Description** |
|-------------|------------------|------------------|
| **Chat UI** | typeQuestion(message) | The student enters their question and later submits it through the chat interface. |
| **ChatController / QueryHandler** | processQuery(userId, message) | It receives the question. |
| **AuthenticationService** | validateToken(token) | Confirms that the user’s session is valid and that the student is authorized to access AIDAP services. |
| **StudentDataService** | getStudentProfile(userId) | Retrieves profile details such as program, year, and personalized attributes required for contextual answering. |
| **CourseDataService** | getCourseContext(userId) | Retrieves the student’s active courses, deadlines, and academic data relevant to their question. |
| **AI Orchestrator** | runLLM(promptContext) | Executes the AI pipeline, merging question + profile + course context into a final prompt. |
| **PolicyRulesEngine** | applyPolicies(promptContext) | Ensures that the final prompt adheres to institutional and safety rules prior to LLM execution. |
| **PromptTemplateStore** | loadTemplate("Q&A") | Fetches the correct template that structures the LLM conversation. |
| **LLMModel** | generateAnswer(finalPrompt) | Produces the AI-generated answer using the final prompt. |


Explanation:
The sequence diagram illustrates how AIDAP responds to a student’s question. After the student enters their question in the Chat UI input, the Chat Controller first verifies the conversation with the Authentication Service. Once the user is authenticated, the Query Handler handles the question. To read the question, QueryHandler asks the AI Orchestrator to load profile information and academic data (via the LMS/Calendar/Registration adapters) to gather the context of the question. The AI Orchestrator then runs the LLM with the prompt plus the added context and returns an answer. The Query Handler formats the answer and sends it back through the Chat Controller to the Chat UI, which then displays the answer to the student.



UC-2: View personalized dashboard

![Image 2](https://i.imgur.com/BxzjznW.png)

Figure 6: Sequence Diagram for UC-2                 


| **Element**              | **Method Name**              | **Description** |
|--------------------------|------------------------------|-----------------|
| **Dashboard UI**         | openDashboard()              | The student would navigate to the dashboard to view academic information. |
|                          | renderDashboard()            | The method displays the combined dashboard content to the student. |
| **DashboardAggregator**  | getDashboard(userID)         | The module coordinates dashboard creation by requesting all the necessary data sources. |
| **AuthenticationService**| validateToken(token)         | The method validates the session and ensures that the student has the access rights. |
| **StudentDataService**   | getStudentProfile(userID)    | The method provides student profile information displayed in the dashboard. |
| **RegistrationAdapter**  | fetchSchedule(userID)        | The module fetches the student’s personalized schedule or timetable data. |
| **LMSAdapter**           | fetchCourse(userID)          | The method retrieves the course that the student is actively enrolling in. |
|                          | fetchExams(userID)           | The module retrieves the upcoming exams from the LMS. |
| **AnalyticsService**     | getGPA(userID)               | The module calculates and displays the student’s GPA or performance. |


Explanation:
The sequence diagram shows how AIDAP generates and displays the student dashboard. When the student opens the dashboard, the DashboardManager validates the user session through the Authentication Service. It then retrieves the student's profile information, course enrollment, exam schedules, academic timetable, and GPA metrics by querying multiple backend services and adapters. Once all the data is collected, DashboardManager combines it into a unified dataset and sends it to the Dashboard UI, which then presents the personalized dashboard to the student.




UC-5: Manage integrations & policies

![Image 3](https://i.imgur.com/0cv2Qws.png)

                                                                             
Figure 6: Sequence Diagram for UC-5

Explanation:
The sequence diagram explains how the admin configures and executes system integration using the Sync Manager. After accessing the Integration Settings UI, the admin can initiate a sync operation, which first passes through the AuthenticationService for role validation. After validation, the Integration Manager will orchestrate the sync by calling the adapter that fits the selected system (LMS, Calendar, Registration, Email). Each adapter would perform its respective syncing, after which it would return a result. The Sync Manager logs the result using the Logging & Monitoring Module and sends a final sync status back to the UI, which will be displayed to the administrator.

| **Element**                 | **Method Name**            | **Description** |
|-----------------------------|-----------------------------|-----------------|
| **Integration Settings UI** | openIntegrationPage()       | Admin uses this method to open the integration settings. |
|                             | clickRunSync(systemID)      | Admin uses this method to trigger a sync update for a selected system. |
| **AuthenticationService**   | validateToken(token)        | The method confirms admin privileges before letting the system sync. |
| **Integration Manager**     | runSync(systemID)           | The method is based on UC-5; its goal is to orchestrate sync execution for LMS, Calendar, Registration, and Email systems. |
|                             | syncLMSData()               | The module will send or receive sync data from LMS, such as course updates. |
| **Calendar Adapter**        | syncCalendarData()          | The module syncs events, reminders, and schedule data with the calendar system. |
| **RegistrationAdapter**     | syncRegistrationData()      | The module sends registration updates and retrieves enrollment data. |
| **EmailAdapter**            | syncEmailAnnouncements()    | The module sends emails and retrieves message-delivery info. |
| **Logging & Monitoring Module** | recordEvent(syncResult) | The module stores logs summarizing syncing results and errors for maintenance monitoring. |



Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose
With all of the refining done for this iteration, a review of the drivers addressed can be composed inside the following table:

![Image 3](https://i.imgur.com/EyaVdz9.png)

![Image 4](https://i.imgur.com/4qeqY9t.png)


![Image 5](https://i.imgur.com/JxExIz6.png)




