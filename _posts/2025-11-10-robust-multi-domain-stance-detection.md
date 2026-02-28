---
title: "Robust Multi-Domain Stance Detection Under Weak Supervision"
date: 2025-11-10
categories: [projects]
tags: [stance-detection, multi-task-learning, domain-generalization, weak-supervision, bert]
image:
  path: /assets/img/ark-stance/ark_arch.png
  alt: ARK-NLP Architecture
---

## Summary

I investigated how to build a stance detection system that generalizes across heterogeneous, weakly supervised datasets without harmonizing label spaces.  

By adapting cyclic multi-domain pretraining with an EMA teacher architecture, I improved AUC under severe class imbalance and identified sarcasm as a systematic annotation failure mode.  

This project demonstrates stability-aware training, cross-domain transfer, and data-centric error analysis for real-world NLP systems.

[📄 Download Full Paper (PDF)](/assets/papers/ark-nlp-stance-detection.pdf){: .btn .btn-primary }
---

# 1. Problem Setting

Stance detection aims to determine whether a text expresses **pro, anti, or neutral** stance toward a specific aspect.

Unlike standard sentiment analysis, stance is **aspect-conditional**.

Example:

> "The pizza was amazing but the service was terrible."

- Stance toward *pizza* → Pro  
- Stance toward *service* → Anti  

In real-world deployments, stance systems must generalize across:

- Political discourse  
- Social movements  
- Public health  
- Product reviews  

However, available datasets suffer from:

- Severe class imbalance  
- Weak supervision artifacts  
- Incompatible label taxonomies  
- Domain-specific bias  

The core research question:

> **How can we train a single model that generalizes across heterogeneous stance datasets without label harmonization?**

---

# 2. Datasets

I evaluated on Masked-ABSA stance datasets spanning U.S. politics and race relations.

## U.S. Race Relations Dataset

| Metric | Value |
|--------|--------|
| Samples | 87,359 |
| Aspect Terms | 239 |
| Pro Labels | 14,340 |
| Anti Labels | 73,019 |
| Label Space | Binary |

The dataset exhibits severe imbalance (~5:1 skew).  
However, the imbalance is not uniform across aspects — it is structurally polarized.

### 1. Global Polarization Structure

![Race Scatter](/assets/img/ark-stance/race_scatter.png)

Each point represents an aspect term.  
- X-axis: Pro label count  
- Y-axis: Anti label count  
- Marker size: total samples  

Most aspects cluster near the vertical axis, indicating strong dominance of the **Anti** class.  
Very few aspects lie near the diagonal (balanced distribution).

This geometric skew motivates using AUC instead of accuracy.

---

### 2. Most Polarized Aspects

![Race Polarized](/assets/img/ark-stance/race_polarized.png)

The top 25 most polarized aspects show extreme Pro–Anti disparities.  
Many aspects are nearly single-class dominated.

This confirms that naïve accuracy can be misleading.

---

### 3. High-Frequency Aspect Proportions

![Race Mixed](/assets/img/ark-stance/race_mixed.png)

Among the 25 aspects with the largest total counts:

- Only a minority exhibit meaningful class balance.
- Even high-frequency aspects remain heavily skewed.

This indicates that imbalance is structural rather than sample-size driven.

## U.S. Politics Dataset

| Metric | Value |
|--------|--------|
| Label Space | Pro / Neutral / Anti |
| Taxonomy | Ternary |
| Supervision | Weak (ideological camp mapping) |

This dataset introduces taxonomy heterogeneity relative to the binary race dataset.

---

# 3. Method: ARK–NLP

To address label heterogeneity and domain shift, I adapted the **Accrue & Reuse Knowledge (ARK)** framework from medical imaging to NLP.

## Architecture

![ARK Architecture](/assets/img/ark-stance/ark_arch.png)

The system consists of:

- Shared BERT encoder  
- Shared projection head  
- Dataset-specific classification heads  
- EMA-updated teacher model  

### Key Design Decisions

| Problem | Design Choice | Rationale |
|----------|--------------|------------|
| Heterogeneous labels | Dataset-specific heads | Avoid label harmonization |
| Label noise | EMA teacher | Stabilize training |
| Class imbalance | AUC metric | Threshold-independent evaluation |

---

## Cyclic Multi-Domain Training

Instead of merging datasets, training proceeds in cycles:

1. Train on Dataset A  
2. Update teacher via EMA  
3. Train on Dataset B  
4. Repeat  

This allows the teacher to accumulate transferable representations while preserving dataset semantics.

---

# 4. Evaluation Strategy

Given severe class imbalance, I used:

- **Primary Metric: AUC**
- Secondary Metric: Accuracy (for comparability)

Why AUC?

- Threshold-independent  
- Robust under skewed distributions  
- Measures ranking quality  

Accuracy alone is misleading in 5:1 skew settings.

---

# 5. Results

## Political Dataset

### Accuracy

![Political Accuracy](/assets/img/ark-stance/politic_accuracy.png)

### AUC

![Political AUC](/assets/img/ark-stance/politic_auc.png)

ARK:

- Converges faster  
- Shows smoother learning curves  
- Achieves higher AUC  

---

## Race Dataset

### Accuracy

![Race Accuracy](/assets/img/ark-stance/race_accuracy.png)

### AUC

![Race AUC](/assets/img/ark-stance/race_auc.png)

Despite extreme imbalance (73k vs 14k):

- ARK significantly improves AUC  
- EMA reduces early-epoch volatility  
- Training remains stable across cycles  

---

# 6. Experimental Findings

Through controlled comparison against independently fine-tuned BERT baselines, I found:

1. **Cyclic training improves cross-domain generalization.**
2. **EMA teacher reduces gradient instability under imbalance.**
3. **Dataset-specific heads preserve taxonomy semantics without alignment.**
4. **Naïve joint training leads to higher variance and weaker convergence.**

---

# 7. Error Analysis: Sarcasm as a Systematic Failure Mode

Many model “errors” were cases where:

- Weak supervision assigned labels based on ideological camp
- Text contained sarcasm that inverted literal stance

Example:

Aspect: Trump  
Weak Label: Anti  
Model Prediction: Pro  

Text:

> "Supreme irony is that [MASK] is very likely the least racist president we have ever had."

Semantically Pro, but labeled Anti due to user affiliation.

This suggests:

> A portion of errors reflect annotation artifacts rather than representation failure.

---

# 8. Implications for Applied ML Systems

This project highlights:

- The importance of stability-aware optimization  
- The danger of naïve dataset aggregation  
- The impact of label noise on evaluation  
- The need for auxiliary modeling (e.g., sarcasm detection)  

---

# 9. Future Directions

- Add sarcasm detection as auxiliary task  
- Conduct ablation on cyclic vs joint training  
- Quantify variance reduction across seeds  
- Extend to larger encoder backbones  

---

# 10. Code

Full implementation available at:

👉 https://github.com/dbasina/Ark_Stance_Detection