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

Shows that models can learn multiple types of physics, and that it is beneficial to transfer to unseen partially overlapping physics.

It does so with a transformer-based model using axial attention, and an embedding using reversible instance normalization to deal with diverse scales and learned projections for each field. No prior information on the PDE is used by the model.

It was trained on PDEBench, more specifically on the compressible and incompressible Navier-Stokes, shallow-water, and 2D Diffusion-Reaction equations.

Also shows that Video Transformer models can transfer to physics surprisingly well considering they are trained on video, but have a much larger memory use.