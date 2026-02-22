---
title: "Image Denoising with Kolmogorov–Arnold Networks (KUnet)"
date: 2025-04-11
categories: [projects]
tags: [computer-vision, image-denoising, KAN, UNet, attention, CVPR]
math: true
---

## Summary

This project explores whether **Kolmogorov–Arnold Networks (KANs)** can be made practical for high-resolution computer vision tasks.

KANs are theoretically powerful and parameter-efficient alternatives to MLPs, but existing implementations are computationally expensive — especially for large image models.  
To address this, I designed **KUnet**, a KAN-augmented UNet architecture engineered for 2K+ resolution image denoising.

The model was submitted to the **NTIRE CVPR 2025 Denoise50 Challenge**, achieving:

- **28.91 PSNR**
- **0.84 SSIM**
- **0.05s runtime per image**
- **9th place (development phase)**

Notably, this was the **second fastest runtime** among the top 9 entries.

---

## Qualitative Results

<figure style="margin: 2rem 0;">
  <img 
    src="/assets/img/kan-image-denoising/ntire_2k_results.png"
    style="width: 100%; height: auto;"
    alt="2K denoising results"
  >
  <figcaption>
    Example from the NTIRE Denoise50 dataset (σ = 50 Gaussian noise).  
    Left: noisy 2K image. Right: KUnet output.
  </figcaption>
</figure>

The model preserves fine textures and color structure while removing heavy Gaussian noise on high-resolution images.

---

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

# Design Decisions & Engineering Insights

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

KANLinear layers were used selectively in token projection space —  
balancing expressiveness and computational feasibility.

This hybridization allowed us to exploit KAN advantages without overwhelming runtime costs.

---

# Competition Results — NTIRE CVPR 2025

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

# Future Directions

- More efficient KAN implementations
- Removing conventional activation layers in favor of spline-based dynamics
- Further optimization of token projection layers
- Extending to event-based vision and image deblurring

---

## Code

GitHub:  
https://github.com/dbasina/KUnet_Dn50_Competition