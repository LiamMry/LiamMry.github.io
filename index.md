---
layout: page
title: "About me"
---

<div align="center">
    <img src="/pp2.jpg" width="250" style="border-radius: 20px;">
</div>

I am a Research Associate (Postdoc) at Heriot-Watt University, in the BISC group, led by Prof. Yoann Altmann. I completed my Ph.D. at Université Paris-Saclay, conducting my research at ONERA (The French Aerospace Lab) and IMS Laboratory. My work focuses on the study of generative models, particularly diffusion models, for solving inverse problems. I am interested in improving the ability of these models to recover high-fidelity images from incomplete or corrupted observations, with applications spanning scientific and medical imaging.

## Research Interests

<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(190px,1fr));gap:0.9rem;margin:1.1em 0 1.5em">

  <div style="border:1px solid rgba(128,128,128,0.22);border-radius:8px;padding:0.9rem 1rem">
    <div style="font-weight:600;margin-bottom:0.3em">Inverse Problems</div>
    <div style="font-size:0.87em;opacity:0.72">Reconstruction from incomplete or noisy observations — CT, scientific and medical imaging.</div>
  </div>

  <div style="border:1px solid rgba(128,128,128,0.22);border-radius:8px;padding:0.9rem 1rem">
    <div style="font-weight:600;margin-bottom:0.3em">Generative Modelling</div>
    <div style="font-size:0.87em;opacity:0.72">Diffusion models as learned priors for Bayesian inference over images.</div>
  </div>

  <div style="border:1px solid rgba(128,128,128,0.22);border-radius:8px;padding:0.9rem 1rem">
    <div style="font-weight:600;margin-bottom:0.3em">Posterior Sampling</div>
    <div style="font-size:0.87em;opacity:0.72">Evaluating and improving approximate samplers in highly underdetermined regimes.</div>
  </div>

</div>

#### Diffusion model — interactive demo - 2D diffusion prior and posterior sampling visualiser —

Annealed Langevin dynamics on a 2D mixture of Gaussians with DPS guidance. Toggle **Posterior sampling** to condition on a linear observation.

{% include diffusion_posterior_sampler_2d.html %}

#### Sampling algorithms — trajectory comparison

Side-by-side comparison of DDPM (stochastic SDE), DDIM (probability flow ODE), and OT-Flow Matching on the same 2D mixture of Gaussians.

{% include sampling_trajectory_comparison.html %}

## Resume
<a href="/LIAM_MOROY_CV.pdf" download>Download CV (PDF)</a>
