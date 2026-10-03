---
title: 'Image types: T1, T2, PD'
kernelspec:
  name: python3
  display_name: Python 3
---

A ajouter:
- référence spin bench pour l'animation et schéma des séquences
- ajout des images interactives: en passant la souris sur une structure ça nous dit ce que c'est, son T1 et T2


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

:::{figure} images/spinT1T2.gif
:label: fig-spin-T1-T2
T1 and T2 relaxation after RF Excitation
:::

:::{admonition} Why is T1 always strictly greater than T2?
:class: tip

**Fundamental Principle:** Transverse relaxation ($T_2$) relies on losing phase coherence between spins. Longitudinal relaxation ($T_1$) requires energy transfer to the surrounding lattice to return to equilibrium.

* **Dephasing requires no energy transfer:** Spins can lose phase alignment simply due to tiny local magnetic field variations.
* **Energy transfer requires time:** Re-establishing $M_z$ requires protons to release energy ($RF$) at the exact **Larmor frequency** to the lattice molecules.
* **The Physics Constraint:** Because every energy-exchange event ($T_1$) also destroys phase alignment ($T_2$), transverse coherence decays at least as fast as energy is lost. Therefore, $T_2$ is physically bounded by $T_1$:

$$T_2 \le T_1$$

In biological tissues, micro-molecular magnetic interactions cause dephasing to occur **orders of magnitude faster** than thermal energy transfer, making $T_2$ (tens to hundreds of ms) much shorter than $T_1$ (hundreds to thousands of ms).
:::

### Molecular Mechanisms Driving T1 & T2 in Tissues

Relaxation rates depend on how closely the **tumbling frequency** of molecules matches the **Larmor frequency** ($\omega_0$) of hydrogen protons:

1. **Free Water & CSF (Long $T_1$, Very Long $T_2$):**
   * Small water molecules tumble extremely fast—much faster than $\omega_0$.
   * **$T_1$ is Long:** Inefficient energy exchange with the lattice.
   * **$T_2$ is Very Long:** Fast molecular motion averages out local magnetic variations, preserving phase coherence for a long time.

2. **Fat & Lipids (Short $T_1$, Short/Medium $T_2$):**
   * Medium-sized hydrocarbon chains tumble at a rate very close to the Larmor frequency.
   * **$T_1$ is Very Short:** Highly efficient energy transfer allows rapid recovery of $M_z$.
   * **$T_2$ is Short:** Dipolar interactions between closely packed hydrogen atoms accelerate phase loss.

3. **Solid Tissues, Macromolecules & Cortical Bone (Long $T_1$, Very Short $T_2$):**
   * Protons bound to rigid protein matrices or mineralized bone structures tumble very slowly.
   * **$T_2$ is Extremely Short ($< 1\text{ ms}$):** Fixed spatial arrangements create strong local magnetic gradients, causing near-instantaneous dephasing. The signal disappears before typical echo times ($TE$) can capture it.

Here is a table giving the typical relaxation times across human tissues at $B_0 = 1.5\text{ T}$:

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

A SE sequence is composed of 2 RF excitation pulses of 90° and 180° refocusing pulse between the excitation and data acquisition in order to refocus the effects of off-resonance and create pure T2-weighting:

:::{figure} images/SEseq.png
:label: fig_SEseq
Spin Echo sequence - T2 contrast
:::

### Gradient Echo (GE) Sequences & Flip Angle ($\theta$)

Gradient Echo sequences replace the 180° refocusing pulse with gradient reversals and low flip angles ($\theta < 90^\circ$). Because field inhomogeneities are not refocused, decay is governed by $T_2^*$:

$$S_{\text{GE}} \propto \frac{\text{PD} \cdot \left(1 - e^{-TR / T_1}\right) \sin\theta}{1 - e^{-TR / T_1}\cos\theta} \cdot e^{-TE / T_2^*}$$

A GE sequence is composed of an RF excitation pulse followed by imaging gradients:

:::{figure} images/GEseq.png
:label: fig_GEseq
Gradient Echo sequence - T2* contrast
:::

```{code-cell} python
:tags: [hide-input]

import plotly.express as px
import pandas as pd

# Data representing T1 and T2 values at 1.5 Tesla across key tissues
tissue_data = pd.DataFrame({
    'Tissue': [
        'Fat', 
        'White Matter (WM)', 
        'Grey Matter (GM)', 
        'Muscle', 
        'CSF / Free Water', 
        'Cortical Bone', 
        'Tendon / Ligament'
    ],
    'T1_ms': [280, 780, 1080, 900, 3500, 1200, 800],
    'T2_ms': [70, 75, 95, 45, 2200, 0.5, 5],
    'Category': [
        'Lipid', 
        'Brain', 
        'Brain', 
        'Soft Tissue', 
        'Fluid', 
        'Bone / Solid', 
        'Connective'
    ]
})

fig = px.scatter(
    tissue_data, 
    x='T1_ms', 
    y='T2_ms', 
    text='Tissue', 
    color='Category',
    log_y=True,  # Logarithmic scale used due to the massive range of T2 (0.5 ms to 2200 ms)
    labels={'T1_ms': 'T1 Relaxation Time (ms)', 'T2_ms': 'T2 Relaxation Time (ms, log scale)'},
    title="Interactive Relaxation Profile Across Human Tissues (1.5T)"
)

fig.update_traces(textposition='top center', marker=dict(size=12))
fig.update_layout(height=500, template="plotly_white")
fig.show()
```
A logarithmic scale is applied to the vertical axis ($T_2$) to clearly compare solid tissues (Cortical Bone $T_2 \approx 0.5\text{ ms}$) with free fluids (CSF $T_2 \approx 2200\text{ ms}$) on the same axis. Tissues in the **top-left** (short $T_1$, long $T_2$) yield high signal intensity easily, whereas tissues in the **bottom-right** require specific sequence strategies (e.g., Ultra-short $TE$ / UTE sequences) to be visualized.

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

**Interactive Tissue Inspector (Tool Concept)**
If you wish to embed an interactive hover tooltip in MyST, you can use Plotly or Altair in Python code blocks.

```{code-cell} python
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
The curve models $M_z(t) = 1 - e^{-TR / T_1}$. At short $TR$ ($\sim 500\text{ ms}$), fat has recovered nearly all its longitudinal magnetization ($M_z \approx 0.85$), whereas CSF has barely recovered ($M_z \approx 0.15$). This vast signal difference creates high **$T_1$ contrast**.

---

## Recap Table

| Weighting | Repetition Time ($TR$) | Echo Time ($TE$) | Flip Angle ($\theta$) | Primary Bright Structures | Key Application |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **T1w (SE)** | Short ($< 800\text{ ms}$) | Short ($< 30\text{ ms}$) | $90^\circ$ | Fat, Subcutaneous tissue, Gadolinium contrast | Normal anatomical mapping |
| **T2w (SE)** | Long ($> 2000\text{ ms}$) | Long ($> 80\text{ ms}$) | $90^\circ$ | Water, CSF, Edema, Cysts | Fluid/Pathology detection |
| **PDw (SE)** | Long ($> 2000\text{ ms}$) | Short ($< 30\text{ ms}$) | $90^\circ$ | Tissues with high hydrogen proton concentration | Cartilage & joint assessment |
| **T2*w (GE)**| Variable | Long / Medium | Small ($10^\circ - 30^\circ$) | Venous blood, Hemorrhage, Calcification | Microbleed & iron detection |

