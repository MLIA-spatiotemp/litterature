---
- date: 2025-01-31
- url: http://arxiv.org/abs/2501.18972
- DOI: 10.48550/arXiv.2501.18972
- citekey: Liu2025_BCATBlockCausal
---

# BCAT: A Block Causal Transformer for PDE Foundation Models for Fluid Dynamics

#### Yuxuan Liu, Jingmin Sun, Hayden Schaeffer

### Abstract

We introduce BCAT, a PDE foundation model designed for autoregressive prediction of solutions to two dimensional fluid dynamics problems. Our approach uses a block causal transformer architecture to model next frame predictions, leveraging previous frames as contextual priors rather than relying solely on sub-frames or pixel-based inputs commonly used in image generation methods. This block causal framework more effectively captures the spatial dependencies inherent in nonlinear spatiotemporal dynamics and physical phenomena. In an ablation study, next frame prediction demonstrated a 2.9x accuracy improvement over next token prediction. BCAT is trained on a diverse range of fluid dynamics datasets, including incompressible and compressible Navier-Stokes equations across various geometries and parameter regimes, as well as the shallow-water equations. The model's performance was evaluated on 6 distinct downstream prediction tasks and tested on about 8K trajectories to measure robustness on a variety of fluid dynamics simulations. BCAT achieved an average relative error of 1.92% across all evaluation tasks, outperforming prior approaches on standard benchmarks.

---

## Ojective

Doing next frame prediction and demonstrating its superiority over next token prediction for PDE solving.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

Performs **regression** over the next frame.

Is **not properly evaluated on unseen parameters or physics**, it is always **trained from scratch**.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input

The **past history** of a 2D physical state from $0$ to $T_0$. Can only handle data on a **regular grid** in this vanilla implementation.

#### Encoding

**Each frame** in the input data is divided into **non-overlapping patches**, which are then flattened and projected into a sequence of **visual tokens**.

All input data is **zero-padded** to $c = 4$ to unify the number of channels.

#### Approximator

The approximator is a **block causal Transformer**, meaning that every token only sees tokens from the same frame or from the previous frames, and that **the entire next frame is predicted in one step**, rather than only one token from that frame.

A decoder-only transformer similar to **GPT-2** and **Llama-3** is used for the architecture.

#### Decoder

An **MLP** is used to reconstruct the frames from the corresponding tokens.

## Experiments

### Data

The dataset consists of fluid dynamics PDEs collected from 3 sources: **PDEBench**, **PDEArena**, and **CFDBench**.

The dataset includes **shallow water** equations and the **Navier-Stokes** equation with incompressible and compressible flow and with different buoyancy settings.

### Results

The BCAT model **beats MPP and DPOT**.

It achieves the **best results on 3 families** and the **second-best results on the other 3 families**. It is only beaten by ViT, which uses a similar structure to BCAT but a different training objective.

## Limitations

Has yet to be adapted to irregular geometries.

Hasn't been properly tested on generalization to unseen parameters or unseen physics.