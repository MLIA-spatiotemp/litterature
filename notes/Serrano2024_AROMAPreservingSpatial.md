- date: 2024-10-21
- url: [http://arxiv.org/abs/2406.02176](http://arxiv.org/abs/2406.02176)
- DOI: [10.48550/arXiv.2406.02176](https://doi.org/10.48550/arXiv.2406.02176)
- citekey: Serrano2024_AROMAPreservingSpatial
---

# AROMA: Preserving Spatial Structure for Latent PDE Modeling with Local Neural Fields

## Louis Serrano, Thomas X. Wang, Etienne Le Naour, Jean-Noël Vittaut, Patrick Gallinari

### Abstract

We present AROMA (Attentive Reduced Order Model with Attention), a framework designed to enhance the modeling of partial differential equations (PDEs) using local neural fields. Our flexible encoder-decoder architecture can obtain smooth latent representations of spatial physical fields from a variety of data types, including irregular-grid inputs and point clouds. This versatility eliminates the need for patching and allows efficient processing of diverse geometries. The sequential nature of our latent representation can be interpreted spatially and permits the use of a conditional transformer for modeling the temporal dynamics of PDEs. By employing a diffusion-based formulation, we achieve greater stability and enable longer rollouts compared to conventional MSE training. AROMA's superior performance in simulating 1D and 2D equations underscores the efficacy of our approach in capturing complex dynamical behaviors.

## Ojective

Designing an architecture for dealing with arbitrary geometries for PDE temporal evolution modeling. 

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

Formulated as a **regression** problem, not evaluated on different physics or parametetric equations.

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

#### Input

The model takes as input the values $u^t_{\mathcal X}$ of $u$ at time $t$, on a discrete sample space $\mathcal X$ that could be a grid, an irregular mesh or a point set.

#### Encoder

Implements a complex pipeline which consists of :
1. **evaluation** of $u^t$ at arbitrary points $x$ (can follow a regular greed or a random sample to follow the input geometry) - $(x, u^t(x))$ ;
2. **positional encoding** (via fFourrier features) and **value embedding** of the sampled points - $(\gamma(x), v(x))$ ;
3. **geometry encoding** of the positional encodings from $T$ learnable tokens via cross-attention - $\mathbf T^\text{geo}$ ;
4. **observation encoding** of the value features $v(x)$ to the spatial encoding $\mathbf T^\text{geo}$ via cross-attention - $\mathbf T^\text{obs}$ ;
5. **dimension reduction** of $\mathbf T^\text{obs}$ by learning the components of a Gaussian multivariate distribution $(\mu, \sigma)$ from which the final token embedding $\mathbf Z$ is sampled - $\mathbf Z$ ;

#### Refiner

The refiner is implemented using a **transformer** (few layers of self-attention and FFN) ; one pass evolves the state from $t$ to $t+\delta t$. Diffusion steps are also inserted between each latent temporal evolution.

#### Decoder

Decoding is conditionned on a query coordinate $x$ and done through the follwing pipeline :
1. **dimensions upscaling** through projection to a higher chennel's dimension followed by self-attention ;
2. **cross-attention** with the query coordinate $x$ ;
3. **output** through a small MLP ;

### Separated Training

The encoder and decoder are jointly optimized as a VAE through a reconstruction loss and a KL divergence loss on the exctracted parameters $(\mu, \sigma)$ to help regularization of the network.

## Experiments

### Dynamics on regular grids

#### Data

- **1D Burgers’ Equation** :
    Models shock waves, using a dataset with periodic initial conditions and forcing term. It includes 2048 training and 128 test trajectories, at resolutions of $(250, 100)$.
- **2D Navier Stokes Equation** :
    for a viscous and incompressible fluid. They consider two different versions $\nu = 10^{-4}$ and $\nu = 10^{−5}$, and use train and test sets of 1000 and 200 trajectories with a base spatial resolution of size $64\times64$.

#### Results

AROMA demonstrates excellent performance across the board, highlighting its ability to capture the dynamics of turbulent phenomena, and is compared against FNO, DINO, CORAL, OFormer, and GNOT.

### Dynamics on irregular grids with shared geometries

#### Data

- **2D Navier-Stokes Equation** :
    With $\mu = 10^{−3}$. On a standard resolution of $64\times64$.
- **3D Shallow-Water Equation** :
    This equation approximates fluid flow on the Earth’s surface. The data includes the vorticity $w$ and height $h$ of the fluid on a standard spatial resolution of $64\times128$.

#### Results

AROMA consistently achieves low MSE across all levels of observation sparsity and evaluation horizons for both datasets. Overall, this method performs best with some exceptions where it is beaten by CORAL, which is not surprising as meta-learning models excel in data-constrained regimes.
Compared against DINO, CORAL, OFormer, and GNOT.

### Dynamics on different geometries

#### Data

The model is evaluated on two problems involving non-convex domains. Both scenarios involve fluid dynamics in a domain with an obstacle, where the area near the boundary conditions is more finely discretized. The boundary conditions are specified by the mesh, and the models are trained with various obstacles and tested on different, yet similar, obstacles. The two problems are **CylinderFlow** and **AirfoilFlow**.

#### Results

AROMA outperforms other models in predicting flow dynamics on both CylinderFlow and AirfoilFlow geometries, achieving the lowest MSE values across all tests.
Compared against DINO, CORAL, OFormer, and GNOT.

### Long rollouts and uncertainty quantification

The authors show the gain in stability in using the diffusion for long rollouts.

## Limitations

- evaluation is limited to fluid dynamics.
- the model's capacity to generalize of finetune to unseen parameters or physics is not assessed.
- the decoder is still a bottleneck in the prediction's precision.