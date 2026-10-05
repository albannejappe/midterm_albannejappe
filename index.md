---
title: "Welcome to MRI Physics: Pulse Sequences & Contrast Mechanisms"
kernelspec:
  name: python3
  display_name: Python 3
---

# Magnetic Resonance Imaging: Fundamentals of Image Contrast

Welcome to this interactive **MyST Book** dedicated to understanding the core physical principles governing contrast generation in Magnetic Resonance Imaging (MRI).

This digital notebook combines theoretical physics, clinical application, audio signatures, and interactive Python simulations to provide a clear intuition of how tissue properties ($T_1$, $T_2$, $T_2^*$, and $PD$) and sequence timing ($TR$, $TE$, flip angle $\theta$) interact to create medical images.

---

## Learning Objectives

By exploring this book, you will learn to:

1. **Understand Physical Relaxation Mechanisms:** Distinguish between longitudinal recovery ($T_1$), true transverse decay ($T_2$), and effective transverse decay ($T_2^*$).
2. **Master Sequence Timing:** Predict how adjusting **Repetition Time ($TR$)** and **Echo Time ($TE$)** alters image weighting in Spin Echo (SE) and Gradient Echo (GRE) sequences.
3. **Recognize Tissue Signatures:** Visually identify brain and musculoskeletal tissue appearances across $T_1$-weighted, $T_2$-weighted, and Proton Density-weighted scans.
4. **Identify Acoustic & Susceptibility Effects:** Connect the physics of gradient switching to sequence noise and understand susceptibility artifacts in clinical MRI and fMRI.

---

## Book Structure & Table of Contents

::::{grid} 1 1 2 2
:gutter: 3

:::{grid-item-card} Chapter 1: Image Types & Weightings
:class-header: bg-light

Explore the fundamental definitions of $T_1$ and $T_2$ relaxation times across human tissues.

* **Definitions & Molecular Physics:** Why $T_1 > T_2$.
* **Sequence Parameters:** How $TR$ and $TE$ control weighting.
* **Visual Identification:** Brain and Knee MRI comparisons.
* **Acoustic Signatures:** Listen to the sound of SE acquisitions.
* **Interactive Inspector:** Explore tissue relaxation values in real time.
:::

:::{grid-item-card} Chapter 2: T2 and T2* Relaxation
:class-header: bg-light

Dive deep into spin dephasing, magnetic field inhomogeneities, and Gradient Echo sequences.

* **What is $T2*$:** Reversible vs. irreversible transverse decay.
* **Clinical Applications:** Susceptibility artifacts, microbleeds, and fMRI BOLD.
* **The $T2*$ Song:** Educational music video integration and physics breakdown.
* **Interactive Simulations:** Python modeling of spin dephasing.
:::
::::


:::{admonition} How to Navigate this Book

- Interactive Figures: You can hover over data points in Python plots to view detailed tissue values.

- Code Blocks: Click on "Show code" tags to inspect the Python scripts generating the mathematical curves.

:::
