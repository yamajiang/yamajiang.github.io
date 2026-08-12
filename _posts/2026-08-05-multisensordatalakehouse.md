---
layout: distill
title: Multi-Sensor Data Lakehouse
description: >-
  Deep dive into the architecture and implementation of <br>
    my Multi-Sensor Data Lakehouse project <br>
    Last Edited: August 11, 2026
tags:
  - projects
published: false
giscus_comments: false
date: 2026-08-11
last_edited: 2026-08-11
featured: false

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
  - name: Problem and Approach
  - name: Dataset 
  - name: System Architecture and Data Pipeline

    

---

## Introduction

Robotic systems generate large amounts of telemetry data from different components and sensors, including cameras, LiDAR, GPS, IMUs, and localization systems. This data is commonly stored in ROS bag files, which are used to record and replay robotic activity, but they are not designed for modern data engineering and analytical workflows.

For this project, I am building a data lakehouse that transforms raw ROS bag files into structured, analytics ready datasets without requiring a ROS runtime. The goal is to take the high-volume, asynchronous telemetry produced by an autonomous vehicle and transform it into a format that can be efficiently queried, synchronized, analyzed, and eventually replayed.

The project follows a **Bronze → Silver → Gold** architecture. Raw ROS messages are first extracted into Apache Parquet files, then cleaned and standardized before being synchronized across sensor streams. The resulting Gold datasets will provide a unified view of vehicle state that can be queried using DuckDB and used for applications such as sensor analysis, mission statistics, debugging, and fleet-level analytics.

I am using the **Ford Multi-AV Seasonal Dataset** as the primary data source. It includes ROS1 bag files containing real-world autonomous vehicle telemetry collected across different environments, seasons, and driving conditions. This makes it a useful dataset for exploring the challenges of working with large scale robotics data.

In this post, I will document the ongoing process of building the project — from extracting ROS messages and handling timestamps to synchronizing asynchronous sensors, building analytical datasets, and exploring technologies such as Parquet, DuckDB, Spark, Delta Lake, and Databricks.

## Problem and Approach
Autonomous vehicles and robotic systems continuously produce heterogeneous telemetry from dozens of sensors operating at vastly different sampling frequencies.

This creates several challenges:
- ROS Dependency
    - Accessing robotics telemetry traditionally requires a configured ROS environment, which creates unnecessary overhead for data analysts and engineers performing offline analysis on historical data.
- Asynchronous Sensor Streams
    - Independent sensors publish at different frequencies and timestamps, which prevents direct connections between datasets. 
- Real Time Replay 
    - Historical analysis is commonly performed using rosbag play, forcing engineers to replay missions in real time. Increasing playback speed frequently results in dropped messages and unstable playback.
- Scalable Analytics
    - Robotics logs are optimized for real-time message transport rather than analytical workloads, making SQL queries and fleet-wide analysis inefficient 

Rather than relying on a traditional ROS-based workflow, this project transforms robotics telemetry into a layered data lakehouse optimized for both large scale analytics and historical replay.

The ingestion pipeline extracts raw ROS bag data and organizes it into progressively refined datasets following a Bronze → Silver → Gold architecture.


##  Dataset 
This project uses the Ford Motor Company large-scale autonomous vehicle telemetry dataset that was collected across multiple seasons and environmental conditions. The dataset is distributed in ROS bag format, making it an ideal source for demonstrating robotics data ingestion, time series synchronization, and lakehouse analytics workflows.

### Dataset Links

- Dataset Paper: [Ford Multi-AV Seasonal Dataset Paper](https://arxiv.org/abs/2003.07969)

- Official Dataset Page: [Ford Multi-AV Seasonal Dataset](https://avdata.ford.com/)

The core challenge addressed by this project is transforming asynchronous robotics telemetry into a unified, analytics-ready format.

The dataset includes heterogeneous sensor streams such as:

- IMU
- GPS
- Cameras
- LiDAR
- Vehicle Pose
- Localization
- Transform Frames

These sensors operate at different frequencies, making the dataset an ideal benchmark for time-series synchronization and robotics lakehouse analytics.


## System Architecture and Data Pipeline
The target architecture for the completed pipeline is shown below. This may change overtime as I progress in development. 
```
                    ROS Bag (.bag)
                           │
                           ▼
               ROS Runtime-Free Ingestion
    (Extract ROS Topics to Parquet → Structured Tables)
                           │
                           ▼
                  Bronze Layer (Raw)
            • One table per ROS topic
            • Original timestamps preserved
            • Immutable raw telemetry
                           │
                           ▼
                Silver Layer (Cleaned)
            • Normalize timestamps
            • Flatten ROS messages
            • Standardize schemas
            • Remove invalid records
                           │
                           ▼
            Multi-Sensor Time Synchronization
            • Master timeline generation
            • As-Of nearest-neighbor joins
            • Time-series interpolation
                           │
                           ▼
                Gold Layer (Unified)
            • Synchronized state table
            • Analytics-ready telemetry
            • Consistent sensor alignment
                           │
                           ▼
             Apache Parquet Lakehouse
                           │
                           ▼
                  DuckDB SQL Engine
                  /                 \
                 /                   \
                ▼                     ▼
      Fleet Analytics        Historical Replay
      • SQL exploration      • Variable-speed playback
      • Mission statistics   • Timeline reconstruction
      • Sensor analysis      • Debugging & validation
``` 

