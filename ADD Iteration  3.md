Iteration 3

Step 2: Establish Iteration Goal by Selecting Drivers
For this iteration, we will focus on refining QA1: 95% of student questions must receive a response within 2 seconds.

Step 3: Choose one or more Elements of the System to Refine
For this scenario, we will look into these components:
QueryHandler
AI Orchestrator
StudentDataService
CourseDataService
Caching Layer

Step 4: Choose one or more Design Concepts that Satisfy the Selected Drivers
The chosen design concept of Iteration 3 is in the following table:


| Design Decision and Location                   | Rationale and Assumption                                                                                     |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Introduce parallel, asynchronous data fetching | Using asynchronous data reduces the total wait time for LMS/Calendar compared to sequential data.            |
| Introduce fast-path LLM selection              | Some questions are very common and can be asked multiple times; these design decisions reduce model latency by using a lightweight model. |

Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces
The instantiation design decisions are composed in the following table:




Step 6: Sketch Views and Record Design Decisions
We can refine the deployment view of this iteration with the following diagram:

![Iteration 3 Deployment Diagram](https://i.imgur.com/ZU9IQXw.png)


We can list out the elements from the refined deployment diagram in the table below:




