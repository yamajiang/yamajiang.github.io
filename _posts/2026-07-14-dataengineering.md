---
layout: distill
title: data engineering
description: >-
  my learning progress + notes for data engineering <br>
  last edited: july 14, 2026
tags:
  - notes
published: true
giscus_comments: false
date: 2026-07-14
last_edited: 2026-07-14
featured: true

_styles: >
  .post.distill p,
  .post.distill li {
    line-height: 1.5;
    margin-bottom: 0.45rem;
  }

  .post.distill h2,
  .post.distill h3 {
    margin-top: 1rem;
    margin-bottom: 0.35rem;
  }

  .post.distill ul {
    margin-top: 0.25rem;
    margin-bottom: 0.5rem;
  }

toc:
  - name: Introduction
  - name: Foundation
    subsections:
      - name: Fundamentals 


---

## Introduction

This is my running notebook for learning data engineering :)

My goal is to build a practical foundation in:
- data modeling
- data pipelines
- storage systems
- orchestration
- data quality and observability

### General Data Workflow
```
                               Raw Data
                                   │
                                   ▼
                            Data Engineer
                  • collects, cleans, transforms
                and stores data by building data pipelines
                                   │
                    ┌──────────────┴───────────────┐
                    ▼                              ▼
              Data Analyst                   Data Scientist
          • analyzes and queries       • uses statistic and ml to
        data to produce reports,     build predictive models test
      metrics, and visualizations    hypothesis, and uncover patterns
                    │                               │
                    └──────────────┬────────────────┘
                                   ▼
                              ML Engineer
                   • takes validated models & data to 
                  build scalable machine learning models 
                     and maintains ml infrastructure
                                   │
                  ┌────────────────┴──────────────┐
                  ▼                               ▼
        MLOps Platform Engineer             AI / Product Engineer
   • automates deployment, scaling,        • integrates ML/AI into apps using
     monitoring, and CI/CD                  LLMs, APIs, and prompt engineering
                                            to build AI-powered user features
```

## Foundation
 
This section includes notes on foundations and specific tools for data engineering. This is to help build a strong understanding of the fundamental concepts, technologies, and best practices required to design, build, and maintain reliable data pipelines.

I used Luke Barousse's bootcamp to help me with understanding some of the fundamentals. 

### Fundamentals

#### What is Data Engineering?

Data engineering is designing, building pipelines, and maintaining systems that move and transform data that can be from multiple different sources so that it is reliable and usable for analytics, machine learning, and products.


Most data engineering pipelines follow four stages:

Ingestion → Storage → Transformation → Serving
- **Ingestion**: Data ingestion is the process of collecting data from multiple sources such as databases, APIs, applications, and streaming platforms (in batches or real time). The goal is to bring raw data into a centralized system for processing.
- **Transformation**: Data transformation involves cleaning, validating, and converting raw data into a structured and usable format. This includes removing duplicates, handling missing values, and applying business rules. The transformed data becomes consistent and ready for analysis.
- **Serving**: Data serving is the process of making processed data available to users, dashboards, applications, or machine learning models. It ensures data can be accessed quickly and efficiently. Common serving methods include data warehouses, APIs, and analytics platforms.
- **Storage**: Data storage is where raw and processed data is securely kept for future use. It can include data lakes, data warehouses, or cloud storage systems. Good storage solutions provide scalability, reliability, and fast data retrieval.

In data engineering, ACID transactions (Atomicity, Consistency, Isolation, Durability) help ensure data integrity across large scale pipelines. They guarantee that complex ETL (Extract, Transform, Load) operations are executed reliably, even in the presence of system failures or concurrent access. 

- Atomicity ensures that all steps in a transaction either complete successfully or are rolled back entirely. 
- Consistency guarantees that every transaction moves the database from one valid state to another while preserving data integrity rules. 
- Isolation prevents concurrent transactions from interfering with each other, ensuring predictable results. 
- Durability ensures that once a transaction is committed, the changes are permanently stored and can be recovered even after a system crash. 

Together, these properties enable robust, reliable, and fault-tolerant data processing in modern data engineering systems.


#### Data Engineering Workflow 
```

                             Data Sources
                   • this is where the data originates  
              ex: apps, robots, equipment, sensors, apis, logs 
                                   │
                                   │
                                   ▼
                               Ingestion
              • where data is collected & moved into storage
        1. batch ingestion: runs on a scheduled interval (hrs, daily) 
          ex: every night, sales are copied to the cloud storage 
        2. streaming ingestion: data is transferred continuously 
          ex: car moves -> GPS updates -> app location and 
              navigation directions update in real time
                                   │
                                   │
                                   ▼
                          Storage (Data Lake)
              • raw data is stored in it original format 
             • scalable storage that holds unstructured, 
               semi structured, and structured data
        ex: JSON, CSV, Parquet, log files, images, videos, XML
                                   │
                                   │
                                   ▼
                     Processing & Transformations
       • raw data is cleaned, validated, transformed for analysis
    ex: remove duplicates, handle missing values, standardize, conversions 
                                   │
                                   │
                                   ▼
                           Data Warehouse
       • processed data stored in structured & optimized database
    • data from multiple sources, cleaned, put into tables for queries 
                                   │
                                   │
                                   ▼
                         Dashboard Analytics
        • different teams can use data for different purposes
    ex: visuals & reports - daily sales, growth trends, performance               

```


#### Data Lake
A Data Lake is a large, low cost storage system that stores raw data in its original format, regardless of its structure 
  - Stores structured, semi-structured, and unstructured data
  - Supports many different types of files such as: CSV, JSON, Parquet, images, videos, audio, sensor data, logs 
  - Data is stored before being cleaned or transformed 
  - Uses Schema-on-Read, meaning the schema is applied only when the data is queried.
  - **Best Use Cases**:
    - Machine Learning
    - Data Science
    - Large scale archival storage 
    - Sensor data
    - Log analytics 
  - **Advantages**:
    - Inexpensive storage
    - Highly scalable
    - Supports many data types
    - Retains raw data
  - **Disadvantages**: 
    - Query performance is usually slower than warehouses
    - Data quality is not enforced

#### Data Warehouse
A Data Warehouse is a centralized repository that stores structured, cleaned, and transformed data optimized for analytics
  - Stores only structured data
  - Data is typically cleaned and transformed before loading (ETL or ELT)
  - Optimized for SQL queries and analytical workloads
  - **Best Use Cases**:
    - Business Intelligence
    - Financial analysis 
    - Reporting and dashboards
    - KPI tracking 
  - **Advantages**:
    - Fast SQL query performance
    - High data quality and consistency 
    - Easy for analysts and business user
  - **Disadvantages**:
    - Limited support for unstructured data
    - High implementation costs  
    - Less flexible when data formats frequently change 
    - Traditional data warehouses rely on batch processing so they are often unable to deliver real time data

     
#### Data Lakehouse 
A Data Lakehouse combines the flexibility and low-cost storage of a Data Lake with the reliability, governance, and SQL performance of a Data Warehouse.

Instead of copying data from a lake into a warehouse, a lakehouse allows analytics and machine learning to operate directly on files stored in the data lake.
- Stores raw and processed data in open file formats (typically Parquet)
- Supports SQL querying directly on data lake files
- Eliminates the need to maintain separate lake and warehouse systems
- Provides ACID transactions for reliable updates
- **Best Use Cases**:
  - Large scale analytics
  - Modern data engineering pipelines
  - Streaming and batch processing
  - Unified data platforms
  - Machine learning workflows
  - AI applications
- **Advantages**:
 - Combines low-cost storage with high-performance analytics
 - Single platform for analytics, AI, and ML
 - Supports open data formats
- **Disadvantages**:
  - More complex architecture than a warehouse
  - Managing security, metadata, data quality, and compliance across structured and unstructured data can be challenging
  - Query performance may not always match that of a highly optimized data warehouse, especially for complex analytical workloads (Performance tuning is often required)


#### Medallion Architecture 

Medallion data architecture is a design pattern used to logically organize date to help improve structure and quality. It consists of 3 progressive layers:

1. Bronze Layer (Raw Data)
  - Stores data exactly how it arrives from sources
  - Raw and unprocessed
  - Original state helps preserve ground truth
  - used for auditing and reprocessing

2. Silver Layer( Cleaned & Structured Data)
  - Cleans, filters, validates, and standardizes raw data
  - Removes duplicates
  - Handles any missing values
  - Fix data types and standardize 

3. Gold Layer (Aggregated Data)
  - Create data that is ready for use for different purposes
  - Feature engineering 
  - Aggregations and calculations
  - Metrics and tables 
  - KPI (Key performance indicator) - serves as the standardized, analytical calculations that track health and performance.

#### Normalized vs Denormalized Tables
- Normalized tables are multiple tables that are linked by keys or id values. Data redundancy is minimized or eliminated, but it is slower because it requires multiple joins.
- Denormalized tables have fewer and "wider" tables which can happen by merging information that are stored in separate tables together. This intentionally introduces data redundancy for faster queries. The trade off is increased storage usage that is use to keep data synchronized.

#### OLTP vs OLAP
OLTP and OLAP are two types of database systems designed for different purposes. 
- Online Transaction Processing (OLTP) handles real time day to day transactions. 
  - Fast read, write, update, delete operations
  - Slow at scanning and aggregating data
  - Database designs are typically normalized tables 
  - Examples: e-commerce orders, ATM transactions, banking,

- Online Analytical Processing (OLAP) supports business analysis and decision making by analyzing historical data.  
  - Fast at scanning and aggregating data
  - Slow at handling many updates
  - Database designs are often denormalized
  - Examples: sales reporting, forecasting, business intelligence  




<!-- ### Stack Decisions (To Fill In)

- Compute engine: TBD
- Warehouse: TBD
- Orchestrator: TBD
- Transformation framework: TBD
- Monitoring: TBD

### Template for Future Entries

Use this format for each update:

```markdown
### YYYY-MM-DD
- What I learned:
- What I built:
- What confused me:
- Next step:
``` -->

<!-- ## Questions and Next Steps

### Open Questions

- Which cloud/data platform should I focus on first?
- How deep should I go into streaming early on?
- What project can best demonstrate end-to-end pipeline skills?

### Next Steps

- Start with SQL and data modeling notes.
- Add first mini-project architecture.
- Update `last_edited` on every meaningful change. -->
