- date: 2023-06-01
- url: [http://arxiv.org/abs/2306.00258](http://arxiv.org/abs/2306.00258)
- DOI: [10.48550/arXiv.2306.00258](https://doi.org/10.48550/arXiv.2306.00258)
- citekey: Subramanian2023_FoundationModelsScientific
---

# Towards Foundation Models for Scientific Machine Learning: Characterizing Scaling and Transfer Behavior

#### Shashank Subramanian, Peter Harrington, Kurt Keutzer, Wahid Bhimji, Dmitriy Morozov, Michael Mahoney, Amir Gholami

### Abstract

Pre-trained machine learning (ML) models have shown great performance for a wide range of applications, in particular in natural language processing (NLP) and computer vision (CV). Here, we study how pre-training could be used for scientific machine learning (SciML) applications, specifically in the context of transfer learning. We study the transfer behavior of these models as (i) the pre-trained model size is scaled, (ii) the downstream training dataset size is scaled, (iii) the physics parameters are systematically pushed out of distribution, and (iv) how a single model pre-trained on a mixture of different physics problems can be adapted to various downstream applications. We find that-when fine-tuned appropriately-transfer learning can help reach desired accuracy levels with orders of magnitude fewer downstream examples (across different tasks that can even be out-of-distribution) than training from scratch, with consistent behavior across a wide range of downstream examples. We also find that fine-tuning these models yields more performance gains as model size increases, compared to training from scratch on new downstream tasks. These results hold for a broad range of PDE learning tasks. All in all, our results demonstrate the potential of the "pre-train and fine-tune" paradigm for SciML problems, demonstrating a path towards building SciML foundation models. We open-source our code for reproducibility.

---

## Ojective

**Exploring** several dimensions that include :
- **Q1** : scaling the downstream task **dataset size**;
- **Q2** : scaling the **model's size**;
- **Q3** : transfer learning behaviour over a single learned physic but **varying PDE parameters (OOD)**;
- **Q4** : capacity of a single model to learn **multiple physics simultaneously**;

## Problem
<!-- regression / classification / génération ? -->
<!-- finetuning / adaptive learning ? -->
<!-- parametric / multiphysics ? -->

The problem is formulated as a **regression** problem.
The model generalizes to unseen distributions by **finetuning**.
The authors mainly tackle the problem of **parametric** for unseen distributions, the multiphysics paradigm is only tackled when studying the properties of a single model to learn multiple physics while staying in-distribution, not studying its transfer properties to unseen physics.  

## Methodology
<!-- accent on encoding -->
<!-- transformer ? -->

Uses **FNO** as model.
**No proper encoding** is done, the input of the model is simply a discretized $h \times w$ map of the source and PDE coefficients, constant coefficients being simply replicated on this grid ; though a **per-instance normalization layer** is incorporated in the model to take into account the variability in the norm of the input data.
The study isn't extended to ViT models.

## Experiments

### Data

For each problem, $2^{15}$ input-output samples (pairs) of training data, along with $2^{12}$ samples of validation and testing data are generated, depending one the problem, on one or multiple of the 3 following equations, all of which are on the domain $[0,1]^2$ :
- **SYS-1** - *Poisson* : $-\text{div} K\nabla u = f$
- **SYS-2** - *Advection-Diffusion* : $-\text{div} K\nabla u + v\nabla u = f$
- **SYS-3** - *Helmholtz* : $-\Delta u + wu = f$

### Results

- **Q1** :
    Experiment where the model is pre-trained on *SYS-1(1, 5)* (the eigenvalue of $K$ in $[1, 5]$) and finetuned with a moderate OOD on *SYS-1(5, 10)*.
    
    → A desired error of 1e-2 needs only about **64 downstream data examples** for fine-tuning, whereas training from scratch requires 8K examples to reach the same accuracy level.
- **Q2** :
    The model's size is approximately increased 16 folds from 64K to 256M parameters, on the same setup as **Q1** as well as on SYS-2 (pre-training on *SYS-2(0.2,1)*, finetuned on *SYS-2(1,2)*).

    → When moving to larger models, both training from scratch and fine-tuning show higher performance that increase with more examples. Fine-tuning the pre-trained model boosts its performance, compared to training from scratch.
- **Q3** :
    Multiple setups :
    - pre-training *SYS-1(1, 2.5)*, finetuning *SYS-1(1, 5)*
    - pre-training *SYS-1(1, 5)*, finetuning *SYS-1(2.5, 7.5)*
    - pre-training *SYS-1(1, 5)*, finetuning *SYS-1(5, 10)*
    - pre-training *SYS-1(1, 5)*, finetuning *SYS-1(10, 20)*

    → The performance is improved with few-shot TL up to the point of diminishing returns with large numbers of downstream data examples. For large distributional shifts TL shows relatively high errors, still improves compared to from scratch but doesn't get as much as an advantage.
- **Q4** :
    Datasets from three PDEs — Poisson’s *SYS1(1,5)*, Advection-Diffusion *SYS-2(0.2,1)*, and Helmholtz *SYS-3(1,10)* — are combined, zero channels being added for coefficients that do not exist when using examples from a specific operator ; this, effectively, serves as selection of the solution operator during the forward pass to predict the solution to the right operator. Evaluated on the same systems, within distribution.

    → The same model (pre-trained on three different tasks) is not only useful in all downstream tasks, in both the zero-shot and the fine-tuning settings, but also shows greater performances than the model trained the single physic of the downstream task.

## Limitations

Doesn't tackle generalization to **unseen physics**.
Doesn't studies scaling properties of **ViT models**.
Is limited to only **3 different physics**. 
