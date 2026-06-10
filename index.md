---
layout: page
title: "About me"
---

<div align="center">
  <img src="/pp2.jpg" width="200" style="border-radius: 10%; margin-bottom: 1rem;">
  <p style="font-size:1.15rem; font-weight:600; margin: 0 0 0.3rem;">Liam Moroy</p>
  <p style="font-size:0.92rem; color:#666; margin: 0 0 1rem;">Postdoc · Heriot-Watt University (BISC) · Edinburgh</p>
  <p style="font-size:0.97rem; font-style:italic; max-width:520px; margin: 0 auto 1.2rem;">
    I build theoretically-grounded generative models (diffusion, flow-matching, ...) for posterior sampling, with applications ranging from aerospace, nuclear and medical imaging, or any kind of signals really.
  </p>
  <!-- Social links -->
  <div style="display:flex; justify-content:center; gap:14px; flex-wrap:wrap; margin-bottom:1.5rem;">
    <a href="mailto:l.moroy@hw.ac.uk" style="text-decoration:none; font-size:0.88rem; padding:5px 12px; border:1px solid #ccc; border-radius:5px;">Email</a>
    <a href="https://scholar.google.com/citations?user=eO4bW58AAAAJ&hl=en&oi=ao" target="_blank" style="text-decoration:none; font-size:0.88rem; padding:5px 12px; border:1px solid #ccc; border-radius:5px;">Google Scholar</a>
    <a href="https://github.com/LiamMry" target="_blank" style="text-decoration:none; font-size:0.88rem; padding:5px 12px; border:1px solid #ccc; border-radius:5px;">GitHub</a>
    <a href="https://www.linkedin.com/in/liam-moroy/" target="_blank" style="text-decoration:none; font-size:0.88rem; padding:5px 12px; border:1px solid #ccc; border-radius:5px;">LinkedIn</a>
    <a href="/LIAM_MOROY_CV.pdf" download style="text-decoration:none; font-size:0.88rem; padding:5px 12px; border:1px solid #ccc; border-radius:5px;">↓ CV (PDF)</a>
  </div>
</div>

I am a Research Associate (Postdoc) at Heriot-Watt University, in the BISC group, led by Prof. Yoann Altmann. I completed my Ph.D. at Université Paris-Saclay, conducting my research at ONERA (The French Aerospace Lab) and IMS Laboratory. My work focuses on the study of generative models, particularly diffusion models, for solving inverse problems. I am interested in improving the ability of these models to recover high-fidelity images from incomplete or corrupted observations, with applications spanning scientific, nuclear and medical imaging.

## Research Interests

<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(190px,1fr));gap:0.9rem;margin:1.1em 0 1.5em">
  <div style="border:1px solid rgba(128,128,128,0.22);border-radius:8px;padding:0.9rem 1rem">
    <div style="font-weight:600;margin-bottom:0.3em">Inverse Problems</div>
    <div style="font-size:0.87em;opacity:0.72">Reconstruction from incomplete or noisy observations — CT, scientific, nuclear and medical imaging, or any kind of signal really.</div>
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

---

## Research in motion

My work sits at the intersection of two questions: *how do diffusion models represent uncertainty over images*, and *how can that representation be exploited when only partial measurements are available?* The two demos below are direct illustrations of these questions.

### Demo 1 — What diffusion models actually do

A diffusion model learns to reverse a noise process: starting from pure Gaussian noise, it iteratively denoises toward samples from the data distribution. The key ingredient is the **score function** ∇ log p<sub>σ</sub>(x), the gradient of the log-density at noise level σ. The visualiser below runs annealed Langevin dynamics on a 2D mixture of Gaussians, showing how particles follow the score field as σ decreases from large (blurry, uninformative) to small (sharp, concentrated on the modes).

This is the core mechanism I study: in practice, the distribution is over *images*, the modes are *plausible reconstructions*, and the score is approximated by a neural network.

{% include diffusion_posterior_sampler_2d.html %}

Toggle **Posterior sampling** to see what happens when a linear observation y = x<sub>1</sub> + noise is available. Particles now follow a *conditioned* score, this is the Diffusion Posterior Sampling (DPS) regime my publications analyse. The **posterior gap** I formalise in the JSTSP paper arises precisely when this conditioning is imprecise.

---

### Demo 2 — Not all samplers are equal

Given a trained score network, there are many ways to integrate the reverse SDE. The comparison below runs three algorithms on the same target distribution: DDPM (full stochastic SDE), DDIM (probability flow ODE), and OT-Flow Matching (straight interpolant paths). All three converge, but with very different trajectory geometries and step-count requirements.

Understanding these trade-offs matters for inverse problems: a sampler that needs 1000 steps for clean generation may be impractical inside an iterative reconstruction loop. Much of my current work is about closing this gap, achieving accurate posterior sampling in far fewer function evaluations.

{% include sampling_trajectory_comparison.html %}

<!-- ## Resume
<a href="/LIAM_MOROY_CV.pdf" download>Download CV (PDF)</a> -->
