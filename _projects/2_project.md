---
layout: page
title: On predicting the progression of diabetic retinopathy
description: Deep learning and longitudinal modelling for diabetic retinopathy progression prediction.
img: /assets/img/robotic-vision/fundus.jpg
importance: 2
category: work
related_publications: true
github: https://github.com/OLIVIERKANAMUGIRE/Diabetic-Retinopathy-and-Predicting-its-Progression
---

## Predicting Progression of Diabetic Retinopathy

**Longitudinal AI for Disease Progression**

Check it out on GitHub:  
👉 [Diabetic Retinopathy and Predicting its Progression]({{ page.github }})

This project focuses on modelling the **progression of diabetic retinopathy (DR)** over time using retinal fundus photographs and deep learning techniques. Unlike traditional work that focuses exclusively on classification or lesion segmentation, this research explores how the disease evolves longitudinally. :contentReference[oaicite:0]{index=0}

---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robotic-vision/diagram.png" title="Model Pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/d8.png" title="Retinal Image Samples" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/node.png" title="Progression Trends" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Left: proposed model architecture for progression prediction. Center: example fundus images. Right: demonstration of latent evolution over time.
</div>

---

### Motivation

Diabetic retinopathy is a leading cause of vision loss worldwide. While existing work often focuses on detecting or grading severity from a **single timepoint**, real-world clinical needs require tools that can **predict future disease progression** to inform treatment planning and early intervention. :contentReference[oaicite:1]{index=1}

---

### Approach Overview

This project proposes a framework that consists of two main components:

1. **Autoencoders for Representation Learning**
   - Build a compact latent representation of high-resolution fundus images.
   - Train convolutional autoencoders to reconstruct retinal images while encoding clinically meaningful features. :contentReference[oaicite:2]{index=2}

2. **Neural Ordinary Differential Equations (NODEs) for Temporal Prediction**
   - Model the evolution of the disease in latent space over time using NODEs.
   - Multiple ODE solvers were evaluated; the Dormand-Prince5 solver achieved the best predictive quality based on structural similarity metrics. :contentReference[oaicite:3]{index=3}

---

### What Was Achieved

- Learned a **latent space** that captures salient image features relevant for progression.
- Predicted future DR states from historical images using a continuous time model.
- Demonstrated potential for forecasting future retinal appearance and DR grade changes. :contentReference[oaicite:4]{index=4}

---

### Code Highlights

The repository contains:

- `main_autoencoder.py` — trains the autoencoder model.
- `main_ode_model.py` — trains the NODE-based progression predictor.
- `main_cv.py` — additional computer-vision utilities.
- `README.md` — project documentation and setup instructions.
- Diagrams and supporting figures illustrating model architecture and training pipeline. :contentReference[oaicite:5]{index=5}

---

### Why This Matters

Prediction of disease progression — not just classification — moves AI tools closer to **clinically actionable insights**. This can help clinicians assess risk, plan treatments earlier, and monitor disease evolution in patients with diabetes.

---
