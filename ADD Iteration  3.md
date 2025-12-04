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

![Image](https://i.imgur.com/ITHaTPv.png)

Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces
The instantiation design decisions are composed in the following table:

![Image](https://i.imgur.com/IDb5QAQ.png)

Step 6: Sketch Views and Record Design Decisions
We can refine the deployment view of this iteration with the following diagram:

![Iteration 3 Deployment Diagram](https://i.imgur.com/ZU9IQXw.png)


We can list out the elements from the refined deployment diagram in the table below:

![Image](https://i.imgur.com/urnK0On.png)

The following UML sequence diagram illustrates how the Caching Layer interacts with other physical nodes, specifically with StudentDevice, LoadBalancer, AppServer, and CacheServer, during the execution of UC-1: Ask a question. The Caching Layer is created in the iteration to support QA-1 by reducing the overall response time when a student submits a question.

![Image](https://i.imgur.com/u8mv3qn.png)


Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose
With Iteration 3 almost done, we can make a table of the main drivers that have been reviewed:


![Image](https://i.imgur.com/tc8mdzm.png)
![Image](https://i.imgur.com/hVN3JiN.png)



