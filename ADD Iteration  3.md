| Design Decision and Location                   | Rationale and Assumption                                                                                     |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Introduce parallel, asynchronous data fetching | Using asynchronous data reduces the total wait time for LMS/Calendar compared to sequential data.            |
| Introduce fast-path LLM selection              | Some questions are very common and can be asked multiple times; these design decisions reduce model latency by using a lightweight model. |

