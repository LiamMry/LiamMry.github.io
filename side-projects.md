---
title: "Side Projects"
permalink: "/side-projects/"
layout: page
---

Outside of my academic research, I like applying generative modelling to other domains. Here are some of my side projects.

<div style="border:1px solid rgba(128,128,128,0.22);border-radius:8px;padding:1rem 1.2rem;margin:1.1em 0 1.5em">
  <div style="display:flex;align-items:baseline;gap:10px;flex-wrap:wrap;margin-bottom:0.4em">
    <span style="font-weight:600">Diffusion Models for Implied Volatility Surfaces</span>
    <a href="https://github.com/LiamMry/expert-octo-barnacle" target="_blank" style="font-size:0.85em">GitHub ↗</a>
  </div>
  <div style="font-size:0.9em;opacity:0.8;text-align:justify">
    A score-based diffusion model (VP-SDE, UNet with attention) trained on 3,500 daily SPY option surfaces (2010–2023) to generate realistic implied volatility surfaces from noise. Without any explicit no-arbitrage constraint, the generated surfaces learn financial consistency directly from data: calendar-spread and butterfly-spread violation rates approach the market baseline. Includes unconditional and conditional sampling pipelines and a comparison of ODE/SDE samplers (Euler, Heun, DPM-Solver) — the same sampler trade-offs explored in the demos on the home page, applied to quantitative finance.
  </div>
  <div style="margin-top:0.6em;display:flex;gap:6px;flex-wrap:wrap">
    <span style="font-size:0.75em;padding:2px 8px;border:1px solid rgba(128,128,128,0.3);border-radius:10px;opacity:0.7">Python</span>
    <span style="font-size:0.75em;padding:2px 8px;border:1px solid rgba(128,128,128,0.3);border-radius:10px;opacity:0.7">PyTorch</span>
    <span style="font-size:0.75em;padding:2px 8px;border:1px solid rgba(128,128,128,0.3);border-radius:10px;opacity:0.7">Score-based diffusion</span>
    <span style="font-size:0.75em;padding:2px 8px;border:1px solid rgba(128,128,128,0.3);border-radius:10px;opacity:0.7">Quantitative finance</span>
  </div>
</div>
