- date: 2024-10-10
- url: [http://arxiv.org/abs/2402.12365](http://arxiv.org/abs/2402.12365)
- DOI: [10.48550/arXiv.2402.12365](https://doi.org/10.48550/arXiv.2402.12365)
- citekey: Alkin2024_UniversalPhysicsTransformers
---

# Universal Physics Transformers: A Framework For Efficiently Scaling Neural Operators

#### Benedikt Alkin, Andreas Fürst, Simon Schmid, Lukas Gruber, Markus Holzleitner, Johannes Brandstetter

### Abstract

Neural operators, serving as physics surrogate models, have recently gained increased interest. With ever increasing problem complexity, the natural question arises: what is an efficient way to scale neural operators to larger and more complex simulations - most importantly by taking into account different types of simulation datasets. This is of special interest since, akin to their numerical counterparts, different techniques are used across applications, even if the underlying dynamics of the systems are similar. Whereas the flexibility of transformers has enabled unified architectures across domains, neural operators mostly follow a problem specific design, where GNNs are commonly used for Lagrangian simulations and grid-based models predominate Eulerian simulations. We introduce Universal Physics Transformers (UPTs), an efficient and unified learning paradigm for a wide range of spatio-temporal problems. UPTs operate without grid- or particle-based latent structures, enabling flexibility and scalability across meshes and particles. UPTs efficiently propagate dynamics in the latent space, emphasized by inverse encoding and decoding techniques. Finally, UPTs allow for queries of the latent space representation at any point in space-time. We demonstrate diverse applicability and efficacy of UPTs in mesh-based fluid simulations, and steady-state Reynolds averaged Navier-Stokes simulations, and Lagrangian-based dynamics.

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

Introduces an efficient and unified learning paradigm for a wide range of spatio-temporal problems.

Defines a model as three main blocks :

* an encoder <span class="math">$\mathcal E$</span> consisting of a message passing to supernodes of the entry points or grid, followed by a transformer on this sequence of supernodes and finally a perceiver to generate a fixed-legth sequence of tokens
* an approximator <span class="math">$\mathcal A$</span> which evolves this latent state over time using a transformer
* a decoder <span class="math">$\mathcal D$</span> which returns the predictions at <span class="math">$t + \Delta T$</span> at queried locations using a perceiver

The encoder / decoder and approximator are separately trained.

Experiments limited to fluid equations but extended to Lagrangian formalism (with a separate training, no transfer learning for this).