# Interview Prepare Notes - (What project have you been worked with?)

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

For `Project 1`: we considered fault tolerance. The challenge was deleting over 100M records from our system — most were test data. We needed a mechanism to retain stale data IDs if any error occurred during the deletion process (e.g. API request failures), so we could retry deletion later by triggering another API request.

For `Project 2`: we needed real‑time data display so users/traders could easily see the latest bet interception data and handle intercepted bets promptly (without delay), which significantly improves user experience.

That’s why real‑time data update technology like MQTT came into our consideration.

## About the point/requirement `4️⃣ How you influenced direction or pushed back when needed`:

My answer:

For `Project 1`: initially, the team agreed with my suggestion. While developing the feature, I noticed that single deletions consumed too much memory, and individual API requests hung and eventually returned a timeout error.

After noticing this issue, I started thinking about how to run the API requests smoothly without timeouts. I introduced the "time‑period batch deletion" concept, which means deleting records only within a specific time range. For example, I first ran deletion for a two‑day window, and if we had more than 10k records in that period, we divided the data into smaller batches before running the deletion API requests.

This approach successfully resolved the timeout errors and made the deletion process more stable and reliable.

For `Project 2`: we faced some UI bugs during testing. In one case, within two seconds, more than 200 intercepted bet messages loaded into the UI list, and the page crashed because the DOM could not handle that many elements being rendered.

Therefore, I researched and found a relevant suggestion: use a virtualized list to display intercepted bets (e.g., 10 per window). When users want to view more, they scroll down; previous entries are hidden because the virtualized list only renders a small set (e.g., 10 records) within the sliding window. When users scroll back up, older records are re‑rendered.

## About the point/requirement `5️⃣ How you adapted based on feedback or shifting priorities`:

My answer:

For `Project 1`: I have also received some feedbacks from team, such as we are planning to add `created_at` & `updated_at` timestamp values as part of response payload before saving data records from bet services into ElasticSearch

For `Project 2`: we have decided to add batch data fetching for MQTT subscriber which will be able to fetch the data efficiently and dispalyed smoothly without any issues.

For both projects, when I received feedback, I re‑evaluated the solution and found the best trade‑offs to implement. Sometimes, for example, we needed to deliver quickly within a short timeframe, so we balanced features, performance, and user experience.

In a word, Analyze each problem on a case-by-case basis. 
