- date: 2024-11-05
- url: [http://arxiv.org/abs/2405.19101](http://arxiv.org/abs/2405.19101)
- DOI: [10.48550/arXiv.2405.19101](https://doi.org/10.48550/arXiv.2405.19101)
- citekey: Herde2024_PoseidonEfficientFoundation
---

# Poseidon: Efficient Foundation Models for PDEs

#### Maximilian Herde, Bogdan Raonić, Tobias Rohner, Roger Käppeli, Roberto Molinaro, Emmanuel de Bézenac, Siddhartha Mishra

### Abstract

We introduce Poseidon, a foundation model for learning the solution operators of PDEs. It is based on a multiscale operator transformer, with time-conditioned layer norms that enable continuous-in-time evaluations. A novel training strategy leveraging the semi-group property of time-dependent PDEs to allow for significant scaling-up of the training data is also proposed. Poseidon is pretrained on a diverse, large scale dataset for the governing equations of fluid dynamics. It is then evaluated on a suite of 15 challenging downstream tasks that include a wide variety of PDE types and operators. We show that Poseidon exhibits excellent performance across the board by outperforming baselines significantly, both in terms of sample efficiency and accuracy. Poseidon also generalizes very well to new physics that is not seen during pretraining. Moreover, Poseidon scales with respect to model and data size, both for pretraining and for downstream tasks. Taken together, our results showcase the surprising ability of Poseidon to learn effective representations from a very small set of PDEs during pretraining in order to generalize well to unseen and unrelated PDEs downstream, demonstrating its potential as an effective, general purpose PDE foundation model. Finally, the Poseidon model as well as underlying pretraining and downstream datasets are open sourced, with code being available at https://github.com/camlab-ethz/poseidon and pretrained models and datasets at https://huggingface.co/camlab-ethz.

---

## Ojective
The goal of this paper is to introduce **POSEIDON**, a foundation model for learning solution operators of Partial Differential Equations (PDEs). It aims to address how the number of training samples can be significantly reduced by using a pre-trained foundation model, which can generalize to a wide variety of PDEs.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->
POSEIDON addresses **regression** problems through **fine-tuning** and operates within a **multi-physics** context (different families of PDEs).

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->
### Encoding
- **Input:** The input consists of initial conditions with dimensions \( a \in \mathbb{R}^{128 \times 128 \times n} \), where both the resolution and the number of channels are fixed through preprocessing. The channels are set to the maximum across all training dataset solutions, and if a solution has fewer channels, extra channels are padded with zeros. The resolution is rescaled via interpolation.
- **Patching:** The input is divided into spatial patches \( a_p \in \mathbb{R}^{p \times p \times n} \).
- **Embedding:** Each patch is linearly embedded to a vector \( v \in \mathbb{R}^{p \times p \times C} \).

### Backbone
POSEIDON uses **SwinV2 transformer blocks** as the backbone, implementing a shifted-window multi-head self-attention mechanism. It incorporates logarithmically-generated positional encoding and time-dependent normalization. The architecture follows a **UNet form**, with blocks that reduce resolution while doubling the number of channels (downward blocks) and vice versa (upward blocks). **ConvNext blocks** are used at corresponding levels of the up and down blocks.

### Decoding
The final features are decoded to recover the output dimensions, and the results are mixed using a convolution operation.

### Foundation Model Training
The model is pre-trained on diverse PDE operators. During pretraining, a uniform data structure is used for all inputs, even if the underlying problems differ. The objective is to predict the solutions of the PDEs by minimizing errors across all tasks. The pretrained model learns generalized skills for solving a wide range of PDEs, which can be adapted for further fine-tuning or deployment on new problems.

### Fine-tuning
The parameters are divided into three groups:
1. A large set inherited from the pretrained model.
2. A smaller, task-specific set initialized randomly.
3. A set tied to time embeddings, also inherited from the pretrained model.

For tasks differing significantly from the pretraining data, the task-specific parameters are trained from scratch, while inherited parameters are fine-tuned with small adjustments. When the task is similar to the pretraining data, all parameters are initialized from the pretrained model and adjusted accordingly. Higher learning rates are used for the task-specific and time-related parameters to enable quicker adaptation.

## Experiments

### Data
POSEIDON is pretrained on a custom dataset containing 6 operators defined in the space-time domain \( [0, 1]^2 \times [0, 1] \). These operators represent PDEs from fluid dynamics, specifically the **compressible Euler equations** and **incompressible Navier-Stokes equations**. The dataset includes approximately 5.11 million training examples derived from 77,840 trajectories sampled at 11 time snapshots.

### Results
POSEIDON is evaluated on 15 challenging downstream tasks covering a variety of PDE types and operators. These tasks are out-of-distribution compared to the pretraining data, with many introducing new physical processes or completely new PDEs, such as wave equations and time-independent PDEs.
POSEIDON demonstrates excellent performance across the 15 downstream tasks (best-performing model on 14 out of 15 tasks), outperforming other models in both **accuracy** and **sample efficiency**. It requires far fewer task-specific samples to achieve the same accuracy as traditional models and generalizes well to **unseen PDEs**, performing strongly on PDEs not included in the pretraining dataset. Its performance scales effectively with both **model size** and **dataset size**.

## Limitations
- The range of PDEs and underlying data distributions is vast, yet POSEIDON was trained and evaluated on a limited subset. Incorporating time-independent PDEs, such as elliptic PDEs, and a broader range of time scales for time-dependent PDEs could significantly enhance its capabilities.
- Although focused on Cartesian geometries, POSEIDON demonstrated promise in generalizing to non-Cartesian geometries via masking. Extending POSEIDON to downstream tasks like uncertainty quantification, inverse problems, and PDE-constrained optimization is a natural next step for future research.
