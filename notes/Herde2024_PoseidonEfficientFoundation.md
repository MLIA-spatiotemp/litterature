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

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

## Experiments

### Data

### Results

## Limitations

---

Introduces a new foundation model for physics architecture based on massive Swin Transformers arranged in a U-net manner with ConvNeXt Blocks for skip connections, and a time-conditioned normalization layer.

The model takes as input the initial map <span class="math">$u(0) = a$</span> as well as an arbitrary time <span class="math">$t$</span> and outputs an approximation of the solution <span class="math">$u^*(t)$</span> at this time <span class="math">$t$</span>.

For a sample <span class="math">$u(t) \forall t \in [0, T]$</span> the model not only trains to predict <span class="math">$u^*(T)$</span> from <span class="math">$u(0)$</span> but any <span class="math">$u^*(t_f) \forall t_f \in ]0, T]$</span> from any <span class="math">$u(t_0) \forall t_0 \in [0, t_f[$</span>.

After pretraining, the model’s parameters are separated into different groups with different initializations and learning rates for finetuning, as different parts are judged more problem specific than others.

The model obtains state of the art results and shows great transferability capacity to unseen PDEs as well as unseen tasks (different than future prediction like mapping the domain shape to the solution).