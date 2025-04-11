---
- date: 2024-11-01
- url: http://arxiv.org/abs/2403.12553
- DOI: 10.48550/arXiv.2403.12553
- citekey: Rahman2024_PretrainingCodomainAttention
---

# Pretraining Codomain Attention Neural Operators for Solving Multiphysics PDEs

#### Md Ashiqur Rahman, Robert Joseph George, Mogab Elleithy, Daniel Leibovici, Zongyi Li, Boris Bonev, Colin White, Julius Berner, Raymond A. Yeh, Jean Kossaifi, Kamyar Azizzadenesheli, Anima Anandkumar

### Abstract

Existing neural operator architectures face challenges when solving multiphysics problems with coupled partial differential equations (PDEs) due to complex geometries, interactions between physical variables, and the limited amounts of high-resolution training data. To address these issues, we propose Codomain Attention Neural Operator (CoDA-NO), which tokenizes functions along the codomain or channel space, enabling self-supervised learning or pretraining of multiple PDE systems. Specifically, we extend positional encoding, self-attention, and normalization layers to function spaces. CoDA-NO can learn representations of different PDE systems with a single model. We evaluate CoDA-NO's potential as a backbone for learning multiphysics PDEs over multiple systems by considering few-shot learning settings. On complex downstream tasks with limited data, such as fluid flow simulations, fluid-structure interactions, and Rayleigh-B\'enard convection, we found CoDA-NO to outperform existing methods by over 36%.

---

## Ojective

Desining a way to handle varying numbers and types of physics coefficients during pre-training and fine-tuning.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

The problem is formulated as a **regression** problem.

Adapts to **unseen parameters** (as well as **slightly different physics**) and **unseen coefficients** through **finetuning**.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input

The input data consists of the initial function $u_0$ **on a regular or irregular grid**.

#### Encoding

The input function is split along the channels dimension to **separate each physical variable** as a a single variable function $u_0 = [u_0^{v_1}, \dots, u_0^{v_d}]$ ; **positional encoders** are learned for each of these variables and concatenated to their respective single variable function.

**GNO** (or FNO in the case of a regular grid) is applied on each of these couples to generate **token-functions** ; each token-function corresponds to a physical variable in the co-domain.

#### Approximator

The token-functions are fed to a **Transformer** to approximate them at $t+1$.

#### Decoder

**GNO** (or FNO in the case of a regular grid) is applied on each token function to obtain each variable at $t+1$.

#### Training

The model can be pre-trained for reconstruction on a set of physical quantities and finetuned for prediction **with a different set of physical quantities**.

## Experiments

### Data

Experiments are conduced on **fluid-structure interaction** and **Rayleigh-Bénard convection** system. The model is also tested on a diverse set of PDEs from **PDEBench**.

### Results

Performs better than GINO, DeepONet, GNN, ViT and U-Net.

The performance gain is higher when the **number of few-shot examples is very low**, and the model can **seamlessly adapt to more turbulent flow** and outperform baselines with a significant margin.

It shows the ability to adapt to the more challenging NS+EW dataset when pretrained solely on the NS dataset.

## Limitations

CoDA-NO’s performance on target PDEs is influenced by the number of training examples.