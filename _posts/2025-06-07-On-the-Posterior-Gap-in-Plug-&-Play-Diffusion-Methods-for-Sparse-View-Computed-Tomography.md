---
title:  "On the Posterior Gap in Plug & Play Diffusion Methods for Sparse-View Computed Tomography"
mathjax: true
layout: post
---

**Authors**: L. Moroy<sup>1,2</sup>, G. Bourmaud<sup>2</sup>, F. Champagnat<sup>1</sup>, J.-F. Giovannelli<sup>2</sup>  
**Affiliations**: <sup>1</sup>ONERA (Univ. Paris-Saclay), <sup>2</sup>IMS (Univ. Bordeaux – CNRS – BINP),   

### 🧠 Overview

This work is an extended version of our [ICASSP 2025 paper](https://arxiv.org/abs/2410.21301). It formalizes the notion of **Posterior Gap (PG)**, the discrepancy between the true posterior distribution and the approximation induced by a **generative model**, and proposes a practical framework to evaluate it for **Sparse-View CT (SVCT)**. We also introduce an **NCE-based calibration strategy** for PnP methods and conduct a comprehensive benchmark of DPS, ΠG, and FPS across three datasets. -> <a href="https://doi.org/10.1109/JSTSP.2026.3659332">IEEE JSTSP</a>

---

## ❗ Problematic

In ultra-sparse regimes (e.g., \\( p \leq 6 \\) projections), the sinogram is weakly informative, making the posterior **broad or multimodal**. Classical evaluation metrics such as PSNR and SSIM only assess point estimates and are therefore inadequate: they fail to capture whether a method actually samples from the true posterior distribution. This limitation becomes particularly critical in dynamic phenomena imaging (e.g., exhaust gas from a nozzle), where only a handful of fixed sensors can be deployed simultaneously.

---

## 📉 Problem Setup

The observation model is:

$$
\mathbf{y}_p = \mathbf{H}_p \mathbf{x} + \sigma_y \mathbf{z}_p, \quad \mathbf{z}_p \sim \mathcal{N}(0, \mathbf{I})
$$

- \\(\mathbf{y}_p \in \mathbb{R}^{m_p}\\): sinogram with \\(p\\) projections  
- \\(\mathbf{H}_p \in \mathbb{R}^{m_p \times n}\\): discretized Radon transform  
- \\(\mathbf{x} \in \mathbb{R}^n\\): target image  
- Goal: sample from the posterior \\( p(\mathbf{x} \mid \mathbf{y}_p) \\) using a PnP diffusion model

---

## 🔬 Posterior Gap Formalism

Given a prior \\( p(\mathbf{x}) \\) and the observation model, the true joint distribution \\( p(\mathbf{x}, \mathbf{z}) = p(\mathbf{x})p(\mathbf{z}) \\) factorizes by construction. A PnP diffusion model induces an approximate posterior \\( \tilde{p}(\mathbf{x} \mid \mathbf{y}) \\), from which we define the approximate joint \\( \tilde{p}(\mathbf{x}, \mathbf{z}) \\). The **Posterior Gap vanishes** if and only if:

1. \\( \tilde{p}(\mathbf{x}) = p(\mathbf{x}) \\) — **Posterior-Prior Similarity (PPS)**
2. \\( \tilde{p}(\mathbf{z}) = \mathcal{N}(0, \mathbf{I}) \\) — **Posterior-Residual Similarity (PRS)**
3. \\( \tilde{p}(\mathbf{x}, \mathbf{z}) = \tilde{p}(\mathbf{x})\tilde{p}(\mathbf{z}) \\) — **Independence** between generated images and residuals

Samples from \\( \tilde{p}(\mathbf{x}, \mathbf{z}) \\) are obtained as follows: draw \\( \mathbf{x} \sim p(\mathbf{x}) \\), \\( \mathbf{z} \sim \mathcal{N}(0, \mathbf{I}) \\), compute \\( \mathbf{y} = \mathbf{H}\mathbf{x} + \sigma_y \mathbf{z} \\), draw \\( \tilde{\mathbf{x}} \sim \tilde{p}(\cdot \mid \mathbf{y}) \\), then set \\( \tilde{\mathbf{z}} = (\mathbf{y} - \mathbf{H}\tilde{\mathbf{x}}) / \sigma_y \\).

---

## 🧪 Evaluation Metrics

**PPS** (does the approximate posterior marginalize to the prior?):
- **MMD** — pixel-level distribution distance (Gaussian RBF kernel, \\( \sigma = 10 \\))
- **CMMD** — semantic similarity via CLIP features

**PRS** (do posterior samples explain the observations?):
- **NCE** (Normalized Cross-Entropy):

$$
\text{NCE}(\tilde{p}(\mathbf{z})) = \frac{1}{m_p} \mathbb{E}_{\tilde{p}(\mathbf{z})} \left[ \|\tilde{\mathbf{z}}\|_2^2 \right] \approx 1
$$

  An NCE \\( < 1 \\) indicates overfitting to measurements; NCE \\( > 1 \\) indicates underfitting.
- **MMD** — distribution distance from \\( \mathcal{N}(0, \mathbf{I}) \\)

**Independence**: Distance Correlation (DCORR) between \\( \{\tilde{\mathbf{x}}_i\} \\) and \\( \{\tilde{\mathbf{z}}_i\} \\). A value near zero indicates independence.

---

## 🔍 Methods Compared

- **DPS** [Chung et al., ICLR 2023]: approximates the likelihood score via Tweedie's estimator; includes a sensitive hyperparameter \\( \zeta \\)
- **ΠG** [Song et al., ICLR 2023]: pseudoinverse-guided approximation with a time-dependent scaling factor
- **FPS** [Dou & Song, ICLR 2024]: filtering-based approach that conditions on a progressively noised measurement \\( \mathbf{y}_t \\); no tunable hyperparameter

Calibrated counterparts (**wDPS**, **wΠG**, **wFPS**) are obtained by tuning a scalar parameter to enforce NCE \\( \approx 1 \\).

---

## ⚙️ NCE-Based Calibration

For DPS, the hyperparameter \\( \zeta \\) is replaced by a scalar \\( \varrho_\text{DPS} \\) and calibrated by solving:

$$
\varrho^\star_\text{DPS} = \arg\min_{\varrho_\text{DPS}} \left( \text{NCE}(\tilde{p}(\mathbf{z};\, \varrho_\text{DPS})) - 1 \right)^2
$$

This can be solved efficiently via golden-section search. The NCE calibration also yields a near-minimal PPS-MMD and DCORR, validating its theoretical soundness. The same scalar calibration principle is applied to ΠG and FPS for fair comparison, though it is not expected to benefit those methods.

---

## 📊 Key Findings

- **All uncalibrated methods exhibit a substantial Posterior Gap**, which grows as the number of projections decreases.
- **DPS** systematically overfits (NCE \\( \ll 1 \\)); **FPS** underfits (NCE \\( \gg 1 \\)); **ΠG** severely overfits across all \\( p \\).
- **wDPS** is the clear winner after calibration: it achieves near-ideal NCE values and the lowest (or second-lowest) CMMD across all three datasets and projection counts.
- **FPS** has inherent posterior robustness — calibration is unnecessary and actually harmful, disrupting its natural prior-likelihood balance.
- **wΠG** improves PRS but degrades PPS, due to the replacement of its time-dependent scaling with a fixed scalar.
- **PSNR curves are misleading**: DPS and ΠG show competitive PSNR despite exhibiting large Posterior Gaps, confirming that estimation error metrics do not capture posterior quality.

---

## 🗂️ Datasets

| Dataset | Images | Resolution | Description |
|---|---|---|---|
| **JET** | 5,896 | 260×260 | Compressible jet flow cross-sections (CFD) |
| **LDCT** | 5,936 | 512×512 | Low-dose CT, 10 patients, 1 mm slice |
| **LIDC** | 239,472 | 512×512 | Lung CT, 1018 patients, heterogeneous acquisition |

All images are resized to 128×128 and normalized to [0, 1]. Noise standard deviation is set to 1% of the dynamic range of 180-projection sinograms.

---

## 🧩 Takeaway

> **PnP diffusion models are not reliable posterior samplers in highly underdetermined regimes.**  
> PSNR/SSIM alone are insufficient: the Posterior Gap framework—decomposed into PPS, PRS, and independence—provides a more complete picture of reconstruction quality.  
> When carefully calibrated, **wDPS** outperforms FPS and ΠG, though it requires recalibration upon any change to the observation model. **FPS** offers calibration-free robustness, making it preferable for dynamic or frequently changing acquisition settings.

---

## 📘 Citation

```
@article{moroy2026posteriorgap,
  title={On the Posterior Gap in Plug \& Play Diffusion Methods for Sparse-View Computed Tomography},
  author={Moroy, Liam and Bourmaud, Guillaume and Champagnat, Fr{\'e}d{\'e}ric and Giovannelli, Jean-Fran{\c{c}}ois},
  journal={IEEE Journal of Selected Topics in Signal Processing},
  year={2026},
  doi={10.1109/JSTSP.2026.3659332}
}
```

## Currently under peer review
