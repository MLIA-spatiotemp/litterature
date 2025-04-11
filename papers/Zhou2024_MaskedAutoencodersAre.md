---
- date: 2024-12-05
- url: http://arxiv.org/abs/2403.17728
- DOI: 10.48550/arXiv.2403.17728
- citekey: Zhou2024_MaskedAutoencodersAre
---

# Masked Autoencoders are PDE Learners

#### Anthony Zhou, Amir Barati Farimani

### Abstract

Neural solvers for partial differential equations (PDEs) have great potential to generate fast and accurate physics solutions, yet their practicality is currently limited by their generalizability. PDEs evolve over broad scales and exhibit diverse behaviors; predicting these phenomena will require learning representations across a wide variety of inputs which may encompass different coefficients, boundary conditions, resolutions, or even equations. As a step towards generalizable PDE modeling, we adapt masked pretraining for physics problems. Through self-supervised learning across PDEs, masked autoencoders can consolidate heterogeneous physics to learn rich latent representations. We show that learned representations can generalize to a limited set of unseen equations or parameters and are meaningful enough to regress PDE coefficients or the classify PDE features. Furthermore, conditioning neural solvers on learned latent representations can improve time-stepping and super-resolution performance across a variety of coefficients, discretizations, or boundary conditions, as well as on certain unseen PDEs. We hope that masked pretraining can emerge as a unifying method across large, unlabelled, and heterogeneous datasets to learn latent physics at scale.

---

## Ojective

Adapting Masked Auto-Encoding to PDE solver pre-training.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

Adopts a **regression** setup.

Can be used for **multiphysics** and **multitasking** through **finetuning**.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input

The **evolution** of a physical state over time on a **1D or 2D** **regular** grid.

#### Encoding

The input is patched to generate tokens the same way as in a ViT.

These tokens are then fed to a Transformer Encoder to encode a latent sequence.

#### Decoder

There are different decoders depending on the task at hand :
- for **pre-training**, the Transformer Encoder is trained alongside a Transformer Decoder in a Masked Auto-Encoding manner to reconstruct the partially masked input sequence;
- for **finetuning** :
	- for classification and regression of physical systems and their parameters, a simple MLP is used to project the CLS token to prediction features;
	- for time-stepping and super-resolution, the encoder is used a a conditioner for another model that can be FNO, U-Net, or  SIR; 

## Experiments

In all PDEs, coefficients and forcing terms are randomly sampled to produce diverse dynamics within a dataset.

### 1D

#### Data

The model is pre-trained on the **KdV-Burgers** equation only.

#### Results

MAE models can learn coefficient-dependent trends. This can be applied to PDEs not seen during training.

Representations learned from pretraining only on the KdV-Burgers equations allow models to cluster PDE samples from the Heat, Burgers, Advection, and KS equations.

The frozen MAE encoder is able to outperform a supervised baseline on the KdV-B, Heat, PDEs, and Res. tasks despite never seeing labels throughout training ; further performance gains can be realized by fine-tuning the MAE encoder.

MAE conditioning consistently improves time-stepping and super-resolution performance.

### 2D

#### Data

The model is pre-trained on the **Heat**, **Advection**, and **Burgers** equations simultaneously.

#### Results

Masked pretraining serves as a good initialization for supervised fine-tuning, of which the effects are more pronounced.

Improvements from the MAE conditioning tend to be more pronounced in 2D.

Out-of-distribution datasets are still challenging ; when extrapolating pretrained encoders to new PDEs, such as the Navier-Stokes equations, the performance is limited.

## Limitations

The results in time-stepping and super-resolution would likely not outperform a foundation model trained specifically for these tasks.