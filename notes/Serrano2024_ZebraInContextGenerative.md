- date: 2024-10-08
- url: [http://arxiv.org/abs/2410.03437](http://arxiv.org/abs/2410.03437)
- DOI: [10.48550/arXiv.2410.03437](https://doi.org/10.48550/arXiv.2410.03437)
- citekey: Serrano2024_ZebraInContextGenerative
---

# Zebra: In-Context and Generative Pretraining for Solving Parametric PDEs

## Louis Serrano, Armand Kassaï Koupaï, Thomas X. Wang, Pierre Erbacher, Patrick Gallinari

### Abstract

Solving time-dependent parametric partial differential equations (PDEs) is challenging, as models must adapt to variations in parameters such as coefficients, forcing terms, and boundary conditions. Data-driven neural solvers either train on data sampled from the PDE parameters distribution in the hope that the model generalizes to new instances or rely on gradient-based adaptation and meta-learning to implicitly encode the dynamics from observations. This often comes with increased inference complexity. Inspired by the in-context learning capabilities of large language models (LLMs), we introduce Zebra, a novel generative auto-regressive transformer designed to solve parametric PDEs without requiring gradient adaptation at inference. By leveraging in-context information during both pre-training and inference, Zebra dynamically adapts to new tasks by conditioning on input sequences that incorporate context trajectories or preceding states. This approach enables Zebra to flexibly handle arbitrarily sized context inputs and supports uncertainty quantification through the sampling of multiple solution trajectories. We evaluate Zebra across a variety of challenging PDE scenarios, demonstrating its adaptability, robustness, and superior performance compared to existing approaches.

## Ojective

Adapting language model pretraining techniques to design a generative model for solving parametric PDEs.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

Solves a **regression** problem and is capable of adapting to new PDEs **parameters** through **in-context learning**.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input

The inputs can be different arrangements of trajectories $u^{<t}_i$ and an initial state $u^0_I$, or the past trajectory $u^{<t}$ up to the time of prediction $t$.

#### Encoding

The model encodes and decodes inputs through a **VQ-VAE**.

It uses a **CNN** to patch the input $u^t$ and then **quantizes each patch** using a learned codebook $\mathcal Z$.

It does so for each frame and flattens it all into a token sequence.

#### Backbone

These tokens are then fed to a **large transformer** with special tokens to separate each trajectory, which can do zero-shot or few-shots in-context modeling after being trained on next token prediction.

#### Decoding

The decoding simply reverses the encoding process, directly decoding the quantized output tokens through a CNN.

## Experiments

### Data

Experiments are conducted on seven datasets :
- 5 in 1D : **Advection**, **Heat**, **Burgers**, **Wave-b**, **Combined** ;
- 2 in 2D : **Wave 2D**, **Vorticity** ;

with **varying** PDE coefficients, boundary conditions, and forcing term. 

### Results

Zebra demonstrates strong overall performance in the one-shot adaptation setting, often surpassing baseline methods that have been trained specifically for this task.

It competes with MPP on the temporal conditioning setting where it is given only two frames, while exhibiting a high flexibility in the number of frames it needs (while MPP shows worse performances when evaluated with a different number of frames in input as in training).

It enables uncertainty quantification through its generative formulation and how its variance is conditionned on the heat parameter $\tau$.

## Limitations

The decoder’s ability to reconstruct details from the quantized latent space limits the quality of the generated trajectories.

The use of convolutions in the encoder and decoder restricts their use to regular domains.