---
layout: page
title: Sentinel Grid
img: assets/img/sentinel.png
importance: 1
category: academic
images:
  slider: true
---

<swiper-container navigation="true" pagination="true" keyboard="true" style="display: block; width: min(100%, 900px); margin: 1.5rem auto;">
  <swiper-slide>
    <img src="{{ '/assets/img/sentinel/sentinel_dash.png' | relative_url }}" alt="Sentinel Grid dashboard" style="display: block; width: 100%; height: auto;">
  </swiper-slide>
  <swiper-slide>
    <img src="{{ '/assets/img/sentinel/sentinel_sessions.png' | relative_url }}" alt="Sentinel Grid session details" style="display: block; width: 100%; height: auto;">
  </swiper-slide>
  <swiper-slide>
    <img src="{{ '/assets/img/sentinel/sentinel_analytics.png' | relative_url }}" alt="Sentinel Grid threat analytics" style="display: block; width: 100%; height: auto;">
  </swiper-slide>
  <swiper-slide>
    <img src="{{ '/assets/img/sentinel/sentinel_distribution.png' | relative_url }}" alt="Sentinel Grid deployment distribution" style="display: block; width: 100%; height: auto;">
  </swiper-slide>
  <swiper-slide>
    <img src="{{ '/assets/img/sentinel/sentinel_map.png' | relative_url }}" alt="Sentinel Grid map view" style="display: block; width: 100%; height: auto;">
  </swiper-slide>
</swiper-container>

SentinelGrid is a Senior Design project developed by a team of five to build an end-to-end cyber-defense platform that uses honeypot activity to analyze attacker behavior and dynamically recommend how defensive resources should be deployed.

As the Machine Learning & Threat Analysis Engineer, I designed and implemented the machine learning pipeline that transforms raw honeypot activity into attacker profiles and deployment recommendations.

- Developed a five stage pipeline that normalizes raw honeypot logs, engineers session-level features, analyzes behavior, labels attacker activity, and generates deployment recommendations
- Extracted features from authentication attempts, command activity, session timing, file transfers, protocol/service usage, and password characteristics to model attacker behavior at the session level
- Applied PCA, KMeans clustering, and Isolation Forest anomaly detection to identify behavioral groupings and unusual attack sessions
- Created a heuristic labeling system that classifies sessions into 14 attacker profiles—including brute-force attacks, credential stuffing, web scanning, malware downloads, database attacks, and low-interaction SSH probes—with confidence scores and supporting evidence
- Designed a confidence aware resource allocation engine that evaluates the latest 100 sessions, filters uncertain results, normalizes demand by currently deployed honeypot counts, and recommends how to distribute a fixed pool of 12 honeypots across SSH, HTTP, MySQL, Redis, FTP, and SMTP services
- Integrated the pipeline with the backend so the platform can process current logs and return JSON deployment recommendations for dynamic honeypot scaling
- Prototyped and validated the workflow using Cowrie honeypot logs, the Zenodo CyberLab Honeynet dataset, and CIC-IDS-2017 network-intrusion data
