---
layout: distill
title: Data Engineering
description: >-
  My learning progress + notes for data engineering <br>
  Last Edited: August 3, 2026
tags:
  - notes
published: true
giscus_comments: false
date: 2026-07-14
last_edited: 2026-08-03
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
  - name: Foundation
    subsections:
      - name: Fundamentals 
      - name: Data Engineering Workflow 
      - name: Data Lake 
      - name: Data Warehouse
      - name: Data Lakehouse 
  - name: SQL
    subsections:
      - name: SELECT FROM
      - name: Basic Queries
      - name: Constraints and Filtering
      - name: Joins
      - name: Expressions and Data Transformations 
      - name: Case Statements
      - name: NULL Values
      - name: Aggregations
  - name: Python


---

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

#### ETL (Extract Transform Load) vs ELT (Extract Load Transform)
<u>ETL (Extract, Transform, Load)</u>
- Extract data from source systems
- Transform and clean the data before loading it into the destination
- Load only processed data into the data warehouse
- Processing is performed by an external ETL tool or server
- Common in traditional, on-premises data warehouses
- Best when data quality, compliance, or storage is a priority

Flow:
Source Systems → Extract → Transform → Load → Data Warehouse

<u>ELT (Extract, Load, Transform)</u>
- Extract data from source systems
- Load raw data directly into the data warehouse or lakehouse
- Transform the data after loading using the warehouse's compute power
- Preserves raw data for auditing and reprocessing
- Common in modern cloud data platforms
- Better suited for large-scale analytics and machine learning workloads

Flow:
Source Systems → Extract → Load → Data Warehouse/Lakehouse → Transform → BI / Analytics / ML


#### Orchestration 
In charge of when to start jobs, what order to run jobs, what to do if a job fails, and whats the status of the jobs. 

- Manages and automates data pipeline workflows
- Determines when jobs should start (schedules or event triggers)
- Controls the order in which jobs and tasks are executed
- Manages dependencies between tasks (ensures prerequisite jobs finish first)
- Handles failures by retrying jobs, sending alerts, or triggering recovery actions
- Monitors the status and health of pipelines (running, completed, failed)
- Provides logging, monitoring, and notifications for pipeline execution
- Ensures data pipelines run reliably and efficiently

This is commonly done by managing every stage of ELT with DAG (Directed Acyclic Graph) where there is a directed flow or connections with no loops back.

Common Orchestration Tools:
- Apache Airflow
- Prefect
- Dagster
- Azure Data Factory


#### Serving
- BI/Analytics
  - Users: Data Analysts, Business Analysts, Data Engineers
  - Provides reports to explain "what happened" and "what is going on."
  - Interactive dashboards and visualizations for business users to explore data
  - Technology: Power BI, Tableau, Excel, Looker

- ML/AI
  - Users: Data Scientists, AI/ML Engineers
  - Uses curated datasets to build predictive and machine learning models.
  - Develops AI applications and model-driven insights using Python.
  - Technology: Python, Jupyter Notebooks, TensorFlow, PyTorch, Scikit-learn

- Reverse ETL
  - Sends cleaned, transformed, and enriched data from the warehouse back into operational systems.
  - Makes analytics available in day-to-day business tools for sales, marketing, customer support, and operations.
  - Examples: Salesforce, HubSpot, Marketo, Zendesk, Slack

#### CI/CD
- Automates the process of building, testing, and deploying code changes
- Enables teams to release updates quickly, consistently, and with fewer errors
- Ensures code is tested before being deployed to production
- Reduces manual deployment effort and improves software reliability
- Supports version control integration (e.g., Git) for collaborative development

<u>Continuous Integration (CI)</u>
- Automatically builds and tests code whenever changes are committed
- Detects bugs and integration issues early
- Runs unit tests, code quality checks, and validation

<u>Continuous Deployment/Delivery (CD)</u>
- Automatically deploys validated code to development, staging, or production environments
- Ensures deployments are repeatable and consistent
- Can include approval gates before production deployment (Continuous Delivery) or fully automated production releases (Continuous Deployment)


#### Data Architecture 

Data architecture is the overall blueprint for how data flows through an organization, from collection to analysis. Data is collected through ingestion using batch or streaming methods, stored in systems such as data lakes, data warehouses, or lakehouses, and processed using ETL or ELT pipelines. Cloud platforms provide the scalability needed to manage these systems. OLTP systems create and manage operational data, while OLAP systems analyze data for insights and reporting. The medallion architecture organizes data into Bronze, Silver, and Gold layers to improve data quality and usability. Together, these components create a reliable system for storing, transforming, and serving data for analytics, applications, and machine learning. 



### Data Engineering Workflow 
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

### Data Lake
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

### Data Warehouse
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

     
### Data Lakehouse 
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

<!-- ----------------------------------------------------------------------------------  -->

## SQL 
Structured Query Language (SQL) is used to manage, manipulate, and retrieve data stored in relational databases. 
- Where does SQL run? 
  - SQL runs on a database management system (DBMS), which can be installed on your local computer or hosted on a remote server. The database engine executes your SQL queries and returns the results.
- With SQL, you typically don't need to worry about data size because modern warehouses automatically scale compute across multiple machines 
- SQL is great working withe structured data like joining tables and aggregating grouping
- SQL doesn't do great when classifying messy text, feature engineering, or nested JSON data, which is where Python comes in for transformation (especially for AI and machine learning) 

### Database Hierarchy  
- A database can contain multiple schemas
- Each schema is a logical container within the database that holds its own tables, views, functions, procedures, and other database objects 



### dbt (databuild tools)
**What is dbt?**
- dbt (Data Build Tool) is used to manage and organize SQL based data transformations in a data warehouse.
- It helps transform raw data into clean, analytics ready datasets using SQL.
When to use dbt
- Use dbt when working with data warehouses that require structured, maintainable SQL transformations.

**It is especially useful for projects with:**
- Many tables
- Complex transformation logic
- Multiple developers collaborating
- Version control and testing requirements

**When dbt may not be necessary:**
- For simple transformations involving only a few tables, writing plain SQL scripts may be sufficient.
- In small projects, introducing dbt can add unnecessary complexity.



### SELECT FROM

- In order to retrieve or query data from a database, we need to use a <u>SELECT</u> statement
- A query is a statement used to retrieve, manipulate, or manage data stored within a relational database
- You can select particular columns from a table:
```
SELECT column, another_column, …
FROM mytable;
```

- Or if we want to retrieve all columns of data from a table, we use the asterisk (*):
```
SELECT *
FROM my table;
```

### Basic Queries 

- <u>WHERE</u> is used to to filter out certain results based on certain conditions
```
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

- <u>GROUP BY</u> discards duplicates based on specific columns and used to group rows that have the same values into summary rows, like "Find the number of customers in each country" 
  -  Almost always used in conjunction with aggregation functions, like COUNT(), MAX(), MIN(), SUM(), AVG(), to perform calculations on each group
  - ``` 
    SELECT column1, aggregate_function(column2), column3, ...
    FROM table_name
    WHERE condition
    GROUP BY column1, column3
    ORDER BY column_name;
    ```
- <u>HAVING</u> is used to filter groups after they have been created with GROUP BY. Unlike WHERE, which filters individual rows before grouping, HAVING filters the aggregated results (often using aggregate functions like COUNT(), SUM(), AVG(), etc.).

  ```
  SELECT column1, aggregate_function(column2)
  FROM table_name
  WHERE condition
  GROUP BY column1
  HAVING aggregate_function(column2) condition
  ORDER BY column1;
  ```

  Example: Find countries that have more than 5 customers.
  ```
  SELECT country, COUNT(*) AS num_customers
  FROM customers
  GROUP BY country
  HAVING COUNT(*) > 5;
  ```
  -  <u>WHERE</u> → Filters individual rows before grouping
  - <u>HAVING</u>→ Filters grouped results after grouping
  Example using both:
  ```
  SELECT department, AVG(salary) AS avg_salary
  FROM employees
  WHERE salary > 30000
  GROUP BY department
  HAVING AVG(salary) > 50000;
  ```
  - WHERE salary > 30000 removes employees earning $30,000 or less before grouping
  - HAVING AVG(salary) > 50000 only returns departments whose average salary is greater than $50,000

- <u>LIMIT</u> limits the particular amount of rows you are querying 
```
SELECT column, another_column, …
FROM mytable
LIMIT 4;
```
- <u>OFFSET</u> is used to specify what row to begin from 
```
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
LIMIT num_limit OFFSET num_offset;
```

- <u> DISTINCT </u> can be used to discard rows that have a duplicate column value
``` 
SELECT DISTINCT column, another_column, … 
```
- <u>ORDER BY </U> is used to sort columns by ascending or descending order
```
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
```


### Constraints and Filtering
- In order to filter out certain results, we can use the <u>WHERE</u> clause along with certain condition statements 
```
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```
- Complex clauses can be constructed by joining numerous AND or OR logical keywords

| Operator | Condition | SQL Example |
|----------|-----------|-------------|
| `=`, `!=`, `<`, `<=`, `>`, `>=` | Standard numerical comparison operators | `col_name != 4` |
| `BETWEEN ... AND ...` | Number is within a range of two values (inclusive) | `col_name BETWEEN 1.5 AND 10.5` |
| `NOT BETWEEN ... AND ...` | Number is not within a range of two values (inclusive) | `col_name NOT BETWEEN 1 AND 10` |
| `IN (...)` | Number exists in a list | `col_name IN (2, 4, 6)` |
| `NOT IN (...)` | Number does not exist in a list | `col_name NOT IN (1, 3, 5)` |
| `=` | Exact string comparison.<br>Case sensitivity depends on the database/collation. | `col_name = "abc"` |
| `!=` or `<>` | Exact string inequality comparison.<br>Case sensitivity depends on the database/collation. | `col_name != "abcd"` |
| `LIKE` | Pattern matching.<br>Without wildcards (`%` or `_`), it matches the entire string.<br>Case sensitivity depends on the database/collation. | `col_name LIKE "ABC"` |
| `NOT LIKE` | Negates `LIKE` pattern matching.<br>Case sensitivity depends on the database/collation. | `col_name NOT LIKE "ABCD"` |
| `%` | Matches zero or more characters.<br>Used only with `LIKE` or `NOT LIKE`. | `col_name LIKE "%AT%"`<br>Matches `"AT"`, `"ATTIC"`, `"CAT"`, or `"BATS"` |
| `_` | Matches exactly one character.<br>Used only with `LIKE` or `NOT LIKE`. | `col_name LIKE "AN_"`<br>Matches `"AND"`, but not `"AN"` |
| `IN (...)` | String exists in a list | `col_name IN ("A", "B", "C")` |
| `NOT IN (...)` | String does not exist in a list | `col_name NOT IN ("D", "E", "F")` |


### Joins
- <u>JOIN</u> is used to combine row data across two separate tables based on a related column, or unique key, between them
- <u>INNER JOIN</u> combines rows from two or more tables by looking for matching values in a shared column. It returns only the rows that find a match in both tables, leaving out any data that does not match
  ```
  SELECT column, another_table_column, …
  FROM mytable
  INNER JOIN another_table 
      ON mytable.id = another_table.id
  WHERE condition(s)
  ORDER BY column, … ASC/DESC
  LIMIT num_limit OFFSET num_offset;
  ```

  Example:
  ```
  SELECT employees.name, departments.department_name
  FROM employees
  INNER JOIN departments
      ON employees.department_id = departments.id;
  ```

- <u>LEFT JOIN</u> (or LEFT OUTER JOIN) returns all rows from the left table and the matching rows from the right table. If there is no match, the columns from the right table contain NULL
  ```
  SELECT column, another_table_column, …
  FROM mytable
  LEFT JOIN another_table
      ON mytable.id = another_table.id
  WHERE condition(s)
  ORDER BY column ASC/DESC
  LIMIT num_limit OFFSET num_offset;
  ```

  Example:
  ```
  SELECT employees.name, departments.department_name
  FROM employees
  LEFT JOIN departments
      ON employees.department_id = departments.id;
  ```
  This query returns every employee, even if they are not assigned to a department.

- <u>RIGHT JOIN</u> (or RIGHT OUTER JOIN) returns all rows from the right table and the matching rows from the left table. If there is no match, the columns from the left table contain NULL 
  ```
  SELECT column, another_table_column, …
  FROM mytable
  RIGHT JOIN another_table
      ON mytable.id = another_table.id
  WHERE condition(s)
  ORDER BY column ASC/DESC
  LIMIT num_limit OFFSET num_offset;
  ```

  Example:
  ```
  SELECT employees.name, departments.department_name
  FROM employees
  RIGHT JOIN departments
      ON employees.department_id = departments.id;
  ```
  This query returns every department, even if no employees belong to it.
  You can usually achieve the same result by swapping the table order and using a LEFT JOIN 

- <u>FULL JOIN</u> (or FULL OUTER JOIN) returns all rows from both tables. Matching rows are combined, while non-matching rows from either table are included with NULL values for the missing side
  ```
  SELECT column, another_table_column, …
  FROM mytable
  FULL JOIN another_table
      ON mytable.id = another_table.id
  WHERE condition(s)
  ORDER BY column ASC/DESC
  LIMIT num_limit OFFSET num_offset;
  ```

  Example:
  ```
  SELECT employees.name, departments.department_name
  FROM employees
  FULL JOIN departments
      ON employees.department_id = departments.id;
  ```
  This query returns:
  - employees with matching departments,
  - employees without a department,
  - departments without any employees.
  Note: MySQL does not support FULL JOIN directly. It can be simulated using a LEFT JOIN, a RIGHT JOIN, and UNION.

### Expressions and Data Transformations 

- An <u>expression</u> is a calculation or transformation performed on one or more columns. Expressions are evaluated when the query runs and can be used to clean, format, or calculate new values without modifying the original data

- Expressions can be used in the `SELECT`, `WHERE`, `ORDER BY`, and other clauses
  ```
  SELECT expression
  FROM mytable;
  ```
  Example:
  ```
  SELECT particle_speed / 2.0 AS half_particle_speed
  FROM physics_data
  WHERE ABS(particle_position) * 10.0 > 500;
  ```


#### Aliases (`AS`)
- <u>AS</u> is used to assign a temporary name (alias) to a column, expression, or table
- Aliases make query results easier to read and simplify references in complex queries
  Column alias:
  ```
  SELECT first_name AS employee_name
  FROM employees;
  ```

  Expression alias:
  ```
  SELECT salary * 1.10 AS increased_salary
  FROM employees;
  ```

  Table alias:
  ```
  SELECT e.name, d.department_name
  FROM employees AS e
  INNER JOIN departments AS d
      ON e.department_id = d.id;
  ```


#### Common Expressions for Data Cleaning

<u>Math Operations</u>
- Performs calculations on numeric data

```
SELECT price * quantity AS total_cost
FROM orders;
```

Common operators:
- `+` Addition
- `-` Subtraction
- `*` Multiplication
- `/` Division
- `%` Modulus (remainder)

<u>Mathematical Functions</u>

| Function | Description | Example |
|----------|-------------|---------|
| `ABS(x)` | Absolute value | `ABS(-5)` → `5` |
| `ROUND(x, n)` | Round to `n` decimal places | `ROUND(price, 2)` |
| `CEIL(x)` | Round up | `CEIL(3.2)` → `4` |
| `FLOOR(x)` | Round down | `FLOOR(3.9)` → `3` |

Example:
```
SELECT ROUND(price, 2) AS rounded_price
FROM products;
```

<u>String Functions</u>

Useful for cleaning inconsistent text data

| Function | Description |
|----------|-------------|
| `UPPER()` | Converts text to uppercase |
| `LOWER()` | Converts text to lowercase |
| `TRIM()` | Removes leading and trailing spaces |
| `LENGTH()` | Returns the length of a string |
| `CONCAT()` | Combines multiple strings |

Examples:
```
SELECT UPPER(first_name) AS first_name
FROM employees;
```

```
SELECT TRIM(email)
FROM customers;
```

```
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```


#### Why use expressions?

Expressions allow you to:
- Clean messy data (remove spaces, standardize capitalization)
- Perform calculations without changing the original data
- Create more readable output
- Avoid extra processing in another programming language (Python, Java, etc.)

For example:
```
SELECT
    TRIM(UPPER(first_name)) AS cleaned_name,
    ROUND(price * quantity, 2) AS total_cost
FROM orders;
```

This query:
- Removes extra whitespace
- Converts names to uppercase
- Calculates the total order cost
- Gives each calculated value a descriptive alias


### Case Statements
- <u>CASE</u> is SQL's version of an `if-else` statement. It evaluates conditions in order and returns a value for the first condition that is true
- Commonly used to categorize data, create labels, or clean inconsistent values

  ```
  SELECT
      column,
      CASE
          WHEN condition1 THEN result1
          WHEN condition2 THEN result2
          ELSE default_result
      END AS alias_name
  FROM mytable;
  ```
  Example:
  ```
  SELECT
      name,
      salary,
      CASE
          WHEN salary >= 100000 THEN 'High'
          WHEN salary >= 50000 THEN 'Medium'
          ELSE 'Low'
      END AS salary_category
  FROM employees;
  ```

  This query categorizes each employee's salary as **High**, **Medium**, or **Low**.

  <u>Common uses: </u>
  - Categorizing values into groups
  - Replacing or standardizing values
  - Handling missing or special cases
  - Creating more readable output


### NULL Values

- <u>NULL</u> represents a **missing or unknown value**
- It is **not** the same as `0`, an empty string (`''`), or `FALSE`
- Because `NULL` means "unknown," it cannot be compared using `=` or `!=`

Check for `NULL` values:
```
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

Check for `non-NULL` values:
```
SELECT *
FROM employees
WHERE manager_id IS NOT NULL;
```


#### Working with NULL Values

- Use <u>COALESCE()</u> to replace `NULL` with a default value
- `COALESCE()` returns the first non-`NULL` value from a list of expressions
  Example:
  ```
  SELECT
      name,
      COALESCE(phone_number, 'No phone number') AS phone
  FROM customers;
  ```

  If `phone_number` is `NULL`, the query returns `"No phone number"` instead


#### NULL in Aggregate Functions

Most aggregate functions ignore `NULL` values.

```
SELECT
    COUNT(*) AS total_rows,
    COUNT(phone_number) AS phone_numbers
FROM customers;
```

- `COUNT(*)` counts **all** rows
- `COUNT(phone_number)` counts only rows where `phone_number` is **not** `NULL`

<u>Common Uses:</u>
- Finding missing data (`IS NULL`)
- Filtering out missing values (`IS NOT NULL`)
- Replacing missing values (`COALESCE()`)
- Understanding how aggregate functions handle missing data

### Aggregations 

- <u>Aggregate functions</u> summarize data from multiple rows into a single value
- They are commonly used to calculate totals, averages, minimums, maximums, and counts
- Aggregate functions ignore `NULL` values (except `COUNT(*)`, which counts every row)

  Basic syntax:
  ```
  SELECT AGG_FUNC(column) AS alias
  FROM mytable
  WHERE condition;
  ```

  Example:
  ```
  SELECT COUNT(*) AS total_employees
  FROM employees;
  ```

#### Common Aggregate Functions

| Function | Description | Example |
|----------|-------------|---------|
| `COUNT(*)` | Counts all rows | `COUNT(*)` |
| `COUNT(column)` | Counts non-`NULL` values in a column | `COUNT(email)` |
| `MIN(column)` | Returns the smallest value | `MIN(salary)` |
| `MAX(column)` | Returns the largest value | `MAX(salary)` |
| `AVG(column)` | Returns the average value | `AVG(salary)` |
| `SUM(column)` | Returns the total of all values | `SUM(sales)` |

  Example:
  ```
  SELECT
      COUNT(*) AS employees,
      AVG(salary) AS average_salary,
      MAX(salary) AS highest_salary
  FROM employees;
  ```


<!-- ----------------------------------------------------------------------------------  -->

## Python
Python is a multipurpose programming language that is used for analyzing data, automating, machine learning, and in many more cases. 

- How does Python work? 
  - Python acts as the orchestration layer in the ELT (Extract, Load, Transform) process. It executes SQL queries in the required order, while the database performs the data extraction, loading, and transformation. Python is responsible for coordinating the workflow, managing execution, and capturing the output and logs.
- Where does Python run? 
  - Python can run locally on your computer or a remote computer/server for production.

### Apache Spark 
**What is Apache Spark?**
- Apache Spark is a distributed data processing framework used to scale Python, SQL, Java, and Scala data transformations across multiple machines
- It enables processing of very large datasets that cannot be handled efficiently on a single computer.
- Splits work across a cluster of machine to process, ingest, and transform data

**When to use Apache Spark:**
- Processing large datasets (GBs to TBs or more)
- Scaling Python transformations across a cluster
- Distributed ETL pipelines
- Batch processing and streaming data
- Machine learning on large datasets using Spark MLlib

**When Spark may not be necessary:**
- For small datasets that fit comfortably in memory, standard Python (e.g., pandas) is often simpler and faster.
- Spark introduces overhead, so it is generally most beneficial when data size or computation complexity justifies distributed processing.



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
