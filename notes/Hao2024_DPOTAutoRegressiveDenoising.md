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
This paper introduces an auto-regressive denoising pretraining strategy for improving neural operators in data-scarce settings, particularly for partial differential equations (PDEs). The authors propose a Fourier attention-based model architecture to scale the model efficiently and train it on over 100k trajectories from more than 10 datasets. The goal is to improve the performance and generalization ability of PDE models by using pre-training.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->
DPOT addresses **regression** problems through **fine-tuning** and operates within a **multi-physics** context (different families of PDEs).

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->
#### Encoding
The model accepts previous solutions at multiple timesteps (0, 1, ..., T-1), which have a fixed resolution and number of channels (via padding along the channel dimension). Irregular grids are supported by adding a mask channel. The encoding includes a patchification layer, where each patch corresponds to a solution at a timestep, followed by a convolutional layer and position encoding.

#### Backbone
The model uses a Fourier attention mechanism, which is multi-headed, followed by a fully connected layer. Several blocks of this backbone are followed by an output projection layer.

#### Training Strategy
The model is trained using a noise injection method, which reduces the gap between training and inference. For efficient training, data is sampled by balancing the probability across datasets to address training inefficiencies due to dataset disparity.

## Experiments

### Data
The study uses 12 datasets from four sources: FNO, PDEBench, PDEArena, and CFDBench. These datasets contain a variety of PDE types, including the Navier-Stokes equations, diffusion-reaction equations, and shallow-water equations.

### Results
The pre-trained DPOT model shows a significant improvement in performance compared to non-pre-trained models, with state-of-the-art results on 9 out of 12 datasets. The results show an average improvement of up to 52% on FNO1e-4, highlighting the effectiveness of the pre-training strategy. Additionally, larger models scale better and outperform smaller models, achieving better zero-shot performance and fine-tuning results.

##### Zero-shot Results
The largest DPOT model (0.5B parameters) outperforms other models on 10 datasets, with an improvement of up to 39% on specific subsets. The results suggest that larger models can leverage more knowledge from the pre-training stage, improving their generalization ability.

##### Fine-tuning Results
Fine-tuning the pre-trained DPOT models for 200 and 500 epochs results in state-of-the-art performance on 9 out of 12 tasks. Larger models achieve better fine-tuning results, with more than 50% improvement in L2RE on 8 out of 12 tasks.

##### Scaling Experiments
The performance of the model improves as the size of the model increases. The scaling experiments demonstrate that the DPOT model scales more efficiently than traditional vision-based transformers.

## Limitations
