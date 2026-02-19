---
layout: page
title: Latent ODE Autoencoder
description: Predicting the future progression of Diabetic Retinopathy through Neural ODEs in latent space.
img: assets/img/robotic-vision/app_frontface.png
importance: 2
category: work
related_publications: true
---

<div class="row justify-content-center mb-4">
  <div class="col-auto">
    <a href="https://github.com/OLIVIERKANAMUGIRE/DR-APP" target="_blank" class="btn btn-sm z-depth-1" style="background:#24292e;color:white;border:none;padding:7px 18px;border-radius:6px;font-size:13px;">
      <i class="fab fa-github me-1"></i> GitHub
    </a>
  </div>
</div>

---

## Overview

This project models the **continuous-time progression** of Diabetic Retinopathy (DR) from retinal fundus images by combining a deep convolutional autoencoder with a **Neural Ordinary Differential Equation (Neural ODE)** solver operating entirely in latent space.

Instead of predicting a discrete next state, the model learns a **smooth latent trajectory** — extrapolating how retinal pathology evolves over time by integrating a learned ODE in the compressed image representation.

---

## The Core Idea

Encoding an image $$x$$ into a latent state $$z_0$$, the model then solves:

$$\frac{dz}{dt} = f_\theta(z, t), \quad z(0) = z_0$$

where $$f_\theta$$ is a convolutional neural network learned from longitudinal patient data. The predicted future retinal state at time $$T$$ is then decoded from $$z(T)$$:

$$\hat{x}(T) = \text{Decoder}\bigl(z(T)\bigr)$$

This formulation allows the model to **query any point in time** continuously, not just fixed discrete steps — a key advantage over recurrent or frame-prediction approaches.

---

## Architecture

### Autoencoder

The encoder compresses a $$3 \times 512 \times 512$$ fundus image down to a spatial latent code $$z_0 \in \mathbb{R}^{16 \times 32 \times 32}$$ through four stages of convolution, batch normalisation, and residual connections, each followed by max-pooling:

$$x \;\xrightarrow{\text{Enc}}\; h \in \mathbb{R}^{512 \times 32 \times 32} \;\xrightarrow{1\times1\text{ conv}}\; z_0 \in \mathbb{R}^{16 \times 32 \times 32}$$

The decoder mirrors this path using transposed convolutions with residual blocks, followed by a sigmoid output activation.

### Neural ODE Function

The ODE dynamics $$f_\theta$$ is a lightweight convolutional network in latent space:

$$f_\theta(z) : \mathbb{R}^{C \times H \times W} \rightarrow \mathbb{R}^{C \times H \times W}$$

implemented as:

$$z \;\xrightarrow{\text{Conv}(C \to 128) + \tanh}\; \xrightarrow{\text{Conv}(128 \to 128) + \tanh}\; \xrightarrow{\text{Conv}(128 \to C)}\; \frac{dz}{dt}$$

Integration uses the **Dormand–Prince `dopri5`** adaptive Runge–Kutta solver via `torchdiffeq`, over $$t \in [0.0,\, 0.5]$$ sampled at 5 steps.

---

## Preprocessing

Each fundus image undergoes **CLAHE** (Contrast Limited Adaptive Histogram Equalization) applied independently per RGB channel before encoding, enhancing local contrast in pathological regions (microaneurysms, exudates, haemorrhages) that are critical for progression modelling:

$$I'_c = \text{CLAHE}(I_c), \quad c \in \{R, G, B\}, \quad \text{clipLimit}=2.0,\; \text{tileGrid}=8\times8$$

---

## Web Application

A Flask app wraps the full inference pipeline with a **3-tab analysis dashboard**:

<div class="row mt-3">
  <div class="col-sm-4">
    <div class="card h-100" style="border:1px solid #e0e0f0;border-radius:10px;padding:16px;">
      <h5 style="color:#7c6af7;">① Reconstruction</h5>
      <p style="font-size:13px;color:#555;">Processed input vs. the ODE-predicted future retinal state side by side.</p>
    </div>
  </div>
  <div class="col-sm-4">
    <div class="card h-100" style="border:1px solid #e0e0f0;border-radius:10px;padding:16px;">
      <h5 style="color:#7c6af7;">② ODE Progression</h5>
      <p style="font-size:13px;color:#555;">Decoded retinal state at every integration step — a visual trajectory through latent space.</p>
    </div>
  </div>
  <div class="col-sm-4">
    <div class="card h-100" style="border:1px solid #e0e0f0;border-radius:10px;padding:16px;">
      <h5 style="color:#7c6af7;">③ Latent Analysis</h5>
      <p style="font-size:13px;color:#555;">PCA scatter of spatial activations in z₀ and per-channel latent heatmaps.</p>
    </div>
  </div>
</div>

---

## Tech Stack

<div class="row mt-3 mb-2">
  <div class="col-12">
    <img src="https://img.shields.io/badge/Python-3.9+-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python"/>
    &nbsp;
    <img src="https://img.shields.io/badge/PyTorch-2.0+-EE4C2C?style=flat-square&logo=pytorch&logoColor=white" alt="PyTorch"/>
    &nbsp;
    <img src="https://img.shields.io/badge/torchdiffeq-dopri5-orange?style=flat-square" alt="torchdiffeq"/>
    &nbsp;
    <img src="https://img.shields.io/badge/Flask-3.0+-000000?style=flat-square&logo=flask&logoColor=white" alt="Flask"/>
    &nbsp;
    <img src="https://img.shields.io/badge/OpenCV-CLAHE-5C3EE8?style=flat-square&logo=opencv&logoColor=white" alt="OpenCV"/>
    &nbsp;
    <img src="https://img.shields.io/badge/scikit--learn-PCA-F7931E?style=flat-square&logo=scikit-learn&logoColor=white" alt="sklearn"/>
  </div>
</div>

---

## Dataset

Training uses **longitudinal fundus image pairs** — two visits per patient separated by a clinical interval. The `LongitudinalFundusDataset` loads `visit_1.png` and `visit_2.png` per patient folder, applying CLAHE preprocessing before returning paired tensors for the autoencoder and ODE training objective.

```
data/
├── patient_001/
│   ├── visit_1.png
│   └── visit_2.png
└── patient_002/
    ├── visit_1.png
    └── visit_2.png
```

---

## Latent Space Analysis

After encoding to $$z_0 \in \mathbb{R}^{16 \times 32 \times 32}$$, each of the $$32 \times 32 = 1024$$ spatial locations is treated as a point in $$\mathbb{R}^{16}$$. PCA projects this cloud to 2D, colored by activation magnitude — revealing how the model distributes spatial retinal information across the learned latent manifold.
