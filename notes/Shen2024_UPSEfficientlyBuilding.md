- date: 2024-07-31
- url: [http://arxiv.org/abs/2403.07187](http://arxiv.org/abs/2403.07187)
- DOI: [10.48550/arXiv.2403.07187](https://doi.org/10.48550/arXiv.2403.07187)
- citekey: Shen2024_UPSEfficientlyBuilding
---

# UPS: Efficiently Building Foundation Models for PDE Solving via Cross-Modal Adaptation

#### Junhong Shen, Tanya Marwah, Ameet Talwalkar

### Abstract

We present Unified PDE Solvers (UPS), a data- and compute-efficient approach to developing unified neural operators for diverse families of spatiotemporal PDEs from various domains, dimensions, and resolutions. UPS embeds different PDEs into a shared representation space and processes them using a FNO-transformer architecture. Rather than training the network from scratch, which is data-demanding and computationally expensive, we warm-start the transformer from pretrained LLMs and perform explicit alignment to reduce the modality gap while improving data and compute efficiency. The cross-modal UPS achieves state-of-the-art results on a wide range of 1D and 2D PDE families from PDEBench, outperforming existing unified models using 4 times less data and 26 times less compute. Meanwhile, it is capable of few-shot transfer to unseen PDE families and coefficients.

---

## Ojective

Proposing a novel method to adapt pretrained Large Language Models (LLMs) to PDE solving on varying PDEs.

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

The problem is formulated as a **regression** problem.

The model is evaluated on unseen distributions and unseen physics by **zero-shot** and **finetuning**.

The author tackle both the problems of generalizing to **unseen parameters** on a known equation and generalizing to **unseen physics**.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

The inputs are first projected in a shared superspace $\mathbb R^{N\times n^{d_\text{max}}}$, where $N$ is the size of the superset of the physical quantities, $d_\text{max}$ is the maximum dimension across all PDEs, and $n$ is the resolution.

The model first **encodes** the input $u_t$ (a single frame of a history) using **FNO**, followed by a pointwise convolution to generate **textual tokens**. In parallel, the textual information of the PDE (formatted as `[PDE family][coefficients]`) is also converted to textual tokens using a frozen LLM embedder.

All these tokens are then concatenated and added a positional encoding, to then be fed to a pre-trained **LLM** which constitutes the heart of the network.

Finally, the output sequence is average into a vector of shape $\mathbb R^e$, a a linear layer is applied to map it to $\mathbb R^{N\times n_d}$, and the resulting tensor is reshaped to obtain the final prediction $\hat u_{t+1}(x)$.

## Experiments

### Data

They train and evaluate their method using **PDEBench**. For training, 7 datasets from different PDE families are combined:
- Burgers Equation (1D)
- Advection (1D)
- Diffusion-Sportion (1D)
- Shallow-Water (2D)
- compressible Navier-Stokes (1D and 2D)
- incompressible Navier-Stokes (2D)

1D and 2D Diffusion-Reaction are explicitly hold out to evaluate the generalization ability of UPS.

### Results

**In general**, UPS with RoBERTa **ranks first on all 7 datasets** and improves the state-of-the-art by an order of magnitude on many 1D datasets.

UPS **outperforms MPP and DPOT** when used with an LLM of similar size.

Scaling up the LLM backbone's size yields better results.

On unseen physics **the 100-shot results of UPS on 1D datasets are better** than the baselines trained on 9K examples.

UPS also generalizes to PDEs in the same families as the training data but with **different coefficients**.

## Limitations

The method could be validated on a broader range of PDEs with higher-order temporal derivatives or **3D domains**.

It seems that the model has more easiness with 1D data than 2D data, as if the transformer in the LLM was easier to adapt to 1D *sequences*.

To seek a truly general foundation model for PDE, the **types of tasks** that UPS can solve could be extended. Currently, UPS is **only applicable to forward prediction**.