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


## Qualitative Results (2K Interactive Preview)

<p style="margin-top:-0.5rem;">
  Toggle between noisy input (σ = 50 Gaussian noise) and denoised output (KUnet).
</p>

<style>
  .pair-grid{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:16px;
    margin:1.5rem 0;
  }
  @media(max-width:900px){
    .pair-grid{grid-template-columns:repeat(2,1fr);}
  }
  @media(max-width:600px){
    .pair-grid{grid-template-columns:1fr;}
  }
  .pair-card{
    border:1px solid rgba(255,255,255,0.08);
    border-radius:12px;
    padding:10px;
  }
  .pair-img{
    width:100%;
    height:auto;
    display:block;
    border-radius:10px;
  }
  .pair-tabs{
    display:flex;
    gap:8px;
    justify-content:center;
    margin-top:8px;
  }
  .pair-tabs button{
    padding:6px 10px;
    border-radius:999px;
    border:1px solid rgba(255,255,255,0.15);
    background:transparent;
    color:inherit;
    cursor:pointer;
    font-size:0.85rem;
  }
  .pair-tabs button.active{
    border-color:rgba(255,255,255,0.5);
  }
</style>

<div class="pair-grid">

  <!-- Loop: 0000001 → 0000012 -->

  <!-- 0000001 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000001.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000001.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000001.png"
         alt="Example 0000001">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000002 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000002.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000002.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000002.png"
         alt="Example 0000002">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000003 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000003.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000003.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000003.png"
         alt="Example 0000003">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000004 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000004.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000004.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000004.png"
         alt="Example 0000004">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000005 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000005.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000005.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000005.png"
         alt="Example 0000005">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000006 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000006.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000006.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000006.png"
         alt="Example 0000006">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000007 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000007.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000007.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000007.png"
         alt="Example 0000007">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000008 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000008.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000008.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000008.png"
         alt="Example 0000008">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000009 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000009.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000009.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000009.png"
         alt="Example 0000009">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000010 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000010.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000010.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000010.png"
         alt="Example 0000010">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000011 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000011.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000011.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000011.png"
         alt="Example 0000011">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

  <!-- 0000012 -->
  <div class="pair-card">
    <img class="pair-img" loading="lazy"
         src="/assets/img/kan-image-denoising/examples/denoised/0000012.png"
         data-noisy="/assets/img/kan-image-denoising/examples/noisy/0000012.png"
         data-denoised="/assets/img/kan-image-denoising/examples/denoised/0000012.png"
         alt="Example 0000012">
    <div class="pair-tabs">
      <button type="button" class="active" onclick="swapPair(this,'denoised')">Denoised</button>
      <button type="button" onclick="swapPair(this,'noisy')">Noisy</button>
    </div>
  </div>

</div>

<script>
  function swapPair(btn, which){
    const card = btn.closest('.pair-card');
    const img = card.querySelector('img');
    img.src = (which === 'noisy') ? img.dataset.noisy : img.dataset.denoised;

    const buttons = card.querySelectorAll('button');
    buttons.forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
  }
</script>


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
[KUnet on GitHub](https://github.com/dbasina/KUnet_Dn50_Competition)