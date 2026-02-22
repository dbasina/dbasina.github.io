---
title: "Image Denoising with Kolmogorov–Arnold Networks (KUnet)"
date: 2025-04-11
categories: [projects]
tags: [computer-vision, image-denoising, KAN, UNet, attention, CVPR]
math: true
---

## Summary
This project investigates whether **Kolmogorov–Arnold Networks (KANs)** can be engineered to work effectively in high-resolution computer vision.

While KANs are theoretically expressive and parameter-efficient alternatives to MLPs, they are typically computationally expensive in practice.  
To address this, I designed **KUnet**, a KAN-augmented UNet architecture optimized for 2K+ resolution image denoising.

The model was submitted to the **NTIRE CVPR 2025 Denoise50 Challenge**, achieving:

- **28.91 PSNR**
- **0.84 SSIM**
- **0.05s runtime per image**
- **9th place (development phase)**

Notably, this was the **second fastest runtime among the top 9 entries**, demonstrating that KAN-based architectures can be competitive in both quality and efficiency when carefully engineered.

## Qualitative Results (2K Preview)

<p style="margin-top:-0.5rem;">
  Below are 12 noisy/denoised pairs (σ = 50 Gaussian noise).  
  Left: noisy input · Right: denoised output (KUnet).  
  Click any image for full resolution.
</p>

<div style="display:grid; grid-template-columns: 1fr 1fr; gap: 18px; align-items:start; margin: 1.5rem 0;">
  <div style="text-align:center; font-weight:600;">Noisy</div>
  <div style="text-align:center; font-weight:600;">Denoised</div>

  <!-- 0000001 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000001.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000001.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000001">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000001.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000001.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000001">
  </a>

  <!-- 0000002 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000002.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000002.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000002">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000002.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000002.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000002">
  </a>

  <!-- 0000003 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000003.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000003.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000003">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000003.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000003.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000003">
  </a>

  <!-- 0000004 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000004.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000004.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000004">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000004.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000004.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000004">
  </a>

  <!-- 0000005 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000005.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000005.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000005">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000005.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000005.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000005">
  </a>

  <!-- 0000006 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000006.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000006.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000006">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000006.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000006.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000006">
  </a>

  <!-- 0000007 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000007.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000007.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000007">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000007.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000007.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000007">
  </a>

  <!-- 0000008 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000008.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000008.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000008">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000008.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000008.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000008">
  </a>

  <!-- 0000009 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000009.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000009.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000009">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000009.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000009.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000009">
  </a>

  <!-- 0000010 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000010.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000010.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000010">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000010.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000010.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000010">
  </a>

  <!-- 0000011 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000011.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000011.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000011">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000011.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000011.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000011">
  </a>

  <!-- 0000012 -->
  <a href="/assets/img/kan-image-denoising/examples/noisy/0000012.png">
    <img src="/assets/img/kan-image-denoising/examples/noisy/0000012.png" style="width:100%; height:auto;" loading="lazy" alt="Noisy 0000012">
  </a>
  <a href="/assets/img/kan-image-denoising/examples/denoised/0000012.png">
    <img src="/assets/img/kan-image-denoising/examples/denoised/0000012.png" style="width:100%; height:auto;" loading="lazy" alt="Denoised 0000012">
  </a>
</div>

Across diverse scenes, the model preserves fine textures and structural details while removing heavy Gaussian noise — even at 2K resolution. Examples shown are from the DIV2K and LSDIR competition test sets. 


## Architecture Overview

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/kan-image-denoising/kunet_architecture.svg"
    style="width: 100%; height: auto;"
    alt="KUnet architecture"
  >
</figure>

KUnet integrates:

- A **UNet backbone** for multi-scale feature extraction  
- **Depthwise separable convolutions** for computational efficiency  
- **Patch tokenization + self-attention** in the latent space  
- **KANLinear projection layers** for expressive token transformations  

The key challenge was making KANs computationally feasible at scale.

---

# Architectural & Engineering Insights

## 1. UNet + Skip Connections

UNet provides an encoder–decoder structure that captures global context while reconstructing spatial detail.

Skip connections are critical: without them, fine-grained details are lost during repeated downsampling.

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/kan-image-denoising/unet_skip_diagram.png"
    style="width: 90%; height: auto;"
    alt="UNet skip connections"
  >
  <figcaption>
    Skip connections pass high-resolution feature maps directly to the decoder.
  </figcaption>
</figure>

### Empirical Observation

- Without skip connections:  
  Convergence required ~20 epochs to reach PSNR -19.23.

- With skip connections:  
  Converged in **3 epochs** to PSNR -19.47.

This dramatically improved optimization stability and reconstruction quality.

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/kan-image-denoising/skip_vs_no_skip_artifacts.png"
    style="width: 100%; height: auto;"
    alt="Skip vs no skip comparison"
  >
  <figcaption>
    Without skip connections (a), artifacts and structural distortions appear.  
    With skip connections and depthwise convolutions (b), spatial features are preserved.
  </figcaption>
</figure>

---

## 2. Depthwise Separable Convolutions

KAN layers are computationally heavy.  
To offset this cost, I replaced standard convolutions with **depthwise separable convolutions**.

Impact:

- Training time per epoch (CIFAR baseline experiments):  
  **~3 hours → ~20 minutes**
- Maintained representational capacity
- Reduced compute overhead to make KAN layers viable

This tradeoff was essential to scaling beyond toy datasets.

---

## 3. Tokenization + Attention in Latent Space

Inspired by Vision Transformers, the latent representation is:

1. Patch-tokenized  
2. Processed via self-attention  
3. Projected using KANLinear layers  

For 2K-resolution images, this was crucial.

Without tokenization:
- Training plateaued at **23.24 PSNR** after 70 epochs.

With tokenization + KAN projections:
- Validation PSNR improved to **28.91**.

Processing structured patches instead of entire images allowed finer-grained denoising and better global context modeling.

---

## 4. Kolmogorov–Arnold Linear Layers

KANs differ from MLPs by placing learnable spline-based activations on edges instead of nodes.

The Kolmogorov–Arnold representation theorem:

$$
f(\mathbf{x}) = \sum_{q=1}^{2n+1} \Phi_q \left( \sum_{p=1}^{n} \phi_{q,p}(x_p) \right)
$$

KANLinear layers were used selectively in token projection space, balancing expressiveness and computational feasibility.

This hybridization allowed us to exploit KAN advantages without overwhelming runtime costs.

---

# Competition Results — NTIRE CVPR 2025

The model was evaluated on the DIV2K and LSDIR datasets under the NTIRE 2025 Denoise50 benchmark.

| Rank | User        | PSNR  | SSIM | Runtime (s) |
|------|------------|-------|------|-------------|
| 1    | deepak.tyagi | 29.79 | 0.86 | N/R |
| 8    | Dennis5251  | 29.12 | 0.85 | 10.43 |
| **9** | **dbasina (Ours)** | **28.91** | **0.84** | **0.05** |

Key observations:

- Within **0.88 PSNR** of top-performing models
- **Second fastest runtime** among top 9
- Competitive against established denoising architectures

This demonstrates that KANs can be competitive in high-resolution vision tasks when engineered carefully.

---

# Key Takeaways

- KANs are viable in computer vision — but require architectural restructuring.
- Efficiency techniques (depthwise conv + tokenization) are essential.
- Hybrid CNN–Transformer–KAN designs are promising for high-resolution tasks.
- Careful engineering can close the gap to SOTA while preserving runtime efficiency.

---

# Technical Stack & Infrastructure

**Core Technologies**
- Python
- PyTorch
- Weights & Biases (experiment tracking and logging)
- SLURM (cluster job scheduling)
- Python plotting libraries (Matplotlib, Seaborn)

**Training Infrastructure**
- 4 × NVIDIA A100 GPUs
- Distributed multi-GPU training via SLURM-managed research cluster

The model was trained on a SLURM-managed research cluster using 4 × NVIDIA A100 GPUs.  
Experiment tracking, hyperparameter sweeps, and metric logging were managed through Weights & Biases to enable reproducible large-scale experimentation.

# Future Directions

- More efficient KAN implementations
- Removing conventional activation layers in favor of spline-based dynamics
- Further optimization of token projection layers
- Extending to event-based vision and image deblurring

---

## Code

GitHub:  
https://github.com/dbasina/KUnet_Dn50_Competition