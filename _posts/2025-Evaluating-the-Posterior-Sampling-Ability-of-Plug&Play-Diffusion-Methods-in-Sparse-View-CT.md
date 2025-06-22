---
title:  "Evaluating Posterior Sampling of PnP Diffusion Models in Sparse-View CT"
mathjax: true
layout: post
categories: media
---

# 📄 Evaluating Posterior Sampling of PnP Diffusion Models in Sparse-View CT - ICASSP 2025

**Authors**: Liam Moroy, Guillaume Bourmaud, Frédéric Champagnat, Jean-François Giovannelli  
**Affiliations**: ONERA / Université Paris-Saclay, IMS / Université de Bordeaux  

---

## 🧠 Overview

This work investigates whether **Plug&Play (PnP) diffusion models**—a class of powerful generative approaches—can accurately sample from the **posterior distribution** in **Sparse-View CT (SVCT)** settings, particularly when the posterior is **non-peaked or multimodal**.

---

## 📉 Problem Setup

The forward model in SVCT is:

$$
\mathbf{y}_p = \mathbf{H}_p \mathbf{x} + \boldsymbol{\epsilon}_p, \quad \boldsymbol{\epsilon}_p \sim \mathcal{N}(0, \sigma_y^2 \mathbf{I})
$$

- $ \mathbf{y}_p $: sinogram with $ p $ projections  
- \( \mathbf{H}_p \): Radon transform  
- \( \mathbf{x} \): target image  
- Goal: approximate the posterior \( p(\mathbf{x} | \mathbf{y}_p) \)

---

## 🧪 Evaluation Metrics

Two criteria to assess the *approximate posterior* \( \tilde{p}(\mathbf{x} | \mathbf{y}_p) \):

1. **Posterior-Prior Similarity (PPS)**:  
   Ensures marginal of approximate posterior matches the prior:

   $$
   \mathbb{E}_{\mathbf{y}_p}[\tilde{p}(\mathbf{x}|\mathbf{y}_p)] \approx p(\mathbf{x})
   $$

   → Measured via FID, KID, CMMD

2. **Normalized Measurement Consistency (NMC)**:  
   Checks if posterior samples explain the observed measurements:

   $$
   \frac{1}{m_p \sigma_y^2} \mathbb{E}_{\mathbf{y}_p, \mathbf{x} \sim \tilde{p}(\cdot|\mathbf{y}_p)} \left[ \| \mathbf{y}_p - \mathbf{H}_p \mathbf{x} \|_2^2 \right] \approx 1
   $$

---

## 🔍 Methods Compared

- **MCG** [Chung et al.]: manifold constraints  
- **DPS** [Chung et al.]: simplified posterior sampling  
- **ΠG** [Song et al.]: pseudoinverse-guided

---

## 📊 Key Findings

- As projections decrease (e.g., \( p = 6 \)), **PnP diffusion models fail to match the true posterior**, even with state-of-the-art methods.
- Surprisingly, **DPS** (the simplest method) yielded the best balance of posterior quality and consistency.
- Traditional metrics like PSNR/SSIM are inadequate in this regime.

---

## 🧩 Takeaway

> **PnP diffusion methods are not inherently reliable posterior samplers in highly underdetermined (sparse-view) regimes.**  
> Evaluating generative models in inverse problems must go beyond image fidelity metrics and consider probabilistic fidelity.

---

## 📘 Citation

