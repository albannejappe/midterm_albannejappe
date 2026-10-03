---
title: Image types: T1, T2, T2*, PD
kernelspec:
  name: base
  display_name: Python 3
---

## What are T1 and T2 ?
def T1, T2
tableau types de tissu et T1, T2 long, mid, court

## Which parameters influence the image type (weight) ?
explication des différentes séquences (spin echo, gradient echo, theta...) qui donnent chaque type d'image
code?

## Some examples
exemples (plusieurs! de différentes parties du corps) de chaque type d'image en expliquant pourquoi telle image est T1-weighted...
(pas seulement en se basant sur TE et TR, mais plus avec l'aspect des différents tissus)
figure interactive: en passant la souris sur une structure ça nous dit ce que c'est, son T1 et T2 (jsp si c'est possible de faire ça)

## Recap
tableau recap TE/TR


# Image Types & Weightings in MRI

In Magnetic Resonance Imaging (MRI), contrast between tissues is primarily determined by intrinsic magnetic relaxation properties (**T1**, **T2**, and **T2*** times) and proton density (**PD**). By adjusting scanner parameters such as Time to Echo ($TE$), Repetition Time ($TR$), and flip angle ($\theta$), we can "weight" the image toward a specific physical property.

---

## What are T1 and T2?

### Fundamental Definitions

* **T1 Relaxation (Spin-Lattice / Longitudinal Relaxation):** The process by which the longitudinal magnetization ($M_z$) recovers back to its thermal equilibrium value ($M_0$) along the main magnetic field ($B_0$). $T_1$ is defined as the time required for $M_z$ to recover to approximately **63%** of $M_0$.
* **T2 Relaxation (Spin-Spin / Transverse Relaxation):** The process by which transverse magnetization ($M_{xy}$) decays due to spin phase coherence loss caused by microscopic magnetic interactions between neighboring nuclei. $T_2$ is defined as the time required for $M_{xy}$ to drop to **37%** of its initial magnitude.
* **T2* Relaxation:** Transverse decay caused by a combination of pure $T_2$ spin-spin relaxation **plus magnetic field inhomogeneities** ($\Delta B_0$). Always shorter than $T_2$ ($T_2^* < T_2$).

$$M_z(t) = M_0 \left(1 - e^{-t / T_1}\right) \quad \text{and} \quad M_{xy}(t) = M_0 e^{-t / T_2}$$

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

Image contrast is selected by manipulating sequence timing ($TR$, $TE$) and excitation parameters (flip angle $\theta$, refocusing pulses).

### Spin Echo (SE) Sequences

A classic 90° excitation followed by a 180° refocusing pulse cancels static magnetic field inhomogeneities, leaving signal intensity proportional to:

$$S_{\text{SE}} \propto \text{PD} \cdot \left(1 - e^{-TR / T_1}\right) \cdot e^{-TE / T_2}$$

* **T1-Weighted (T1w):** Short $TR$ (allows incomplete $T_1$ recovery, highlighting $T_1$ differences) + Short $TE$ (minimizes $T_2$ decay).
* **T2-Weighted (T2w):** Long $TR$ (eliminates $T_1$ weighting by allowing full longitudinal recovery) + Long $TE$ (maximizes differences in $T_2$ decay rates).
* **Proton Density-Weighted (PDw):** Long $TR$ (minimizes $T_1$ weighting) + Short $TE$ (minimizes $T_2$ weighting).

### Gradient Echo (GRE) Sequences & Flip Angle ($\theta$)

Gradient Echo sequences replace the 180° refocusing pulse with gradient reversals and low flip angles ($\theta < 90^\circ$). Because field inhomogeneities are not refocused, decay is governed by $T_2^*$:

$$S_{\text{GRE}} \propto \frac{\text{PD} \cdot \left(1 - e^{-TR / T_1}\right) \sin\theta}{1 - e^{-TR / T_1}\cos\theta} \cdot e^{-TE / T_2^*}$$

```python
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
