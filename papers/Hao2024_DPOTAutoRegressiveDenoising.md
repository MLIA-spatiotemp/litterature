- date: 2024/03/06
- url: [https://arxiv.org/abs/2403.03542v4](https://arxiv.org/abs/2403.03542v4)
- DOI: undefined
- citekey: Hao2024_DPOTAutoRegressiveDenoising
---

# DPOT: Auto-Regressive Denoising Operator Transformer for Large-Scale PDE Pre-Training

#### Zhongkai Hao, Chang Su, Songming Liu, Julius Berner, Chengyang Ying, Hang Su, Anima Anandkumar, Jian Song, Jun Zhu

### Abstract

Pre-training has been investigated to improve the efficiency and performance of training neural operators in data-scarce settings. However, it is largely in its infancy due to the inherent complexity and diversity, such as long trajectories, multiple scales and varying dimensions of partial differential equations (PDEs) data. In this paper, we present a new auto-regressive denoising pre-training strategy, which allows for more stable and efficient pre-training on PDE data and generalizes to various downstream tasks. Moreover, by designing a flexible and scalable model architecture based on Fourier attention, we can easily scale up the model for large-scale pre-training. We train our PDE foundation model with up to 0.5B parameters on 10+ PDE datasets with more than 100k trajectories. Extensive experiments show that we achieve SOTA on these benchmarks and validate the strong generalizability of our model to significantly enhance performance on diverse downstream PDE tasks like 3D data. Code is available at \url{https://github.com/thu-ml/DPOT}.

---

## Ojective

Designing a unified Neural Operator that can work on different phyics for temporal evolution prediction of the system.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

DPOT addresses **regression** problems through **fine-tuning** and operates within a **multi-physics** context.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input and Preprocessing

The input consists the past solution $u$ up to $u_t$ at time $t$.

All resolutions are unified to $H=128$ (lower resolutions being upscaled through interpolation, and higher resolutions being downscaled through random sampling or interpolation) ; irregular grids are supported by adding a mask channel.

Different number of chanels are supported through 1-padding to match the dataset with the maximum number of channels.

#### Encoding

First the input is divied into **patches** using a convolution layer and position-encoded.

A novel **temporal aggregation** layer is then applied on each patch spacial location :

$$
z_\text{agg} = \sum_t W_t z^t_p e^{-i\gamma t}
$$

#### Backbone

The model uses a Fourier attention mechanism, which is a **multi-headed FNO with a two layers MLP in the Fourrier space** (instead of a single projection as in the original FNO), followed by a fully connected layer. Several blocks of this backbone are followed by an output projection layer.

#### Training Strategy

The model is trained using a noise injection method, which reduces the gap between training and inference. For efficient training, data is sampled by balancing the probability across datasets to address training inefficiencies due to dataset disparity.

## Experiments

### Data

The study uses 12 datasets from four sources:
- FNO ;
- PDEBench ;
- PDEArena ;
- CFDBench ;

These datasets contain a variety of PDE types, including the Navier-Stokes equations, diffusion-reaction equations, and shallow-water equations.

### Results

The pre-trained DPOT model shows a significant improvement in performance compared to non-pre-trained models, with **state-of-the-art results on 9 out of 12 datasets**. The results show an average improvement of up to 52% on FNO.

Larger models scale better and outperform smaller models, achieving better zero-shot performance and fine-tuning results.

##### Zero-shot Results

The largest DPOT model (0.5B parameters) outperforms other models on 10 datasets ; it is notably better then MPP-L.

The results suggest that larger models can leverage more knowledge from the pre-training stage, improving their generalization ability.

##### Fine-tuning Results

Fine-tuning the pre-trained DPOT models for 200 and 500 epochs results in state-of-the-art performance on 9 out of 12 tasks. Larger models achieve better fine-tuning results, with more than 50% improvement in L2RE on 8 out of 12 tasks.

##### Scaling Experiments

The performance of the model improves as the size of the model increases. The scaling experiments demonstrate that the DPOT model scales more efficiently than traditional vision-based transformers.

## Limitations

The pre-processing only uses padding to adapt irregular geometries and missing chanels ; expects a resolution close to 128.

The study is limited to 2D systems.