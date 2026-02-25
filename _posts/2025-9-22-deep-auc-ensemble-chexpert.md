---
title: "Improving Medical Image Classification with Deep AUC Maximization Ensembles"
date: 2026-02-24 10:00:00 +0000
categories: [projects]
tags: [deep learning, computer vision, medical imaging, ensemble learning, auc, chexpert]
image: /assets/img/deepauc/showcase_grid.png
---

## Overview

Most medical imaging systems are **evaluated using ROC-AUC**, yet almost all are trained with **cross-entropy loss**.

This project investigates whether **directly optimizing AUC using Deep AUC Maximization (DAM)** leads to consistent performance improvements in medical imaging.

Using the CheXpert chest X-ray dataset, I implemented a two-stage production-oriented training pipeline combining:

- Cross-entropy warm start  
- Disease-specific Deep AUC optimization  
- Per-disease binary specialization  
- Ensemble inference  

Experimental results show that Deep AUC Maximization consistently improved mean ROC-AUC by **2–3% across modern CNN and transformer architectures**.

| Architecture | Baseline (CE) | Deep AUC | Absolute Δ | % Improvement |
|-------------|--------------|----------|------------|---------------|
| ConvNeXt    | 0.877        | 0.8943   | +0.0173    | **+1.97%**    |
| DenseNet169 | 0.876        | 0.9035   | +0.0275    | **+3.14%**    |
| Swin        | 0.878        | 0.9003   | +0.0223    | **+2.54%**    |
| DaViT       | —            | 0.8995   | —          | —             |

The leads to the central question:

> Should AUC maximization be used as the norm going forward for classification tasks to improve performance?

---

# The Pipeline
<figure style="margin: 2rem 0;">
  <img
    src="/assets/img/deepauc/deepauc_pipeline.svg"
    style="width: 100%; height: auto; border-radius:12px;"
    alt="Two-stage Deep AUC Maximization pipeline"
  />
  <figcaption style="text-align:center; margin-top:0.5rem;">
    Two-stage training pipeline: Cross-Entropy warm start → Disease-specific Deep AUC optimization → Ensemble inference.
  </figcaption>
</figure>

The pipeline was designed to enable users to quicky test DeepAUC maximization with different image models available from timm and Transformer Libraries.

## Stage 1 — Shared Representation Learning (Cross-Entropy)

- Multi-label training using **binary cross-entropy**
- Single backbone predicting all pathologies jointly
- 2 training epochs
- Goal: learn stable radiographic feature embeddings

This stage prioritizes:
- Stable gradients
- Calibrated probability outputs
- Efficient shared feature learning

---

## Stage 2 — Disease-Specific Deep AUC Optimization

After Stage 1:

1. Load pretrained backbone weights  
2. Replace the final classification layer  
3. Train **independent binary classifiers**, one per disease  
4. Optimize each using **Deep AUC Maximization loss**  
5. Train for 2 additional epochs  

This results in **five independent pathology-specific models**, each directly optimizing ROC-AUC.

### Why This Matters

- AUC is fundamentally a ranking metric.
- Multi-label CE couples gradients across diseases.
- Independent binary optimization reduces cross-label interference.
- Each model specializes in its pathology distribution.

---

## Final Step — Ensembling

At inference time, the five disease-specific models are ensembled.

This provides:

- Reduced variance  
- Improved robustness  
- More stable ROC curves across thresholds  
- Better low-FPR performance (clinically critical region)

### Pipeline Summary

Shared Backbone (CE)  
→ Replace Final Layer  
→ Binary Disease-Specific DAM Training  
→ Ensemble Predictions  

This design balances stability, specialization, and robustness.

---

# Industry-Relevance

This pipeline was built with time and computation resource constraints in mind. Some of the highlights are:

- No increase in backbone size
- Minimal additional training time (only +2 epochs)
- Modular disease-specific heads
- Compatible with existing CE pipelines
- Easy to ablate or deploy incrementally

Rather than retraining entire architectures for each disease, we simply replace the final layer and finetune. This is also the recomended way of implementing Deep AUC Maximization. 

---

# Experimental Setup

- **Dataset:** CheXpert (multi-label chest X-ray classification)
- **Architectures:**
  - ConvNeXt
  - DenseNet169
  - Swin Transformer
  - DaViT
- **Evaluation:**
  - ROC-AUC per pathology
  - Mean ROC-AUC (primary metric)

All models used identical data splits and augmentation strategies.

---

# Results

## Mean Test AUC Improvement

| Architecture | Baseline (CE) | Deep AUC | Absolute Δ | % Improvement |
|-------------|--------------|----------|------------|---------------|
| ConvNeXt    | 0.877        | 0.8943   | +0.0173    | **+1.97%**    |
| DenseNet169 | 0.876        | 0.9035   | +0.0275    | **+3.14%**    |
| Swin        | 0.878        | 0.9003   | +0.0223    | **+2.54%**    |
| DaViT       | —            | 0.8995   | —          | —             |

Deep AUC Maximization improved mean ROC-AUC by ~2–3% across architectures. In our experiments we found that AUC maximization is sensitive to class imbalance due to the inherent nature of AUC being dependant on how many postivie and negative samples are present. Therefore, during training, batches need to sampled appropriately for heavily imbalanced classes based on the class imbalance ratios. The authors provide a clean implementation of the samplers that can be tuned to work well with other datasets. 

Notably:

- Gains were consistent across CNNs and transformers.
- Improvements required **no architectural changes**.
- Strongest gains appeared in imbalanced pathologies.

---

## Per-Disease AUC by Architecture

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/deepauc/auc_by_architecture.png"
    style="width: 100%; height: auto; border-radius:10px;"
    alt="Per-disease AUC by architecture"
  >
  <figcaption style="text-align:center; margin-top:0.5rem;">
    ROC-AUC across five thoracic pathologies.
  </figcaption>
</figure>

Observations:

- Pleural Effusion consistently achieves the highest AUC.
- Consolidation remains comparatively harder.
- DenseNet169 performs strongest overall.
- Improvements are pathology-dependent, not uniform.

---

# Qualitative Inference Examples

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/deepauc/showcase_grid.png"
    style="width: 100%; height: auto; border-radius:10px;"
    alt="Chest X-ray inference examples"
  >
  <figcaption style="text-align:center; margin-top:0.5rem;">
    Example predictions. “Top” indicates highest predicted probabilities; “GT” denotes ground truth labels.
  </figcaption>
</figure>

### Observations

- Correct labels frequently appear among top predictions with moderate confidence.
- False positives reflect overlapping radiographic features.
- “GT: none” cases show diffuse low probabilities, indicating reasonable calibration.
- Lateral views and device artifacts introduce higher uncertainty — highlighting real-world robustness challenges.

---

# Key Takeaways

- Deep AUC Maximization consistently boosts ROC-AUC by ~2–3% on CheXpert across CNNs and transformers.
- Direct AUC optimization performs well under class imbalance provided that sampling is done appropriately.
- Ensembling disease-specific models further improves robustness although slighly expensive. 
- Combining multiple architectures (e.g., ConvNeXt + Swin) outperforms the same models trained with cross-entropy alone.

---

## Repository

🔗 https://github.com/dbasina/DeepAUCMaximization