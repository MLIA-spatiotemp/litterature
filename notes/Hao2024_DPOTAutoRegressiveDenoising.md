- date: 2024/03/06
- url: [https://arxiv.org/abs/2403.03542v4](https://arxiv.org/abs/2403.03542v4)
- DOI: undefined
- citekey: Hao2024_DPOTAutoRegressiveDenoising
---

# DPOT: Auto-Regressive Denoising Operator Transformer for Large-Scale PDE Pre-Training

## Zhongkai Hao, Chang Su, Songming Liu, Julius Berner, Chengyang Ying, Hang Su, Anima Anandkumar, Jian Song, Jun Zhu

### Abstract

Pre-training has been investigated to improve the efficiency and performance of training neural operators in data-scarce settings. However, it is largely in its infancy due to the inherent complexity and diversity, such as long trajectories, multiple scales and varying dimensions of partial differential equations (PDEs) data. In this paper, we present a new auto-regressive denoising pre-training strategy, which allows for more stable and efficient pre-training on PDE data and generalizes to various downstream tasks. Moreover, by designing a flexible and scalable model architecture based on Fourier attention, we can easily scale up the model for large-scale pre-training. We train our PDE foundation model with up to 0.5B parameters on 10+ PDE datasets with more than 100k trajectories. Extensive experiments show that we achieve SOTA on these benchmarks and validate the strong generalizability of our model to significantly enhance performance on diverse downstream PDE tasks like 3D data. Code is available at \url{https://github.com/thu-ml/DPOT}.

---

## Ojective

## Methodology
<!-- particularities - accent on encoding -->

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->

### Paradigm
<!-- parametric / multiphysics ? -->

## Experiments

### Data

### Results

## Limitations

---

Presents a denoising strategy for pre-training PDE foundation models. Uses a “Fourier Attention Layer” (even though there is no proper attention mechanism) which is basically FNO with a two-layers neural network in Fourier space instead of only a linear projection.

Trained on PDEBench, PDEArena and CFDBench ; it doesn’t inject prior PDE information in the model and shows transferability to unseen PDEs.