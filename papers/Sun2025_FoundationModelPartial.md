---
- date: 2025-01-31
- url: http://arxiv.org/abs/2404.12355
- DOI: 10.48550/arXiv.2404.12355
- citekey: Sun2025_FoundationModelPartial
---

# Towards a Foundation Model for Partial Differential Equations: Multi-Operator Learning and Extrapolation

#### Jingmin Sun, Yuxuan Liu, Zecheng Zhang, Hayden Schaeffer

### Abstract

Foundation models, such as large language models, have demonstrated success in addressing various language and image processing tasks. In this work, we introduce a multi-modal foundation model for scientific problems, named PROSE-PDE. Our model, designed for bi-modality to bi-modality learning, is a multi-operator learning approach which can predict future states of spatiotemporal systems while concurrently learning the underlying governing equations of the physical system. Specifically, we focus on multi-operator learning by training distinct one-dimensional time-dependent nonlinear constant coefficient partial differential equations, with potential applications to many physical applications including physics, geology, and biology. More importantly, we provide three extrapolation studies to demonstrate that PROSE-PDE can generalize physical features through the robust training of multiple operators and that the proposed model can extrapolate to predict PDE solutions whose models or data were unseen during the training. Furthermore, we show through systematic numerical experiments that the utilization of the symbolic modality in our model effectively resolves the well-posedness problems with training multiple operators and thus enhances our model's predictive capabilities.

---

## Ojective

Building a multimodal model that can take as inputs the past evolution of a physical state as well as a symbolic expression representing its governing equation (or an approximated guess) and combine them to predict the future evolution as well as a better approximation of the governing equation.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

Poses the problem as a **regression** problem.

Adapts to **unseen physics** without finetuning, only using **in context information** given by an equation guess.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input

The inputs consists of :
- the **previous evolution** (data input) of the physical state up to $t_0$ ; the physical state is here defined on a **1D** domain;
- **an equation guess** which can be empty or erroneous;

#### Encoding

- the data input is first passed through an **MLP** to generate tokens corresponding to each timestep $t$ ; each token is then fed to a **self-attention encoder** along with their respective time $t$ as position encodings;
- the equation input is first translated is symbolic notation using the **Polish Notation**, and is then embedded as a normal text sequence to then be fed to a **self-attention encoder**;

#### Approximator

The two token sequences previously generated are concatenated and fused by a **Feature Fusion** block using **self-attention**, which outputs fused data feature tokens and fused symbol feature tokens.

#### Decoder

- the data decoder decodes the fused data feature tokens using cross-attention with **different future times query** locations ; the decoded queries are projected back to the input space through an **MLP**;
- the symbol decoder works like an **auto-regressive text generator** and uses the fused symbol feature tokens **as conditioning** to generate the updated symbolic representation of the equation;

## Experiments

### Data

For each of the following system, a parameter family is created by randomly sampling PDEs’ parameters uniformly within a 10% value variation around a point-of-interest :
- Diffusion Equation
- Porous Medium Equation
- Klein-Gordon Equation
- Sine-Gordon Equation
- Cahn-Hilliard Equation
- Korteweg–De Vries (Kdv) Equation
- Advection Equation
- Wave Equation
- Diffusion Reaction Equation
- Viscous Conservation Law
- Inviscid Conservation Law
- Fokker-Planck Equation

### Results

Performs better than FNO or DeepONet, but relies on the symbolic encoder to do so.

Generalises to unseen coefficients and unseen physics.

Performs well even when only the skeleton of the equation (with coefficients unknown) is given.

## Limitations

Relies on the symbolic encoder to generalise to unseen physics or even just to perform well on known physics.