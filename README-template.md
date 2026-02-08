# Near–Real-Time Conflict-Related Fire Detection Using Unsupervised Deep Learning
##### Based on the RaVAEn model: https://github.com/spaceml-org/RaVAEn

**Official implementation of the paper:**

> **Near–Real-Time Conflict-Related Fire Detection Using Unsupervised Deep Learning and Satellite Imagery**
> <!-- *Remote Sensing Applications: Society and Environment (RSASE), 2026 -->
> **Authors:** Kuldip Singh Atwal, Dieter Pfoser, Daniel Rothbart (George Mason University)

---

## Abstract

This repository contains the official implementation of the study **“Near–Real-Time Conflict-Related Fire Detection Using Unsupervised Deep Learning and Satellite Imagery.”** We adapt the **RaVAEn** framework to detect conflict-related fires and burn scars in Sudan using high-resolution satellite imagery with a **resolution-agnostic Variational Autoencoder (VAE)**.

Unlike the original RaVAEn model designed for coarse-resolution disaster monitoring on-board satellites, this version is optimized for **3 m resolution PlanetScope imagery**, enabling detection of small, fragmented fires in active war zones. The model employs a lightweight, unsupervised approach that learns generalized visual primitives from diverse land-cover data (WorldFloods) and identifies fire-affected areas as anomalies in the latent space. The system delivers actionable insights within **24–30 hours** of image acquisition.

---

## Key Features

- **Resolution-Adaptive Architecture:** Incorporates *Frequency Decomposition*, *Multi-scale Dilated Convolutions*, and *BlurPool (anti-aliased downsampling)* to handle domain shifts between training data (10 m Sentinel-2) and inference data (3 m PlanetScope).

- **Unsupervised Learning:** No labeled fire data required. The model learns nominal surface representations and flags deviations via latent-space cosine distance.

- **Lightweight & Fast:** Designed for operational near–real-time monitoring with end-to-end latency of approximately **24–30 hours**.

- **Data-Scarce Robustness:** Validated on verified conflict incidents in Sudan (e.g., El Fasher, Gandahar Market) where ground truth is inaccessible.

---

## Methodology

### 1. Preprocessing and Spectral Alignment

The pipeline processes **4-band PlanetScope imagery** (Red, Green, Blue, NIR).

- **Normalization:** Inputs are log-transformed to manage high dynamic range and scaled to the `[-1, 1]` interval using the 1st and 99th percentiles of the training data.

- **Tiling:** Imagery is partitioned into non-overlapping **32 × 32 pixel tiles**.

---

### 2. Model Architecture
The core model, defined in `deeper_vae.py`, is a modified Variational Autoencoder that:

1. **Decomposes** inputs into low-frequency structural components and high-frequency texture components.
2. **Encodes** features using multi-scale atrous (dilated) convolutions to capture context at varying resolutions.
3. **Projects** representations into a **128-dimensional latent space**.

This design enables robustness to sensor resolution changes and texture fragmentation common in conflict environments.

---

### 3. Anomaly Detection

Inference compares **temporally paired tiles** (Pre-incident vs. Post-incident).

- **Metric:** Cosine distance between latent vectors <b>z</b><sub>pre</sub> and <b>z</b><sub>post</sub>.

- **Baseline:** A pixel-wise cosine distance method is included as a scientific control.

---

## Dataset

### Training Data — WorldFloods

The model is pre-trained on the **WorldFloods** dataset derived from Sentinel-2 imagery to learn diverse spectral and textural patterns.

- **Source:** https://github.com/spaceml-org/ml4floods
- **Adaptation:** Stochastic scale augmentation is applied during training to simulate the resolution shift from Sentinel-2 (10 m) to PlanetScope (3 m).

---

### Inference Data — PlanetScope

Commercial **PlanetScope Level 3B Ortho Scene (Analytic, 4-band)** imagery is used for inference.

- **Access:** Due to licensing restrictions, PlanetScope imagery for Sudan cannot be shared publicly. Researchers must obtain licenses directly from Planet Labs.

- **Case Studies:** Metadata for the five Sudan conflict incidents (dates and AOIs) is provided in the manuscript.

---

## Citation

If you use this code, model architecture, or methodology in your research, please cite:

```bibtex
@article{atwal2026near,
  title   = {Near–Real-Time Conflict-Related Fire Detection Using Unsupervised Deep Learning and Satellite Imagery},
  author  = {Atwal, Kuldip Singh and Pfoser, Dieter and Rothbart, Daniel},
  journal = {arXiv preprint arXiv:2512.07925},
  year    = {2026}
}
```
---

## Acknowledgements

This work was conducted as part of the Sudan Conflict Observatory (SCO). The researchers involved in the SCO project include the authors of this article as well as the following analysts: Moneim Adam, Mathieu Bere, Hind Fadul, Anusha Srirenganathanmalarvizhi, Zifu Wang, David Wong and Chaowei Yang.

Computing resources were provided by the Office of Research Computing at George Mason University.
