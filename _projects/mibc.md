---
layout: page
title: Multimodal Survival Prediction in Muscle Invasive Bladder Cancer
description: 
img: assets/img/wsi.png
importance: 2
category: academic
---

- Multimodal deep learning framework to predict patient survival by integrating whole-slide pathology image embeddings with clinical and treatment data.
- Extracted pathology embeddings from foundation models including Prov-GigaPath, UNI, UNI2, and ResNet-18, and implemented attention-based multiple instance learning (MIL) architectures.
- Engineered preprocessing pipelines for TCGA-BLCA data, including pathology patch extraction, clinical feature encoding, treatment representation, and train/validation/test dataset generation.
- Designed a treatment-conditioned multimodal fusion model using PyTorch and optimized survival prediction with Cox proportional hazards loss and attention mechanisms.
- Evaluated six survival prediction models using the concordance index (C-index), Kaplan-Meier analysis, and log-rank testing; achieved a 0.7385 C-index with the treatment-conditioned GigaPath model.
