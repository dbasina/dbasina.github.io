---
title: "Image Denoising with Kolmogorov-Arnold Networks"
date: 2025-04-11
categories: [projects]
tags: [computer-vision, denoising, KAN, UNet, attention]
---

## Executive Summary
Built a KAN-augmented UNet denoiser for high-resolution (2K+) images by combining skip connections, depthwise separable convolutions, tokenization + self-attention, and KAN-based token projection layers. The model was submitted to the **NTIRE CVPR 2025 Denoise50 Challenge**, reaching **28.91 PSNR** and **0.05s runtime per image** in the development phase (ranked **9th**).

## Architecture
<p align="center">
  <img src="/assets/img/kan-image-denoising/kunet_architecture.svg" width="95%" alt="KUnet + KAN architecture diagram">
</p>

<p align="center">
  <em>KUnet denoiser with depthwise convolutions, tokenized latent processing, self-attention, and KAN projection layers.</em>
</p>

## Key Ideas
- **UNet + skip connections** to preserve fine detail during reconstruction
- **Depthwise separable convolutions** to reduce compute overhead and make KAN layers feasible
- **Patch tokenization + self-attention** for stronger denoising on high-resolution images
- **KANLinear projections** for expressive token-space transformations

## Results
- **Validation PSNR (tokenization + KAN projections):** 28.91  
- **Runtime:** 0.05s per image (development phase report)  
- **Competition standing (development phase):** 9th  

## Links
- Code: https://github.com/dbasina/KUnet_Dn50_Competition
