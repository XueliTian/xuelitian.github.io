---
title: Databricks vs Snowflake
date: 2024-11-05
lastmod: 2025-11-11
description: A practitioner’s take on Databricks vs Snowflake - who they serve best, why we consolidated on Databricks, and how to choose based on workloads, skills, and cost—plus context on benchmarks, openness, and the evolving rivalry.
draft: false
---

# TL;DR

Databricks for AI, Snowflake for BI; Databricks for Engineers*, Snowflake for Analysts; Databricks for In-house Development, Snowflake for Consulting Engagements.

*Sorry True Data Scientist, you are referred as a ML Engineer here.

A slightly longer version: Between those two, If you are looking for a Modern Data Warehousing solution mainly serving Data Analysts or Business Analysts, pick Snowflake; If you are looking for a modern Data Lake mainly serving Data Scientists and Engineers , pick Databricks. If you are looking for something serving all of them, either would work - But what is your team's bread and butter?

# Why I qualify to comment

I have been using both platforms since 2019. Actually I was the key decision maker of moving to Databricks from Azure HDInight (a horrible product), and part of the  committee made the decision of using Snowflake.

Fast forward, in 2023, I led the team and made the decision of consolidating our data platform into one, depreciating snowflake and eventually moving away from Snowflake.

Since 2019, I have been invited to discuss this topic in different private groups and customer experience sharing group. 

I have always wanted to write such a blog post about Databricks vs Snowflake, especially when the topic was hot (not only once). But I felt the potential legal risk as we are using both platforms. But as we have moved out of Snowflake completely, I feel less risky to write such a topic.

For the remaining of the blog post, I will use the term Cloud Data Lakehouse to refer to the battlefield. I love this term much more than the current Databricks term (data intelligence platform) or Snowflake term (The AI Data Cloud) as of Nov 6, 2024. I will talk more later on why they moved away from the Data Lakehouse like term.

# Our Journey

TL;DR: We chose Databricks as I am part of the Data Science org and we have other data infrastructure focusing more on the descriptive analysis. 

## Why do we choose to use both at the beginning

Back in 2019, the Cloud Data Lakehouse area is very fresh. Databricks hasn't created the term "Lakehouse" (it was created in Jan 2020), and Snowflake was still new to the market. We were still suffering from HDInsight and ADF V1 on Azure. 

At the beginning of the year, Cloudera completed its $5.2 billion acquisition of Hortonworks.  Since Azure HDInsight was co-created by Hortonworks and Microsoft, we began to think about our future. A mix of things were taken into consideration: Should we move beyond Hive and Hadoop to Spark, should we consider Kafka and go full streaming, should we consider something more like PaaS instead of IaaS (HDInsight was marketed as PaaS but actually an IaaS with less flexibility, taking the bad part of both worlds), should we consider another cloud provider so that we can use BigQuery and Cloud Composer, or Redshift and glue.

That was the time Azure Databricks jumped into our vision. I has been GA (General Available) on Azure for a year with Python native notebooks on Spark. As a data science platform, it was much easier to use than DSVM (data science virtual machine) on Azure. As a big data platform, it can start a cluster right after the workspace creation with 90% less The configuration time than HDInsight.  That was the ideal system for us.

Right after the exploration, I was tasked to find a SAS alternative. Azure ML was still very young back then without a native notebooks, and Cloudera was expensive and heavy. At the end of the data, we chose Databricks for our Data science environment and data lake.

Then some data warehouse need comes to us. Back in the Hive day, we have a low cost data warehouse like solution. But keeping a performant enough Databricks Cluster running 24/7 was still too expensive for us in 2019. So we are tasked to find a good representation layer for our data lake.

Our choice were limited back then. Azure Synapse was still called Azure SQL Data Warehouse with very limited feature. Surprisingly, it was also expensive. As a company, we were moving away from GCP so Big Query was not an option. We were stuck in Azure in general. So this become a pending question.

I remember attending the last ever in-person Strata conference in 2019, and everyone was interested in Snowflake while competitors still joked about its performance and scalability. However, even the competitors cannot challenge how easy it is to use Snowflake. I talked with several vendors and came back with a clear answer: Snowflake.

At the end of 2019, we ended up with a recommended architecture from both Databricks and Snowflake: Databricks as the Data Science Environment, and Snowflake as the presentation layer. There was a small debate on which platform would keep the data gravity; but we ended up with saving most of data in Azure Blob Storage, keeping it flexible for both platforms to use.

## Why do we choose to migrate, and how we migrated

As Databricks and Snowflake began to invade into each other's area, we began the thinking of unification. Back then, We started a new data engineering team, so we would like to make sure the team could focus on growing key capabilities on a platform. However, with the growing of both platforms, it seems impossible even for the senior team members to catch up with what is developed, what is possible, and what is possible beyond marketing material. 

We loved the easy data management and serverless of Snowflake, and we loved the flexibility and scalability of Databricks. To us, Snowflake is like an Intel based Mac Mini, It works when it works, but hard to push the boundary, especially in the ML area; While Databricks is like a powerful Linux server that you can get anything done if you have the right people. Snowflake would get 80% of our projects done easier (not faster nor better), but unfortunately, the team's bread and butter is the other 20% of projects - something has high value, long strategic impact and requires flexible and powerful computing which Snowflake cannot provide. 

To be specific, for the other 20% of the projects, we hit the Snowflake Promise wall again and again: their account team gathered our ideas, promised share certain solutions back which we either got nothing or got nothing could really work. 

So at the end of the day, we found most of the core Snowflake capabilities are the ones good but we don't care, and Snowflake is not capable of supporting our core competency.

So the migration changed from a why question to a when question. We were eagerly waiting for the availability of "Snowflake like experience" on Databricks.

With the Unity Catalog becoming General Averrable (GA) and Serverless SQL warehouse  becoming Public Preview (PP) on Azure Databricks (August 2022), a small group of us began sitting down near a table and drawing the future, while still doing our daily work under regular workload. Eventually, it took us 6 - 9 months to design the architecture and scope out the implementation plan, and another 12 months to implement the plan while keeping everything running with optimized cost structure.  

The design and implementation is much beyond a Snowflake to Databricks migration: it is more about modernizing our Enterprise Data Analytics Platform.  We upgraded our infrastructure, re-designed our end to end workflow and created the unified technology foundation for all future AI projects with potential to cover advanced BI use cases. We call it Databoost internally, which is suggested by ChatGPT.

I definitely want to praise both Databricks and Snowflake, as they make the effort of migrating off their platform easy and legal worry free. 

# Which one should you choose?

You: Architect, technical decision maker

## Cloud Provider Matters

- AWS: Both Snowflake and Databricks are 3rd party products on marketplace.
- Azure
	- Snowflake is a 3rd party product on marketplace
	- Azure Databricks is 1st party product co-developed by Microsoft and Databricks thus.
- GCP: Both Databricks and Snowflake have much less market share on GCP. However, both are catching up on their GCP offering with promises to catch up functionalities on other platforms 

## Billing

Databricks: Harder to manager, more of PaaS. (At least on Azure, not sure about AWS and GCP)
- Databricks would create infrastructure in your resource group manag ed by Databricks, which could cause billing challenge inside the organization

Snowflake: Really easy to understand and track, true SaaS.

## The sales team

Databricks: Google/Salesforce like, developer friendly

Snowflake: Oracle/IBM like, Executive friendly

## Ecosystems

Snowflake is more Modern Data Stack (MDS) friendly. Much more companies build on Snowflake comparing to Databricks. Be cautious, more doesn't necessary means better.

# Which one should you learn?

You: A new graduate or developer looking for a job

From the market perspective, there are more Snowflake job opportunities, but Databricks job opportunities pay more (and harder). 

As Snowflake is used by more companies and business units (BU) in big companies, there are much more consulting opportunity in Snowflake than Databricks. And there are more low-hanging fruits in those consulting opportunities. 

# Appendix: From collaboration to competition

## Tone Change

When we decided the architecture of using Databricks and Snowflake together, that was a happy time between the two companies. We even have meetings with Databricks and Snowflake reps together in the same room, and they were talking about a lot of their customers using both together. And the boundary is clear: Databricks is your data science environment and data lake, and Snowflake is your data warehouse. 

Right before pandemic (Feb 2020), things begin the change: Databricks began to embrace the amazing concept of Data lakerhouse, while Snowflake began to introduce Python Notebook vendors to us. The sales rep began to avoid metnioning the other's name, or even began to attack others' areas. 

Soon Snowflake began to adress the pressure on us the bring more data into snowflake, and to renew/extend the contract. We were confused and unfortable back then, but when looking back, all began to make sense - it is all about the IPO in Sep 2020.

The year of 2020 is crazy for everyone, but between Databricks and Snowflake, it was the year that they began to invade each other's area quietly. I would guess Databricks started the war with the concept of Data lakehouse, and Snowflake covuterback with more Data Engineering and Data Science feature.  

## Open Conflict: TPC-DS benchmark

- **Time**: The conflict became prominent in November 2021 when Databricks published results from a TPC-DS benchmark that claimed superior performance over Snowflake.
- Started by: Databricks
- **Reason**: The primary reason for the conflict was Databricks' announcement that it had set a new world record in the TPC-DS benchmark, demonstrating that its data lakehouse technology was significantly faster and more cost-effective than Snowflake's. Databricks claimed it was 2.7 times faster and 12 times better in price-performance compared to Snowflake. Snowflake challenged these claims, arguing that the benchmarks did not reflect real-world scenarios and were misleading.
- Comment: Databricks started an academic friendly war with Snowflake. As Databricks founders have strong academic background, it is a crushing win. 

Sources
- [Snowflake rebuts DataBricks' Snowflake performance comparison](https://blocksandfiles.com/2021/11/15/snowflake-rebuts-databricks-snowflake-performance-comparison/)
- [Snowflake vs. Databricks Feud: Key Insights - Acceldat](https://www.acceldata.io/blog/snowflake-vs-databricks-feud)

## Open Conflict: Total Cost of Ownership

- Time: October 2024
- Started by: Snowflake
- Reason: Disputes over which platform offers better cost efficiency. The conflict arose due to differing pricing models and cost management capabilities. Databricks offers separate charges for platform services and cloud infrastructure, allowing for potential optimization but requiring complex management. Snowflake bundles these costs, providing simplicity but less flexibility

## Some other small conflicts

In recent years, the rivalry between Databricks and Snowflake has been marked by several significant conflicts, primarily revolving around issues of openness, platform capabilities, and strategic acquisitions. Here are some notable conflicts:

## Conflicts

1. **Open Source Catalogs (2024)**
   - **Time**: June 2024
   - **Reason**: Both companies aimed to position themselves as leaders in open-source data management.
   - **Main Topic**: Databricks open-sourced its Unity Catalog under an Apache 2.0 license, allowing for broader interoperability and reducing vendor lock-in. This move was shortly after Snowflake announced plans to open-source its Polaris Catalog, which is based on the Apache Iceberg format. The conflict highlights their competition in promoting open-source solutions for data governance [[1](https://siliconangle.com/2024/10/18/big-data-dust-two-ai-giants-war-open/)] [[6](https://venturebeat.com/data-infrastructure/databricks-open-sources-unity-catalog-challenging-snowflake-on-interoperability-for-data-workloads/)] [[11](https://www.infoworld.com/article/2337619/databricks-races-with-snowflake-to-open-up-data-catalog-source-code.html)].

2. **Iceberg vs. Delta Lake (2023-2024)**
   - **Time**: Ongoing since 2023
   - **Reason**: Adoption of open table formats and competition over which format would dominate.
   - **Main Topic**: Snowflake embraced Apache Iceberg to enhance its open-source credentials, whereas Databricks continued to push Delta Lake while also supporting Iceberg through its acquisition of Tabular Technologies in 2024. This acquisition was seen as a strategic move to gain influence over Iceberg's development and compatibility with Delta Lake [[1](https://siliconangle.com/2024/10/18/big-data-dust-two-ai-giants-war-open/)] [[8](https://blog.cloudera.com/databricks-follows-cloudera-by-adopting-iceberg-while-snowflake-mulls-open-source-approach/)] [[10](https://www.chaosgenius.io/blog/iceberg-vs-delta-lake-metadata-indexing/)].

3. **Instacart Migration Drama (2023)**
   - **Time**: August 2023
   - **Reason**: Disputes over customer migration and cost savings.
   - **Main Topic**: Instacart reportedly reduced its Snowflake usage costs by migrating some workloads to Databricks, leading to public disputes about the nature and impact of this migration. The situation was complicated by the fact that Snowflake's CEO was on Instacart's board, raising questions about transparency and influence [[9](https://www.reddit.com/r/dataengineering/comments/166ah28/instacart_databricks_and_snowflake_drama/)] [[12](https://www.snowflake.com/en/blog/snowflake-and-instacart-the-facts/)].

4. **Annual Conference Scheduling (2023)**
   - **Time**: June 2023
   - **Reason**: Competitive positioning during major industry events.
   - **Main Topic**: Both companies scheduled their annual conferences at the same time, forcing shared customers to choose between them. This move underscored the intense rivalry and competition for market dominance in enterprise AI platforms [[2](https://foundationcapital.com/databricks-vs-snowflake-what-their-rivalry-reveals-about-ais-future/)].

These conflicts reflect a broader strategic battle between Databricks and Snowflake as they vie for leadership in the data management and AI platform markets. The focus on open-source solutions, strategic acquisitions, and high-profile customer migrations highlights the dynamic nature of their competition.


Sources

1. [Big-data dust-up: Why two AI giants are at war over who’s more open](https://siliconangle.com/2024/10/18/big-data-dust-two-ai-giants-war-open/)
2. [Databricks vs. Snowflake: What their rivalry reveals about AI's future](https://foundationcapital.com/databricks-vs-snowflake-what-their-rivalry-reveals-about-ais-future/)
3. [Databricks vs Snowflake Wars: 7 Critical Differences - Graphable](https://www.graphable.ai/blog/databricks-vs-snowflake/)
4. [Bill cutting challenges ahead for Snowflake's accountants](https://www.theregister.com/2023/09/05/snowflakes_instacart_protestations_opinion/)
5. [Lakehouse Format Trends to Watch in 2024 - sundeck](https://blog.sundeck.io/lakehouse-formats-trends-to-watch-6c8d0730bf1c?gi=c8bc0c2db5c5)
6. [Databricks open-sources Unity Catalog, challenging Snowflake on interoperability](https://venturebeat.com/data-infrastructure/databricks-open-sources-unity-catalog-challenging-snowflake-on-interoperability-for-data-workloads/)
7. [Will Snowflake Stock Rebound? New CEO, AI Product Initiatives Key](https://www.investors.com/news/technology/snowflake-snow-stock-buy-now/)
8. [Databricks Follows Cloudera by Adopting Iceberg, While Snowflake mulls open-source approach](https://blog.cloudera.com/databricks-follows-cloudera-by-adopting-iceberg-while-snowflake-mulls-open-source-approach/)
9. [Instacart, Databricks and Snowflake drama — r/dataengineering](https://www.reddit.com/r/dataengineering/comments/166ah28/instacart_databricks_and_snowflake_drama/)
10. [Iceberg vs Delta Lake (I) — Metadata Management & Indexing](https://www.chaosgenius.io/blog/iceberg-vs-delta-lake-metadata-indexing/)
11. [Databricks races with Snowflake to open up data catalog source code](https://www.infoworld.com/article/2337619/databricks-races-with-snowflake-to-open-up-data-catalog-source-code.html)
12. [Snowflake and Instacart: The Facts](https://www.snowflake.com/en/blog/snowflake-and-instacart-the-facts/)
13. [Delta Lake Universal Format (UniForm) for Iceberg compatibility, now in GA](https://www.databricks.com/blog/delta-lake-universal-format-uniform-iceberg-compatibility-now-ga)

## The results

After years of competition, the market ends up with only five major players:  BigQuery, Databricks, Redshift, Snowflake, Azure Synapse. I began to hear more and more in Linkedin that people discussing the war between snowflake and databricks like the war between Coca Cola and Pepsi: at the end of the day, both benefit from the war. And I would echo that.

# Appendix: Additional Notes

Snowflake stops showing up in big tech conference in 2025.

Analogy: 
- Databricks is like Honda — they sell you the engine and give you the car for free.
- Snowflake is like Toyota — they sell you a reliable car that just works, but you’ll never open the hood. 

# Appendix: Further Reading

## On Databricks

- [An Architecture for Fast and General Data Processing on Large Clusters (2013)](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2014/EECS-2014-12.pdf)
- [Shark: SQL and Rich Analytics at Scale (2013)](https://dl.acm.org/doi/10.1145/2463676.2465288)
- [Discretized Streams: Fault-Tolerant Streaming Computation at Scale (2013)](https://dl.acm.org/doi/10.1145/2517349.2522737)
- [Spark SQL: Relational Data Processing in Spark (2015)](https://dl.acm.org/doi/10.1145/2723372.2742797)
- [Apache Spark: A Unified Engine for Big Data Processing (2016)](https://dl.acm.org/doi/10.1145/2934664)
- [Structured Streaming: A Declarative API for Real-Time Applications in Apache Spark (2018)](https://people.eecs.berkeley.edu/~matei/papers/2018/sigmod_structured_streaming.pdf)
- [Accelerating the Machine Learning Lifecycle with MLflow](https://people.eecs.berkeley.edu/~matei/papers/2018/ieee_mlflow.pdf)

## On Snowflake

- [The Snowflake Elastic Data Warehouse](https://event.cwi.nl/lsde/papers/p215-dageville-snowflake.pdf)
- [Building An Elastic Query Engine on Disaggregated Storage](https://www.usenix.org/system/files/nsdi20-paper-vuppalapati.pdf)
- [CLOUD DATA WAREHOUSING: HOW SNOWFLAKE IS TRANSFORMING BIG DATA MANAGEMENT](https://iaeme.com/MasterAdmin/Journal_uploads/IJCET/VOLUME_14_ISSUE_3/IJCET_14_03_016.pdf)
