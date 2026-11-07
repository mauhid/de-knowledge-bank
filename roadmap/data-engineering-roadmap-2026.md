# Data Engineering Roadmap 2026 (Manish Kumar)

Oct 20, 2026 · @Mohammad Mauhid Usmani

## Overview and 6-month timeline

The roadmap takes a minimum of 6 months from scratch at 2–3 hours a day, 5 days a week. It is organized from Manish Kumar's video "data engineer roadmap 2026 | slowest data engineer roadmap" ([video](https://www.youtube.com/watch?v=K7ZtpSsl0AU)). Every level (fresher, 2–4 YOE, 5+ YOE) is covered so the knowledge bank is complete.

1. **Months 1–2: Python + SQL only.** Learn syntax first. After 20–25 Python lectures, solve 1 Python question daily (30 Days Programming Challenge). SQL: 2 HackerRank questions daily, then 1 LeetCode question daily.
2. **Months 3–4: Spark + Cloud.** Python practice never stops (1 question daily). After 15–20 days of Spark fundamentals, solve each LeetCode SQL question in SQL first, then in PySpark. Cloud: ADF about 2 weeks, then Databricks about 4 weeks.
3. **Month 5: Projects (3–4 weeks).** About 1 week for Manish's guided project, then projects from other creators. AI tools allowed only when truly stuck.
4. **Month 6: The rest.** CI/CD 2–3 days, dimensional modeling about 1 week, orchestration 1–2 weeks, plus revision.
5. **Interviews.** Expect 2–3 rejections while you learn how scenario questions are asked; keep going.

Learning order: Python → SQL → Spark → Project → Cloud → Project → Warehousing, CI/CD, Orchestration (Streaming and system design as needed).

**Ground rules from the video:** follow one roadmap only; no practice means no selection; make your own notes for revision; no AI tools while learning, except to convert LeetCode input tables into Spark DataFrames.

## FAQ: key advice

| Question | His answer |
| --- | --- |
| How long will it take? | At least 6 months from scratch. Not 30, 60 or 90 days. |
| Who is it for? | Anyone switching into DE (DBA, DevOps, SWE, data analyst, even 14 YOE), and existing DEs checking market needs. |
| Who is it not for? | People already enrolled in a course or following another roadmap. Stick to one. |
| Should I buy courses (Sumit Mittal, Deepak Goyal, etc.)? | Fine if you need structure, but anything above about ₹20,000–30,000 is not worth it (e.g. multi-lakh programs). |
| Should I get certified? | Optional. Certifications cost around ₹10,000. They help with HR and service companies, not in interviews. Do it if your company pays. |
| Should I use AI tools? | No, not while learning. Better to fail for a day and search Google, Stack Overflow or GitHub. This builds critical thinking. |
| Can I prepare in 2 months? | Only if you are revising things you already know. |
| Snowflake + SQL only? / ADF + Databricks only? | No. Shortcuts get you few interview calls converted. Learn everything properly. |
| "100 hours of content" courses? | Content hours don't matter; practice does. No practice, no selection. |
| System design? | Mostly for 5+ YOE; see his separate system design video. |
| Budget? | About ₹2,000–2,500 total for 4–5 Udemy courses (about ₹400 each). |
| How do I revise and remember? | Make your own notes (revision takes about 10% of the original time) and keep practicing. He does not share his notes. |

## Tracks by experience

We cover all three tracks, so the website can tag each topic by level.

| Area | Freshers | 2–4 YOE (Data Engineer) | 5+ YOE (Senior DE) |
| --- | --- | --- | --- |
| Python | Python + Pandas (no NumPy) | Python, no Pandas | + OOP, design patterns, UML, async |
| Coding/DSA | 2–4 YOE level, slightly less | Arrays and strings, easy | Medium: two pointers, sliding window, binary search, sorting, stack/queue, recursion |
| SQL | Up to joins; window functions a bonus | Everything, all levels, optimization, execution order, EXPLAIN | + DB internals, SQL vs NoSQL |
| Spark | 5–7 overview videos | Full | Same as 2–4 |
| Databricks | Not needed | Required | Required |
| Cloud | Basics, one cloud (AWS preferred) | AWS or Azure, skip GCP | AWS or Azure |
| Other | REST API, Excel, Power BI | Warehousing basics, CI/CD, orchestration | Full warehousing, system design, streaming |

**Why the fresher stack works:** Python + SQL + REST API makes you eligible for backend Python developer roles. Add Spark for DE roles, and Excel + Power BI for data analyst / BI roles. You target three roles at once because DE fresher openings are few.

**Service vs product companies:** service companies ask tool-specific questions (AWS/Azure services) and value certificates. Product companies focus on system design and coding (one level harder) and pay more.

**Streaming:** people switching from another domain should skip it unless they have already worked on it.

## 1. Python

Python is the most important and most time-consuming part; it "rewires the brain", and most people quit here.

**Topics to cover**

- [ ] Core syntax (at least the first 30 lectures of Manish's playlist)
- [ ] OOP concepts
- [ ] FastAPI / REST API basics
- [ ] Pandas (freshers track)
- [ ] Interview concepts: shallow vs deep copy, decorators, scope (GeeksforGeeks "Top 50 Python interview questions")
- [ ] DSA, easy: arrays and strings
- [ ] DSA, medium (5+): two pointers, sliding window, binary search, sorting algorithms, stack and queue, recursion
- [ ] OOP in depth + design patterns (5+): factory, facade, strategy, decorator and a few others (not all 23)
- [ ] UML diagrams (5+)
- [ ] Async programming (5+)

**Learning resources (pick one channel)**

- [Manish Kumar, Python Fundamentals](https://www.youtube.com/playlist?list=PLTsNSGeIpGnEpkKQfQuOWmT85Pvsx1BWY) (41 videos, DE-focused, includes OOP and FastAPI); his preferred choice
- Telusko, "Python for Beginners" (about 125 lessons)
- Code with Harry (about 100 lessons)
- Book: *Design Patterns* by the Gang of Four (all 23 patterns; read only the common ones)

**Practice**

1. Manish's 30 Days Programming Challenge (with assignments), after you are comfortable with syntax
2. HackerRank Python (Prepare section), then GeeksforGeeks, then LeetCode arrays and strings
3. 2–4 YOE: easy LeetCode array/string questions. 5+: about 5–7 medium questions per pattern (about 20+ in total)

No DP, trees or graphs; those are asked of software engineers, not data engineers.

## 2. SQL

SQL is easier than Python, but interviews show no mercy: everything can be asked.

**Topics to cover**

- [ ] Syntax: SELECT, WHERE, GROUP BY, joins, subqueries
- [ ] Window functions
- [ ] Recursive CTEs
- [ ] Query optimization (e.g. don't use a window function where GROUP BY works)
- [ ] SQL execution order
- [ ] EXPLAIN plan basics
- [ ] DB internals (5+): sharding, partitioning, indexing (which index on which column), connection pooling
- [ ] SQL vs NoSQL: when to use which, advantages of each (5+)

**Learning resources**

- [W3Schools SQL](https://www.w3schools.com/sql/), for syntax from zero
- Ankit Bansal's YouTube channel (about 90 complex SQL questions), for problem-solving thinking
- Rishabh Mishra's YouTube channel, for window functions and recursive CTEs

**Practice (solve easy, medium and hard)**

1. HackerRank SQL first, if you are not comfortable yet
2. LeetCode Database section, free questions only; this should be your first choice because it checks the most test cases
3. DataLemur, since some questions are asked directly in interviews

Interviews ask data-style questions, not puzzles like printing star patterns in SQL.

## 3. Spark and Databricks

Spark logic maps one-to-one to SQL: only the syntax differs, so the stronger your SQL, the stronger your PySpark. The same Spark is asked at 2–4 and 5+ YOE.

**Spark topics**

- [ ] Spark fundamentals (theory)
- [ ] Spark practicals (DataFrame API, PySpark coding)
- [ ] Local setup with Docker, or Databricks Community Edition

**Databricks topics**

- [ ] Calling one notebook from another
- [ ] Workflows
- [ ] Unity Catalog
- [ ] Delta Lake / Delta Live Tables
- [ ] Auto Loader
- [ ] Scenario-based Databricks questions

**Resources (pick one per tool)**

- Spark: Manish Kumar's "Complete Spark" playlist (follow the playlist order, not lecture numbers), or Prashant Kumar Pandey's Udemy courses "Apache Spark Programming for Beginners" and "Apache Spark 3 – Beyond Basics and Cracking Job Interviews"
- Spark local setup: Manish's "How to install PySpark with Docker" video
- Databricks: Prashant Pandey's "Master Azure Databricks for Data Engineers" (Udemy), Ramesh Retnasamy's Azure Databricks course (Udemy), or Ease With Data's "Databricks Zero to Hero" playlist (Shubham Khandelwal)

**Practice method**

1. Take a LeetCode Database question and solve it in SQL.
2. Copy the input tables and ask an AI tool only to convert them into Spark DataFrames (the one allowed AI use).
3. Write the PySpark solution yourself and run it locally or in Databricks.

## 4. Cloud

Pick AWS or Azure and skip GCP, which has few openings. Azure has the most free resources; AWS has fewer structured ones. If you already work on AWS, learn AWS.

**Equivalent services (approximate mapping)**

| Purpose | AWS | Azure | GCP |
| --- | --- | --- | --- |
| Object storage | S3 | ADLS | GCS |
| ETL / pipelines | Glue | ADF (Data Factory) | Dataflow (not a close match) |
| Spark compute | EMR | Databricks | Dataproc |
| Streaming ingestion | Kinesis | Event Hubs | Pub/Sub |
| Warehouse | Redshift | Synapse (he uses Snowflake instead) | BigQuery |

**Azure focus:** ADF and Databricks matter most; skip Synapse in favour of Snowflake. VMs and Blob Storage are useful but need little study.

**Topics to cover**

- [ ] Cloud basics and core storage
- [ ] ADF (or AWS Glue equivalent), about 2 weeks
- [ ] Databricks on cloud, about 4 weeks

**Resources**

- Ramesh Retnasamy's Azure Data Factory and Azure Databricks courses (Udemy)
- WafaStudies YouTube: ADF, Azure Databricks and real-time questions playlists
- Ansh Lamba YouTube: Azure projects and scenario-based questions
- AWS: he has no specific resource; Darshil Parmar covers AWS projects (see Projects)

## 5. Projects

Build 2 major and 2 minor projects, after the four fundamentals (Python, SQL, Spark, Cloud). Use 2 creators for different mindsets; more than that adds little because projects start to look the same.

- [ ] Major project 1: Manish Kumar's "DE Complete Project 1" playlist (code-first Spark pipelines, production-grade style), about 1 week
- [ ] Major project 2: Ansh Lamba (Azure-heavy) or Darshil Parmar (AWS and Azure)
- [ ] Minor project 1
- [ ] Minor project 2

Ramesh Retnasamy's Azure courses are project-based too. AI tools are allowed here only when you are truly stuck on an error, never to explain the code to you.

What projects teach: how Python, SQL, Spark and cloud combine into a running pipeline that answers a business need, which is what scenario-based interview questions test.

## 6. Warehousing

2–4 YOE are asked basics; 5+ YOE can be asked anything in his 20-lecture playlist and may have to build a dimensional model.

- [ ] Fact vs dimension tables
- [ ] Normalization
- [ ] Star schema and snowflake schema
- [ ] Slowly changing dimensions (SCD)
- [ ] Full dimensional modeling (5+): Manish's "Data Warehouse Modeling" playlist (20 lectures)
- [ ] Book: *The Data Warehouse Toolkit* by Ralph Kimball (what he and "90% of engineers" read)
- [ ] One warehouse tool: Snowflake (Azure, or AWS without Redshift), Redshift (AWS), or BigQuery (GCP)

Snowflake resources: "Data Engineering Simplified" YouTube channel, or Nikolai Schuler's "Snowflake – The Complete Masterclass" (Udemy).

## 7. CI/CD

CI/CD gets code from development to production. Interviews ask mostly about pull requests, merging and Git commands.

- [ ] Git basics (any 30–60 min video): clone, pull, commit, push, status, branches, merge conflicts
- [ ] One CI/CD tool; his pick is Azure DevOps (independent of the Azure cloud; usable with AWS or GCP). Others: GitHub Actions, GitHub + Jenkins
- [ ] Resource: the CI/CD section (about 2h 25m) of Ramesh Retnasamy's ADF course; do this section even if you skip the rest

## 8. Orchestration

Orchestration schedules pipelines (e.g. run daily at 6 AM). No interviewer can insist on Airflow specifically, though knowing it gives a small edge.

- [ ] Pick one: Airflow, ADF + Databricks Workflows (Azure), or Lambda + Step Functions / MWAA (AWS). Others exist: Autosys, Control-M
- [ ] Airflow resources: Manish's Airflow series, Coder2j (about 1.5 of 2 hours is enough to write a DAG), Marc Lamberti (Udemy, for internals)

## 9. Streaming

Optional: people switching domains should avoid it unless they have worked on it. Kafka and Spark Streaming come together.

- [ ] Pub/sub model; Kafka (Kinesis, Event Hubs and Pub/Sub work similarly)
- [ ] Kafka: Learning Journal YouTube (overview, enough for 2–4 YOE) or Prashant Pandey's Kafka course on Udemy (5+)
- [ ] Spark Streaming: Ease With Data YouTube playlist
- [ ] Skip for now: Flink, Dataflow / Apache Beam

## 10. System design

Mostly for 5+ YOE. For 2–4 YOE and switchers, a solid understanding of your own projects' pipelines is enough.

- [ ] Manish's "system design round for data engineer" video (40–50 min)

## Books, certifications and paid courses

He names two books and no specific certification. Paid courses total about ₹2,000–2,500 if you buy 4–5 Udemy courses.

| Type | Item | For | Note |
| --- | --- | --- | --- |
| Book | *The Data Warehouse Toolkit*, Ralph Kimball | Warehousing, 5+ | His main recommendation |
| Book | *Design Patterns*, Gang of Four | Python OOP, 5+ | Read only the common patterns |
| Certification | Cloud / Databricks certs (none named) | 3–5+ YOE, optional | About ₹10,000 each; helps HR and service companies, not interviews |
| Udemy | Prashant Kumar Pandey: Spark for Beginners; Spark 3 Beyond Basics | Spark | Alternative to Manish's playlist |
| Udemy | Prashant Kumar Pandey: Master Azure Databricks | Databricks | Or Ramesh Retnasamy's |
| Udemy | Ramesh Retnasamy: Azure Data Factory; Azure Databricks | Cloud, CI/CD | ADF course has the CI/CD section |
| Udemy | Nikolai Schuler: Snowflake – The Complete Masterclass | Warehousing | Or free YouTube |
| Udemy | Marc Lamberti: Apache Airflow | Orchestration | For internals only |
| Udemy | Prashant Kumar Pandey: Apache Kafka | Streaming, 5+ | Optional |

One open question: the course price ceiling in the FAQ is unclear in the auto-generated transcript; confirm it against the video.

## Knowledge bank tracker

Each topic becomes one notes chapter for the website. Update the status as we go.

| # | Topic | Level tags | Status |
| --- | --- | --- | --- |
| 1 | Python core syntax | All | Not started |
| 2 | Python OOP | All, deep for 5+ | Not started |
| 3 | FastAPI / REST API | All | Not started |
| 4 | Pandas | Freshers | Not started |
| 5 | Python interview concepts | All | Not started |
| 6 | DSA: arrays, strings, patterns | All, medium for 5+ | Not started |
| 7 | Design patterns, UML, async | 5+ | Not started |
| 8 | SQL (syntax to optimization) | All | Not started |
| 9 | DB internals, SQL vs NoSQL | 5+ | Not started |
| 10 | Spark | All | Not started |
| 11 | Databricks | 2–4, 5+ | Not started |
| 12 | Cloud (Azure or AWS) | All | Not started |
| 13 | Projects | All | Not started |
| 14 | Warehousing + Snowflake | All, deep for 5+ | Not started |
| 15 | Git and CI/CD | 2–4, 5+ | Not started |
| 16 | Orchestration (Airflow) | 2–4, 5+ | Not started |
| 17 | Streaming (Kafka, Spark Streaming) | 5+, optional | Not started |
| 18 | System design | 5+ | Not started |
| 19 | Excel and Power BI | Freshers | Not started |
