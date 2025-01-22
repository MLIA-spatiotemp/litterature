- date: 2024-11-08
- url: [http://arxiv.org/abs/2410.23889](http://arxiv.org/abs/2410.23889)
- DOI: [10.48550/arXiv.2410.23889](https://doi.org/10.48550/arXiv.2410.23889)
- citekey: Koupai2024_GEPSBoostingGeneralization
---

# GEPS: Boosting Generalization in Parametric PDE Neural Solvers through Adaptive Conditioning

## Armand Kassaï Koupaï, Jorge Mifsut Benet, Yuan Yin, Jean-Noël Vittaut, Patrick Gallinari

### Abstract

Solving parametric partial differential equations (PDEs) presents significant challenges for data-driven methods due to the sensitivity of spatio-temporal dynamics to variations in PDE parameters. Machine learning approaches often struggle to capture this variability. To address this, data-driven approaches learn parametric PDEs by sampling a very large variety of trajectories with varying PDE parameters. We first show that incorporating conditioning mechanisms for learning parametric PDEs is essential and that among them, $\textit{adaptive conditioning}$, allows stronger generalization. As existing adaptive conditioning methods do not scale well with respect to the number of parameters to adapt in the neural solver, we propose GEPS, a simple adaptation mechanism to boost GEneralization in Pde Solvers via a first-order optimization and low-rank rapid adaptation of a small set of context parameters. We demonstrate the versatility of our approach for both fully data-driven and for physics-aware neural solvers. Validation performed on a whole range of spatio-temporal forecasting problems demonstrates excellent performance for generalizing to unseen conditions including initial conditions, PDE coefficients, forcing terms and solution domain. $\textit{Project page}$: https://geps-project.github.io

## Ojective

Designing a low-rank adaptation mechanism for boosting generalization of Parametric PDE neural solvers. 

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

Tackles the problem of **adaptive learning** to unseen parameters of PDEs in a **parametric** setup. Is agnostic to the kind of task performed by the network.

## Methodology

The method is model agnostic. It proposes a framework for performing a low-rank adaptation to unseen environment by generating a code $\mathbf c^e \in \mathbb R^r$ meant to encapsulate the environment information.

#### Joint Training

At training, the model's parameters $\theta^s$, which are shared across environment, are jointly trained with the environment specific codes $\mathbf c^e$ on every training environment.

For a new, unseen environment, only the context specific codes are learned.

#### Adaptation Rule

For each layer $L_i$ of the network (parameterized by a weight matrix $\mathbf W_{L_i} \in \mathcal R^{d_\text{in}\times d_\text{out}}$) adaptation is performed through a low-rank matrix : $\mathbf W^e_{L_i} = \mathbf W_{L_i} + \mathbf A_{L_i} \text{diag}(\mathbf c^e)\mathbf B_{L_i}$, where $\mathbf A_{L_i} \in \mathbb R^{d_\text{in}\times r}$, $\mathbf B_{L_i} \in \mathbb R^{r\times d_\text{out}}$ are shared parameter matrices across all environments.

## Experiments

### Data

Experiments were performed on four dynamical systems. $d_p$ represents the degrees of variations used to define an environment for each equation.
- the motion of a **pendulum** (ODE - 1D), which can be subject to a driving or damping term - $d_p = 4$ ;
- a **Large Eddy Simulation** version of the **Burgers** equation (1D) - $d_p = 3$ ;
- **Gray-Scott**, a reaction-diffusion system with complex spatio-temporal patterns (2D) - $d_p = 2$ ;
- a **Large Eddy Simulation** version of the **Kolmogorov flow**, a 2D turbulence equation for incompressible flows - $d_p = 3$ ;

For evalutation, out of distribution is defined by changes in system parameters, forcing terms or domain definition, along the $d_p$ degrees of variation.

### Results

GEPS performs better than all baseline on elmost every problem, even when scaling to larger problems.

The baselines being CAVIA and FOCA (gradient-based meta-learning methods), LEADS (multi-task learning method for dynamical systems), and CoDA (a hyper-network-based metalearning method which is currently SOTA among the adaptation methods).

## Limitations

These findings are still to be confirmed for more complex dynamics and real world conditions for which the variety of behaviors should be much larger than for the simple dynamics we experimented with.