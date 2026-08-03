---
layout: page
title: Multimodal Survival Prediction in Muscle Invasive Bladder Cancer
description: 
img: assets/img/wsi.png
importance: 2
category: academic
---

* Developed a multimodal deep learning framework for survival prediction in Muscle Invasive Bladder Cancer (MIBC) by integrating H&E whole-slide pathology images, clinical metadata, and treatment information from the TCGA-BLCA dataset.
* Built a preprocessing pipeline for 110 patients with matched pathology, clinical, and survival data, including clinical feature encoding, treatment representation, reproducible train/validation/test splits, tissue-aware WSI patch extraction, and patch-level feature generation.
* Extracted pathology embeddings using ResNet-18, Prov-GigaPath, UNI, and UNI2-h
* Implemented and evaluated multiple survival architectures, including mean-pooling MIL, attention MIL, gated attention MIL, clinical-guided MIL, clinical-only prediction, and a treatment-conditioned multimodal model.
* Designed a treatment-conditioned gating mechanism that incorporates chemotherapy, radiation, surgery, and pharmaceutical therapy information into the pathology-based survival model.
* Trained the models using Cox proportional hazards partial likelihood loss with Adam optimization and evaluated survival risk using the concordance index (C-index), Kaplan-Meier survival analysis, and log-rank testing.
* Achieved the strongest performance with the treatment-conditioned Prov-GigaPath model, reaching a **0.7385 C-index**, compared with **0.5231** for the GigaPath baseline.
* Results showed that incorporating treatment information consistently improved survival prediction for stronger pathology encoders, demonstrating the value of combining tumor morphology with therapeutic context.
* Identified limitations including the small 110-patient cohort, heuristic treatment timing, single-slide selection, limited patch sampling, and lack of external validation. All log-rank tests were non-significant (p > 0.05), highlighting the need for larger cohorts and independent validation.
