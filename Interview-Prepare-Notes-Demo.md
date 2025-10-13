# Interview Prepare Notes - (What project have you been worked with?) [Purely self-prepare thoughts]

## About the point/requirement `1️⃣ Projects you’ve led or contributed to that involved meaningful technical decisions`:

My answer:

For `Project 1`: I led the design of an automated stale data deletion system.

My main decision was to use a Kubernetes CronJob combined with a Node.js API, instead of embedding deletion logic directly in the database or running ad‑hoc scripts.

This solution allowed us to decouple the cleanup logic from the main microservice, and it could also be reused by other services as a typical example.

For `Project 2`: I drove the architecture for the bet interception functionality.

I chose a microservice‑based approach with Node.js services for ingestion and CRUD operations, connected to Elasticsearch for data storage.

One key decision was to integrate MQTT for real‑time updates rather than relying on API polling — this improved UI responsiveness and reduced server load.

## About the point/requirement `2️⃣ How you worked alongside PMs, designers, or other stakeholders`:

My answer:

For both projects, what I did can be divided into a few steps:

(1) I met with the Product Owner (PO) and Team Lead (TL) to understand the business requirements, `analyze` the `business flow`, and identify the key features and functionalities.

(2) I started to `draw diagrams` and schemas (data structures) to visualize the system architecture, components, and interfaces.

(2.5) I also followed `system design principles` such as SOLID and DRY to ensure the system is maintainable, scalable, and easy to understand and use.

(3) I `discussed` the technical solution with the PO and TL to ensure it aligns with the business requirements and constraints.

(4) Once the solution was approved, we `divided` the work into `JIRA` epics and specific `tasks`. With the help of the Agile Lead (AL), we allocated the necessary resources and estimated the rough delivery timeline for stakeholders.

## About the point/requirement `3️⃣ Your thinking behind architectural decisions, trade-offs, or scaling challenges`:

My answer:

For `Project 1`: we considered fault tolerance. The challenge was deleting over 200M records from our system — most were test data. We needed a mechanism to retain stale data IDs if any error occurred during the deletion process (e.g. API request failures), so we could retry deletion later by triggering another API request.

For `Project 2`: we needed real‑time data display so users/traders could easily see the latest bet interception data and handle intercepted bets promptly (without delay), which significantly improves user experience.

Challenges:

For `Project 1`: initially, the team agreed with my suggestion. While developing the feature, I noticed that single deletions consumed too much memory, cause Out Of Memory (`OOM`) error on Kubernets pods and individual `API requests hung` and eventually returned a timeout error.

After noticing this issue, I started thinking about how to run the API requests smoothly without timeouts. I introduced the `"time‑period batch deletion"` concept, which means deleting records only within a specific time range. For example, I first ran deletion for a two‑day window, and if we had more than 10k records in that period, we divided the data into smaller batches before running the deletion API requests.

This approach successfully resolved the timeout errors and made the deletion process more stable and reliable.

For `Project 2`: we faced some UI bugs during testing. In one case, within two seconds, more than 200 intercepted bet messages loaded into the `UI` list, and the `page crashed` because the DOM could not handle that many elements being rendered.

Therefore, I researched and found a relevant suggestion: use a `virtualized list` to display intercepted bets (e.g., 10 per window). When users want to view more, they scroll down; previous entries are hidden because the virtualized list only renders a small set (e.g., 10 records) within the sliding window. When users scroll back up, older records are re‑rendered.


That’s why real‑time data update technology like MQTT came into our consideration.

## About the point/requirement `4️⃣ How you influenced direction or pushed back when needed`:

My answer:

For `Project 1`: I recommanded team to run the cronjob on the TEST envrionment kubernetes pods for a week time and detect if there are any issue occurring, once the project becomes stable, I then asked team to released the code changes into production which ensures the microservice is able to running smoothly without errors.

For `Project 2`: When we discussed the delivery timeline, I was pushed back to my team lead based on the distributed work load by specific JIRA tickets with estimated story points, and convinced my team lead to report to stakeholders to separate the tasks into 2 stages, the MVP stage and official production release stage, and based on priorities, we could deliver the functionality more accurately.


## About the point/requirement `5️⃣ How you adapted based on feedback or shifting priorities`:

My answer:

For `Project 1`: I have also received some feedbacks from team, such as we are planning to add `created_at` & `updated_at` timestamp values as part of response payload before saving data records from bet services into ElasticSearch, so later on we can track the deletion based on the timestamp, which is also easier and accurate, query also shorter and efficient

For `Project 2`: we have decided to add `batch data fetching` for MQTT subscriber which will be able to fetch the data efficiently and dispalyed smoothly without any issues.

For both projects, when I received feedback, I re‑evaluated the solution and found the best trade‑offs to implement. Sometimes, for example, we needed to deliver quickly within a short timeframe, so we balanced features, performance, and user experience.


In a summary:

- I excel at deconstructing complex projects into manageable phases, such as analysis, design, and estimation.
- I am skilled at researching and identifying appropriate technologies to address specific challenges like performance and fault tolerance.
- By presenting data, I facilitate a shift to phased deliverables, which allows us to push back on unattainable dates with a constructive alternative.
- I actively solicit and incorporate feedback from team members, and I am flexible in adapting priorities based on our collective discussions.

In a word, Analyze each problem on a case-by-case basis. 


## My perosnal thought:

The descring steps could be: `What project it is` -> `How to work together with related people` -> `what kind of chanlledges faced, how did overcome those` -> `how to influence the direction and push back some feedbacks` -> `how to adapt the feedbacks and shifiting the priorities`


## Shorter version

Project 1: its a stale data clean up task by helping business to save some costs for cloud storage (✅)

Techs: Keubernetes Cronjob configurations, ElasticSearch, NodeJS API - No mention

Steps how we work together: (✅)
- Find out the team leads (devops + my) to figure out the business requirements over the meeting
- Draw diagrams and understand work flow clearly
- Come up with tech solutions based on some system design princples 
- Run through the proposal with team and grab feedbacks
- After approved, then, start to call AL for help with JIRA tickets creation and also prioritize the specific tasks

Challenges: (✅)
- Handle large amount of data deletion requests smoothly without timeouts
- Avoid Out Of Memory (`OOM`) error on Kubernetes pods

Influence direction: introduced the `time-period-batch-deletion` concept -> making smaller range of deletion and also created batches for better API request handling

Team member suggestion: add some observability logs for tracking the deletion behaviour more accurately

Project 2: it's a bet interception funcitonality, which help traders to accept/reject/counter-offer bets easily, so prevously, blah, blah ..

Techs: MQTT, NodeJS Service + API, ReactJS, ElasticSearch  - No mention

Steps how we work together:
- Find out the PO, UI designer, team lead to figure out the business requirements over the meeting (✅)
- Draw diagrams and understand work flow clearly
- Once designer comes up with a UI design, we also discuss together UI technical discussions whether the UI part is able to be achieved or not (✅), what components needs to be used for from ant design framework
- Meanwhile, design the tech solutions based on understandings
- Run through the proposal with team and grab feedbacks & polish
- After approved, then, start to call AL for help with JIRA tickets creations and also prioritize the specific tasks

Challenges: (✅)
- Handle the UI bugs when loading too many intercepted bet messages at once

Pushed back: we have implemented stages releases -> stage 1: basic version API initial loading -> stage 2: real time intercepted cards displayed -> stage 3 after tested few rounds, we do production release (✅)

Stakeholders clearly know the roadmap for this functionality development

Summary:

- Like to separate the big task into smaller ones, and analyse the entire workflow and then provide the technical solution for it
- Also, proactively to do some researching and identifying appropriate technologies to address specific challenges like performance and fault tolerance

Last word: analyze each problem on a case-by-case basis. 