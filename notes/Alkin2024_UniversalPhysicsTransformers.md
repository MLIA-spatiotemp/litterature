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

Introducing an efficient and unified learning paradigm for a wide range of spatio-temporal problems, which doesn't depend on grid- or particle-based latent structures.


## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

The problem is formulated as a **regression** problem.

The model is **not properly evaluated on unseen parameters or physics**, it is always **trained from scratch**.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input

The input data consists of the initial value $u_0$ at certain input positions $\{x^i\}_i$ which can follow a regular grid or an irregular mesh.

#### Encoding

Implements a complex pipeline which consists of :
1. **evaluation** of $u^t$ at arbitrary points (can follow a regular greed or a random sample to follow the input geometry) ;
2. **embedding + positional encoding** of the sampled points ;
3. **message passing** to a smaller number $n_s$ of supernodes (uniformly defined over the sampled points) ;
4. **transformer** blocks on the obtained sequence ;
5. **perceiver** blocks to reduce the number of blocks to $n_\text{latent}$ ;

#### Approximator

A transformer is employed to propagate the compressed representation $z^t$ forward in time $z^{t+\delta t}$.
This process can be repeated multiple times to predict a longer time ahead $z^{t + \Delta t}$.

#### Decoder

The decoder, given an arbitrary query location $x$, returns the prediction of the model at this location $\hat u^{t+\Delta t}(x)$ from the latent representation $z^{t+\Delta t}$ via a perceiver, using a posiion embedding of the location $x$ as query and the latent representation $z^{t+\Delta t}$ as keys and values.

#### Separated Training

The responsability of the encoder-decoder pair is isolated during training with reconstruction losses on the encoding-decoding and decoding-encoding processes, while the approximator is trained through a reconstruction loss on the encoding-approximating-decoding process.

## Experiments

### Steady state flows

#### Data

The model is evaluated on the **ShapeNet-Car dataset** which consists of 889 car shapes, where each car surface is represented by 3.6K mesh points in 3D space. The pressure is regressed at each surface point with MSE.

#### Results

UPT achieves a favorable cost-vs-performance tradeoff where the best GINO models requires 900 seconds per epoch whereas a only slightly worse (-0.17) UPT model takes only 4 seconds.

### Transient flows

#### Data

**10K Navier-Stokes simulations** were generated within a pipe flow using the pisoFoam solver from OpenFOAM. For each simulation, between one and four objects (circles of variable size) are placed randomly within the pipe flow, and the uni-directional inflow velocity varies between 0.01 to 0.06 m/s.

The simulation is carried out for 100s resulting in 100 timesteps, and an adaptive meshing algorithm is used to produce between 29K and 59K mesh points.

#### Results

UPTs outperform compared methods by a large margin. It generalizes across a wide range of different number of input or output positions, with even slight performance increases when using more input points.

The latent rollout achieves on par results to autoregressively unrolling via the physics domain, but speeds up the inference significantly.

### Lagrangian fluid dynamics

#### Data

The capacity of the model to be reframed for Lagrangian dynamics is explored n the Taylor-Green vortex dataset in three dimensions (**TGV3D**), where the inputs are the two consecutive velocities of the particles in the dataset at a given timestep $\{t − 1 , t\}$, and the respective particle positions, and the outputs are the two consecutive velocities at a later timesteps $\{t + \Delta T − 1, t + \Delta T \}$.

#### Results

The results demonstrate that UPTs effectively learn the underlying field dynamics.

## Limitations

Experiments are limited to fluid equations.

No proper evaluation of the model's capacity to generalize to unseen parameters (except for the inflow velocity) or learn multiple physics. 