---
title: T2 and T2*
kernelspec:
  name: base
  display_name: Python 3
---

## What is T2* ?
définition de T2*
comment ça influence en vrai les images ?
code

## T2* song
chanson (lien ou vidéo carrément si c'est possible)
paroles + analyse
code de l'analyse

## Recap
tableau récap des différences de T2 et T2*


# T2 and T2* Relaxation Physics

While $T_1$ relaxation measures longitudinal recovery back to equilibrium, **$T_2$** and **$T_2^*$** relaxation describe the loss of transverse magnetization ($M_{xy}$) in the transverse plane due to spin dephasing.

---

## What is T2*?

### Definition and Physics

When radiofrequency (RF) excitation tips magnetization into the transverse plane, spins initially precess in phase. Over time, they lose phase coherence, causing the transverse signal $M_{xy}$ to decay exponentially.

This transverse signal decay happens at two distinct rates:

1. **$T_2$ Relaxation (True Spin-Spin Decay):** Irreversible phase loss caused by micro-scale magnetic interactions between neighboring proton spins (molecular "crosstalk").
2. **$T_2^*$ Relaxation (Observed Decay):** Rapid effective decay caused by pure $T_2$ relaxation **PLUS static magnetic field inhomogeneities** ($\Delta B_0$).

$$\frac{1}{T_2^*} = \frac{1}{T_2} + \frac{1}{T_{2,\text{inhom}}}$$

Since $T_{2,\text{inhom}} > 0$, $T_2^*$ is always significantly shorter than $T_2$ ($T_2^* < T_2$).

<Image src="image_agent_tag_9245813729334519079" alt="Diagram showing T2 and T2* decay curves over time" caption="Comparison of T2 vs T2* decay curves" />

---

### How T2* Influences Real MRI Images

* **Gradient Echo (GRE) Sensitivity:** GRE sequences lack a 180° refocusing pulse, making them directly weighted by $T_2^*$ rather than $T_2$.
* **Susceptibility Artifacts:** Tissues with iron, blood breakdown products (hemosiderin, deoxyhemoglobin), or interfaces between air and tissue create localized magnetic field gradients, causing fast signal loss ("blooming artifacts").
* **Functional MRI (fMRI):** Blood Oxygen Level Dependent (BOLD) fMRI relies entirely on local $T_2^*$ changes caused by paramagnetism in blood flow.

<Image src="image_agent_tag_92458137293345190602" alt="Comparison between Gradient Echo and Spin Echo MRI sequences" caption="GE (T2*) vs SE (T2) sensitivity" />

---

### Interactive Python Simulation: $T_2$ vs $T_2^*$ Signal Decay

```{code-cell} python

import numpy as np
import matplotlib.pyplot as plt

# Time vector in milliseconds
t = np.linspace(0, 200, 500)

# Typical brain tissue relaxation values (ms)
T2_tissue = 80.0          # Pure T2 relaxation
T2_inhom = 25.0           # Decay rate due to local field inhomogeneity

# Calculate T2* rate
T2_star = 1.0 / ((1.0 / T2_tissue) + (1.0 / T2_inhom))

# Calculate transverse magnetization signals
M_xy_T2 = np.exp(-t / T2_tissue)
M_xy_T2_star = np.exp(-t / T2_star)

plt.figure(figsize=(8, 4))
plt.plot(t, M_xy_T2, label=f'Spin Echo ($T_2$ = {T2_tissue:.0f} ms)', color='darkblue', linewidth=2)
plt.plot(t, M_xy_T2_star, label=f'Gradient Echo ($T_2^*$ = {T2_star:.1f} ms)', color='crimson', linestyle='--', linewidth=2)

plt.axhline(y=0.37, color='gray', linestyle=':', label='37% Signal Threshold')
plt.title("$T_2$ vs $T_2^*$ Decay Curves in Transverse Magnetization ($M_{xy}$)")
plt.xlabel("Time / Echo Time (ms)")
plt.ylabel("Normalized Transverse Signal ($M_{xy}$)")
plt.grid(True, alpha=0.3)
plt.legend()
plt.show()
```

### The $T_2^*$ Song: "Twinkle, Twinkle, $T_2^*$"

To help memorize the physical concepts governing spin dephasing and magnetic field inhomogeneity, listen to this scientific adaptation of Twinkle, Twinkle, Little Star written by Greg Crowther and performed by Science Groove.

# Song Performance
Lyrics & Physics Breakdown

|Song Lyrics|Physical Concept & Meaning|
|---|---|
|Twinkle, twinkle, $T_2^$,How I wonder what you are!|Introduction to $T_2^*$ as an observed relaxation rate in NMR/MRI.|
|XY signal soon decays;Why do spins go out of phase?|Explains that transverse signal decay in the $XY$ plane is directly caused by phase coherence loss among precessing spins.|
|Twinkle, twinkle, $T_2^$,Something pulls those spins apart. |Field inhomogeneities cause neighboring spins to precess at slightly different Larmor frequencies, pulling vector phases apart. |
|Spin-spin crosstalk sets $T_2$,But by then $T_2^$ is through.|Pure molecular $T_2$ interaction takes longer, whereas static inhomogeneity causes $T_2^*$ to decay much faster.|
|A brief duration here is sealed; By an inhomogeneous field.| The short lifespan of $T_2^*$ signal decay is dictated by static field non-uniformities ($\Delta B_0$).|

# Code Analysis: Quantifying Dephasing Rates from Song Concepts
This Python snippet models how individual spin vectors spread out in phase over time under fixed field inhomogeneities, reproducing the physical process described in the song.

```{code-cell} python
import numpy as np
import matplotlib.pyplot as plt

# Simulate 100 individual spins with slightly different off-resonance frequencies
np.random.seed(42)
num_spins = 100
time_steps = np.linspace(0, 0.05, 300) # 0 to 50 ms

# Off-resonance frequency dispersion (Hz) due to field inhomogeneity
delta_f = np.random.normal(0, 30, num_spins)

# Calculate phase accumulation for each spin over time
phases = 2 * np.pi * np.outer(delta_f, time_steps)

# Sum individual spin vectors to compute net transverse magnetization magnitude
net_vector_x = np.mean(np.cos(phases), axis=0)
net_vector_y = np.mean(np.sin(phases), axis=0)
net_magnitude = np.sqrt(net_vector_x**2 + net_vector_y**2)

plt.figure(figsize=(8, 3.5))
plt.plot(time_steps * 1000, net_magnitude, color='purple', label='Net Transverse Magnetization')
plt.title("Dephasing of 100 Spins Due to Magnetic Inhomogeneity (Inhomogeneous Broadening)")
plt.xlabel("Time (ms)")
plt.ylabel("Coherence Magnitude")
plt.grid(True, alpha=0.3)
plt.legend()
plt.show()
```

# Summary: Comparing $T_2$ and $T_2^*$

|Property|T2​ Relaxation|T2∗​ Relaxation|
|---|---|---|
|Full Name|Spin-Spin Relaxation|Effective Transverse Relaxation
|Physical Mechanism|Intrinsic molecular dipolar interactions|Pure $T_2$ + Static field inhomogeneities ($\Delta B_0$)|
|Reversibility|Irreversible (random thermal motions)|Reversible using a 180° RF pulse|
|Relative Duration|Longer ($T_2 > T_2^*$)|Shorter ($T_2^* < T_2$)|
|Primary Sequence|Spin Echo (SE)|Gradient Echo (GE)|
|Clinical Utility|Edema, tumors, fluid detection|fMRI (BOLD), microbleeds, calcification, iron quantification|
