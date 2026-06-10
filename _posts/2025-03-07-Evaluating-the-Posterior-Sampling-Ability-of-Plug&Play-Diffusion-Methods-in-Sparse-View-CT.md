---
title:  "Evaluating Posterior Sampling of PnP Diffusion Models in Sparse-View CT - ICASSP 2025"
mathjax: true
layout: post
---

**Authors**: L. Moroy<sup>1,2</sup>, G. Bourmaud<sup>2</sup>, F. Champagnat<sup>1</sup>, J.-F. Giovannelli<sup>2</sup>  
**Affiliations**: <sup>1</sup>ONERA (Univ. Paris-Saclay), <sup>2</sup>IMS (Univ. Bordeaux – CNRS – BINP),   

<div style="border: 1px solid rgba(74,144,217,0.35); border-radius: 8px; padding: 1rem 1.2rem; margin: 1.2rem 0; background: rgba(74,144,217,0.05);">
  <div style="font-size:0.8rem; font-weight:600; text-transform:uppercase; letter-spacing:0.07em; color:#4a90d9; margin-bottom:0.5rem;">Key contribution</div>
  <p style="margin:0 0 0.5rem;"><strong>First systematic evaluation</strong> of whether PnP diffusion methods (DPS, ΠG, FPS) actually sample from the posterior in sparse-view CT — not just produce sharp images.</p>
  <p style="margin:0; font-size:0.88rem; opacity:0.75;">Venue: ICASSP 2025 · <a href="https://ieeexplore.ieee.org/document/10888207">Conference Paper</a></p>
</div>

### 🧠 Overview

This work investigates whether **Plug&Play (PnP) diffusion models**—a class of diffusion models agnostic to the considered observation model—can accurately sample from the **posterior distribution** in **Sparse-View CT (SVCT)** settings, particularly when the posterior is **non-peaked or multimodal**.


## ❗ Problematic

Diffusion models are typically evaluated using image-to-image metrics like PSNR or SSIM, which only assess point estimates and fail to reflect how well the posterior distribution is captured. Most SVCT applications assume highly informative sinograms, where the posterior is near unimodal. However, in real-world scenarios with limited projections, the posterior becomes broad or multimodal—yet current methods and evaluations overlook this regime.


---

## 📉 Problem Setup

The observation model is:

$$
\mathbf{y}_p = \mathbf{H}_p \mathbf{x} + \boldsymbol{\epsilon}_p, \quad \boldsymbol{\epsilon}_p \sim \mathcal{N}(0, \sigma_y^2 \mathbf{I})
$$

- \\(\mathbf{y}_p\\): sinogram with \\(p\\) projections  
- \\( \mathbf{H}_p \\): Radon transform  
- \\( \mathbf{x} \\): target image  
- Goal: approximate the posterior \\( p(\mathbf{x} \vert \mathbf{y}_p) \\)

---

## 🧪 Evaluation Metrics

Two criteria to assess the *approximate posterior* \\( \tilde{p}(\mathbf{x} \vert \mathbf{y}_p) \\):

1. **Posterior-Prior Similarity (PPS)**:  
   Ensures marginal of approximate posterior matches the prior:

   $$
   \mathbb{E}_{\mathbf{y}_p}[\tilde{p}(\mathbf{x}|\mathbf{y}_p)] \approx p(\mathbf{x})
   $$

   → Measured via FID, KID, CMMD

2. **Normalized Measurement Consistency (NMC)**:  
   Checks if posterior samples explain the observed measurements:

   $$
   \frac{1}{m_p \sigma_y^2} \mathbb{E}_{\mathbf{y}_p, \mathbf{x} \sim \tilde{p}(\cdot\vert\mathbf{y}_p)} \left[ \vert \mathbf{y}_p - \mathbf{H}_p \mathbf{x} \|_2^2 \right] \approx 1
   $$

---

## 🔍 Methods Compared

- **MCG** [Chung et al.]: manifold constraints  
- **DPS** [Chung et al.]: simplified posterior sampling  
- **ΠG** [Song et al.]: pseudoinverse-guided

---

## 📊 Key Findings

- As projections decrease (e.g., \\( p = 6 \\)), **PnP diffusion models fail to match the true posterior**, even with state-of-the-art methods.
- Surprisingly, **DPS** (the simplest method) yielded the best balance of posterior quality and consistency.
- Traditional metrics like PSNR/SSIM are inadequate in this regime.

---

## 🧩 Takeaway

> **PnP diffusion methods are not inherently reliable posterior samplers in highly underdetermined (sparse-view) regimes.**  
> Evaluating generative models in inverse problems must go beyond image fidelity metrics and consider probabilistic fidelity.

---

## 📘 Citation
```
@article{moroy2024posterior,
  title={Evaluating the Posterior Sampling Ability of Plug\&Play Diffusion Methods in Sparse-View CT},
  author={Moroy, Liam and Bourmaud, Guillaume and Champagnat, Fr{\'e}d{\'e}ric and Giovannelli, Jean-Fran{\c{c}}ois},
  journal={arXiv preprint arXiv:2410.21301},
  year={2024}
}
```
