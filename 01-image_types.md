---
title: 'Image types: T1, T2, T2*, PD'
kernelspec:
  name: base
  display_name: Python 3
---

## Which parameters influence the image type (weight) ?
explication des différentes séquences (spin echo, gradient echo, theta...) qui donnent chaque type d'image ajout des schéma de la séquence
code?

## Some examples
exemples (plusieurs! de différentes parties du corps) de chaque type d'image en expliquant pourquoi telle image est T1-weighted...
(pas seulement en se basant sur TE et TR, mais plus avec l'aspect des différents tissus)
figure interactive: en passant la souris sur une structure ça nous dit ce que c'est, son T1 et T2 (jsp si c'est possible de faire ça)
ajout des images


## Image Types & Weightings in MRI

In Magnetic Resonance Imaging (MRI), contrast between tissues is primarily determined by intrinsic magnetic relaxation properties (**T1**, **T2**, and **T2*** times) and proton density (**PD**). By adjusting scanner parameters such as Echo Time (TE), Repetition Time (TR), and flip angle ($\theta$), we can "weight" the image toward a specific physical property.

---

# What are T1 and T2?

### Fundamental Definitions

T1 and T2 are intrinsic properties of materials and tissues. They are times expressed in milliseconds and are based on relaxation times following a B1 excitation.

* **T1 Relaxation (Spin-Lattice / Longitudinal Relaxation):** The process by which the longitudinal magnetization ($M_z$) recovers back to its thermal equilibrium value ($M_0$) along the main magnetic field ($B_0$). $T_1$ is defined as the time required for $M_z$ to recover to approximately **63%** of $M_0$.

$$M_z(t) = M_0 \left(1 - e^{-t / T_1}\right)$$

* **T2 Relaxation (Spin-Spin / Transverse Relaxation):** The process by which transverse magnetization ($M_{xy}$) decays due to spin phase coherence loss caused by microscopic magnetic interactions between neighboring nuclei. $T_2$ is defined as the time required for $M_{xy}$ to drop to **37%** of its initial magnitude.

$$M_{xy}(t) = M_0 e^{-t / T_2}$$

### Relaxation Times Across Human Tissues ($B_0 = 1.5\text{ T}$)

| Tissue Type | $T_1$ Relaxation | $T_2$ Relaxation | Typical $T_1$ Value (ms) | Typical $T_2$ Value (ms) |
| :--- | :--- | :--- | :--- | :--- |
| **Fat** | Very Short | Short / Medium | ~250 – 300 | ~60 – 80 |
| **White Matter (WM)** | Short | Short | ~600 – 800 | ~70 – 80 |
| **Grey Matter (GM)** | Intermediate | Intermediate | ~900 – 1100 | ~90 – 100 |
| **Muscle** | Intermediate | Short | ~800 – 1000 | ~40 – 50 |
| **Free Water / CSF** | Long | Long | ~2000 – 4000 | ~2000 – 3000 |
| **Bone / Cortical** | Very Long | Very Short | N/A | < 1 |

---

## Which Parameters Influence Image Weighting?

Image contrast is selected by manipulating sequence timing (TR, TE) and excitation parameters (flip angle $\theta$, refocusing pulses).

### Spin Echo (SE) Sequences

A classic 90° excitation followed by a 180° refocusing pulse cancels static magnetic field inhomogeneities, leaving signal intensity proportional to:

$$S_{\text{SE}} \propto \text{PD} \cdot \left(1 - e^{-TR / T_1}\right) \cdot e^{-TE / T_2}$$

* **T1-Weighted (T1w):** Short $TR$ (allows incomplete $T_1$ recovery, highlighting $T_1$ differences) + Short $TE$ (minimizes $T_2$ decay).
* **T2-Weighted (T2w):** Long $TR$ (eliminates $T_1$ weighting by allowing full longitudinal recovery) + Long $TE$ (maximizes differences in $T_2$ decay rates).
* **Proton Density-Weighted (PDw):** Long $TR$ (minimizes $T_1$ weighting) + Short $TE$ (minimizes $T_2$ weighting).

### Gradient Echo (GE) Sequences & Flip Angle ($\theta$)

Gradient Echo sequences replace the 180° refocusing pulse with gradient reversals and low flip angles ($\theta < 90^\circ$). Because field inhomogeneities are not refocused, decay is governed by $T_2^*$:

$$S_{\text{GRE}} \propto \frac{\text{PD} \cdot \left(1 - e^{-TR / T_1}\right) \sin\theta}{1 - e^{-TR / T_1}\cos\theta} \cdot e^{-TE / T_2^*}$$

```{code-cell} python
:tags: [hide-input]  # Optionnel : masque le code par défaut pour ne laisser que la figurepython
import numpy as np
import matplotlib.pyplot as plt

# Interactive calculation / plot of T1 Recovery curves in Jupyter / MyST
tr_range = np.linspace(0, 3000, 500)
t1_fat = 260
t1_wm = 750
t1_gm = 1000
t1_csf = 3000

plt.figure(figsize=(8, 4))
plt.plot(tr_range, 1 - np.exp(-tr_range / t1_fat), label='Fat (Short T1)', color='orange')
plt.plot(tr_range, 1 - np.exp(-tr_range / t1_wm), label='White Matter', color='gray')
plt.plot(tr_range, 1 - np.exp(-tr_range / t1_gm), label='Grey Matter', color='brown')
plt.plot(tr_range, 1 - np.exp(-tr_range / t1_csf), label='CSF (Long T1)', color='blue')

plt.axvline(x=500, color='red', linestyle='--', label='Short TR (~500ms) - T1 Contrast')
plt.title("Longitudinal Magnetization Recovery (T1)")
plt.xlabel("TR (ms)")
plt.ylabel("Normalized M_z")
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

## Some Examples & Visual Analysis

Visual identification of weighting relies on checking high-signal (bright) vs. low-signal (dark) reference tissues rather than looking at scanner headers alone.

1. Brain MRI
* T1-Weighted:
- CSF appears dark (very long $T_1$, slow signal recovery).
- White Matter appears bright (short $T_1$, rapid recovery due to myelin fat).
- Grey Matter is intermediate (darker than WM).

* T2-Weighted:
- CSF appears hyperintense (bright) (long $T_2$, slow transverse signal decay).
- White Matter appears darker than Grey Matter.

* Proton Density (PD):
- High signal overall; excellent anatomical detail to distinguish WM/GM boundaries with minimal liquid/fat distortion.

2. Knee & Musculoskeletal MRI
* T1-Weighted: Subcutaneous and marrow fat are bright; cortical bone and ligamentous structures are dark; joint fluid is dark. Ideal for structural anatomy and bone marrow replacement.
* T2-Weighted: Joint fluid / effusion is bright white; muscle and meniscus are intermediate to dark. Ideal for highlighting fluid/edema (pathology).
* T2*-Weighted (Gradient Echo): Highly sensitive to susceptibility artifacts (e.g., microbleeds, iron deposits, joint hardware).

Extrait de code**Interactive Tissue Inspector (Tool Concept)**
If you wish to embed an interactive hover tooltip in MyST, you can use Plotly or Altair in Python code blocks.

```{code-cell} python
:tags: [hide-input]  # Optionnel : masque le code par défaut pour ne laisser que la figurepython
import plotly.express as px
import pandas as pd

# Data for interactive plot
data = pd.DataFrame({
    'Tissue': ['Fat', 'White Matter', 'Grey Matter', 'Muscle', 'CSF'],
    'T1_ms': [260, 750, 1000, 900, 4000],
    'T2_ms': [70, 75, 95, 45, 2000],
    'Category': ['Lipid', 'Brain', 'Brain', 'Soft Tissue', 'Fluid']
})

fig = px.scatter(
    data, x='T1_ms', y='T2_ms', text='Tissue', color='Category',
    hover_data=['T1_ms', 'T2_ms'],
    title="Interactive Tissue Relaxation Profile (Hover over points)"
)
fig.update_traces(textposition='top center', marker=dict(size=12))
fig.show()
```

---

## Recap Table

| Weighting | Repetition Time ($TR$) | Echo Time ($TE$) | Flip Angle ($\theta$) | Primary Bright Structures | Key Application |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **T1w (SE)** | Short ($< 800\text{ ms}$) | Short ($< 30\text{ ms}$) | $90^\circ$ | Fat, Subcutaneous tissue, Gadolinium contrast | Normal anatomical mapping |
| **T2w (SE)** | Long ($> 2000\text{ ms}$) | Long ($> 80\text{ ms}$) | $90^\circ$ | Water, CSF, Edema, Cysts | Fluid/Pathology detection |
| **PDw (SE)** | Long ($> 2000\text{ ms}$) | Short ($< 30\text{ ms}$) | $90^\circ$ | Tissues with high hydrogen proton concentration | Cartilage & joint assessment |
| **T2*w (GRE)**| Variable | Long / Medium | Small ($10^\circ - 30^\circ$) | Venous blood, Hemorrhage, Calcification | Microbleed & iron detection |

