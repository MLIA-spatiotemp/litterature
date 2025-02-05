- date: 2023-10-04
- url: [http://arxiv.org/abs/2310.02994](http://arxiv.org/abs/2310.02994)
- DOI: [10.48550/arXiv.2310.02994](https://doi.org/10.48550/arXiv.2310.02994)
- citekey: McCabe2023_MultiplePhysicsPretraining
---

# Multiple Physics Pretraining for Physical Surrogate Models

#### Michael McCabe, Bruno Régaldo-Saint Blancard, Liam Holden Parker, Ruben Ohana, Miles Cranmer, Alberto Bietti, Michael Eickenberg, Siavash Golkar, Geraud Krawezik, Francois Lanusse, Mariel Pettee, Tiberiu Tesileanu, Kyunghyun Cho, Shirley Ho

### Abstract

We introduce multiple physics pretraining (MPP), an autoregressive task-agnostic pretraining approach for physical surrogate modeling. MPP involves training large surrogate models to predict the dynamics of multiple heterogeneous physical systems simultaneously by learning features that are broadly useful across diverse physical tasks. In order to learn effectively in this setting, we introduce a shared embedding and normalization strategy that projects the fields of multiple systems into a single shared embedding space. We validate the efficacy of our approach on both pretraining and downstream tasks over a broad fluid mechanics-oriented benchmark. We show that a single MPP-pretrained transformer is able to match or outperform task-specific baselines on all pretraining sub-tasks without the need for finetuning. For downstream tasks, we demonstrate that finetuning MPP-trained models results in more accurate predictions across multiple time-steps on new physics compared to training from scratch or finetuning pretrained video foundation models. We open-source our code and model weights trained at multiple scales for reproducibility and community experimentation.

---

## Ojective

Training large visual transformers based surrogate models on multiple heterogeneous physical systems simultaneously.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->
MPP addresses **regression** problems through **fine-tuning** and operates within a **multi-physics** context.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input

Inputs consist of a past evolution of the system $u \in \mathbb{R}^{T\times H\times W\times C}$ (supporting arbitrary channel counts and resolutions) up to $u(t)$ for which we want to predict the future evolution.

#### Encoding

A reversible instance normalization (**ReVIN**) is applied to unify input magnitudes.

Channels are then projected using convolutional filters $\mathbf e_{v_i}$ proper to each physical field $v_i$ that are then added in a **shared embedding space** $\mathbf e(x, t) = \sum_i v_i(x, t) \mathbf e_{v_i}$ where $\mathbf e(x, t) \in \mathbb R^N$. This setup supports fine-tuning for unseen fields by adding new channels.

Resolution space is finally **cut into patches** using strided convolutions.

#### Backbone

The model employs a **ViT** architecture with temporal self-attention, spatial **axial self-attention** (independently in the $x$ and $y$ axes), and a final MLP layer.

#### Decoding

Decoding involves reconstructing the fields from the shared embedding space via a projection layer and reversing the normalization (**De-ReVIN**).

## Experiments

### Data

To evaluate their method, the team uses 2D datasets from **PDEBench**, which includes four PDE systems:
- compressible and incompressible Navier-Stokes ;
- Shallow-water ;
- 2D Diffusion-Reaction ;

and feature diverse state variables, resolutions, initial/boundary conditions, and simulation parameters.

### Results

##### Transfer to Low-Data Domains

The compressible fluid data was excluded from pretraining to evaluate transfer capabilities. Fine-tuning on specific compressible Navier-Stokes datasets ("Near" and "Far") showed that MPP-pretrained models **outperform both training from scratch and VideoMAE**, particularly in **low-data regimes**.

"Near" data reflects behavior closer to incompressible simulations, while "Far" data features turbulent conditions unseen during pretraining. MPP models achieve greater accuracy in short-term predictions, with larger benefits on "Far" data.

##### Broader Usage of Pretrained Representations

Inverse problem tasks were evaluated using the incompressible Navier-Stokes system for:

- Identifying constant forcing terms using trajectory data. MPP reduced RMSE by half compared to training from scratch.
- Estimating buoyancy parameters using unseen PDEArena data. MPP did not outperform the optimal constant prediction, suggesting limitations in scalar prediction tasks.

Results highlight MPP's strength in dense prediction tasks resembling pretraining, but limited benefits for scalar prediction.

#### Scaling and Efficiency

Pretraining on MPP required significantly less memory compared to VideoMAE for equivalent input sizes. MPP scales efficiently, with memory usage ranging from 6.7 GB to 59.7 GB depending on the model size.

## Limitations

The architecture assumes uniformly gridded data.

The study is limited to 2D systems.