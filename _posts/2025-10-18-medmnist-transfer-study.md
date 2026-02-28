---
title: "Transfer Learning vs From-Scratch Training in Medical Imaging (MedMNIST Study)"
date: 2025-10-18
categories: [projects]
tags: [computer-vision, medical-imaging, transfer-learning, transformers, ResNet, benchmarking]
math: false
image:
  path: /assets/img/medmnist-transfer/in1k_resnet_vs_swinb_12plots.png
  alt: Transfer-learning-improvements
---

## Overview

In medical imaging, datasets are often small and highly specialized.  
A central question emerges:

> Should we rely on pretrained transformers, or do convolutional networks remain competitive when trained from scratch?

To explore this, I conducted a systematic benchmarking study across **all 12 MedMNIST2D datasets**, evaluating how architecture, initialization, and optimizer interact under constrained-data regimes.

This project was completed in **October 2025** with a focus on understanding practical behavior rather than chasing leaderboard metrics.

---

## Experimental Design

Each experiment compared:

- **ResNet50 vs Swin-Base**
- **ImageNet-1k pretrained vs random initialization**
- **SGD vs AdamW optimizer**
- **5 random seeds per configuration**
- Evaluation metric: **AUC**

All comparisons include mean ± standard deviation and paired t-tests for statistical validation.

Training was performed on a single NVIDIA A100 GPU.

The goal was to analyze *patterns of behavior* across datasets — not isolated results.

---

# Results

## 1. Pretrained Models (ImageNet-1k)

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/medmnist-transfer/in1k_resnet_vs_swinb_12plots.png"
    style="width: 100%; height: auto; border-radius:10px;"
    alt="Pretrained ResNet50 vs Swin-B across 12 MedMNIST datasets"
  >
  <figcaption style="text-align:center; margin-top:0.5rem;">
    Pretrained comparison across all 12 MedMNIST datasets.
  </figcaption>
</figure>

When initialized with ImageNet weights, **Swin-B outperforms ResNet50 on the majority of datasets**, often with statistical significance.

This strongly suggests that transformer architectures benefit more from large-scale pretraining and are able to transfer global representations effectively to medical domains.

---

## 2. From-Scratch Training

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/medmnist-transfer/scratch_resnet_vs_swinb_12plots.png"
    style="width: 100%; height: auto; border-radius:10px;"
    alt="Scratch ResNet50 vs Swin-B across 12 MedMNIST datasets"
  >
  <figcaption style="text-align:center; margin-top:0.5rem;">
    From-scratch comparison across all 12 MedMNIST datasets.
  </figcaption>
</figure>

Without pretraining, the pattern changes.

**ResNet50 frequently matches or exceeds Swin-B performance**, particularly in lower-data regimes.

This indicates that convolutional inductive bias provides strong regularization when pretrained representations are unavailable.

---

## 3. Optimizer Sensitivity (SGD vs AdamW)

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/medmnist-transfer/resnet_swin_sgd_adam_chest_breast.png"
    style="width: 100%; height: auto; border-radius:10px;"
    alt="SGD vs Adam comparison"
  >
  <figcaption style="text-align:center; margin-top:0.5rem;">
    Optimizer comparison on datasets with strongest scratch instability.
  </figcaption>
</figure>

Further analysis shows that **transformers are more sensitive to optimizer choice** when trained from scratch.

- AdamW converges faster and often improves final AUC.
- SGD progresses more gradually and exhibits smoother training dynamics.
- The largest AdamW gains were observed on BreastMNIST and ChestMNIST.

This highlights that architecture and optimizer must be considered jointly.

---

# Interpretation

The comparison between pretrained and scratch regimes reveals a consistent structural pattern:

- **Transformers excel when pretrained.**
- **CNNs remain competitive when trained from scratch.**
- **Transformers are more optimizer-sensitive in low-data settings.**

These results suggest that inductive bias plays a stabilizing role when data is limited, while pretrained global attention mechanisms dominate when prior knowledge is available.

---

# Practical Engineering Implications

From an applied ML perspective:

### When pretrained checkpoints are available
Fine-tuning transformer models (e.g., Swin-B) is typically advantageous.  
Transfer learning significantly improves both convergence speed and final performance.

### When training from scratch
ResNet50 offers competitive and often more stable behavior in small datasets.

### Optimizer trade-offs
- **AdamW**: Faster convergence, better for rapid iteration and compute-constrained environments.
- **SGD**: Slower but steadier optimization trajectory, potentially better for controlled long-horizon experiments.

In production systems, these decisions affect:

- Training cost
- Iteration velocity
- Deployment latency
- Stability under data shifts

Architecture, initialization, and optimizer must be selected together — not independently.

---

# Statistical Rigor

- 5 seeds per configuration  
- Paired statistical testing  
- Error bars represent standard deviation  
- Consistent evaluation metric (AUC)  

This ensures observed differences are systematic rather than incidental.

---

# Key Takeaways

- Pretraining substantially benefits transformer architectures.
- CNN inductive bias provides robustness in low-data regimes.
- Optimizer selection meaningfully impacts transformer stability.
- Architectural decisions must consider data regime and compute constraints.

These findings are directly applicable to teams building real-world medical ML systems.

---

## Code

Repository:  
[MedMNIST Experiments on GitHub](https://github.com/dbasina/MedMNIST-experiments)