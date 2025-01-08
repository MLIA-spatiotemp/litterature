- date: 2023-06-01
- url: [http://arxiv.org/abs/2306.00258](http://arxiv.org/abs/2306.00258)
- DOI: [10.48550/arXiv.2306.00258](https://doi.org/10.48550/arXiv.2306.00258)
- citekey: Subramanian2023_FoundationModelsScientific
---

# Towards Foundation Models for Scientific Machine Learning: Characterizing Scaling and Transfer Behavior

## Shashank Subramanian, Peter Harrington, Kurt Keutzer, Wahid Bhimji, Dmitriy Morozov, Michael Mahoney, Amir Gholami

### Abstract

Pre-trained machine learning (ML) models have shown great performance for a wide range of applications, in particular in natural language processing (NLP) and computer vision (CV). Here, we study how pre-training could be used for scientific machine learning (SciML) applications, specifically in the context of transfer learning. We study the transfer behavior of these models as (i) the pre-trained model size is scaled, (ii) the downstream training dataset size is scaled, (iii) the physics parameters are systematically pushed out of distribution, and (iv) how a single model pre-trained on a mixture of different physics problems can be adapted to various downstream applications. We find that-when fine-tuned appropriately-transfer learning can help reach desired accuracy levels with orders of magnitude fewer downstream examples (across different tasks that can even be out-of-distribution) than training from scratch, with consistent behavior across a wide range of downstream examples. We also find that fine-tuning these models yields more performance gains as model size increases, compared to training from scratch on new downstream tasks. These results hold for a broad range of PDE learning tasks. All in all, our results demonstrate the potential of the "pre-train and fine-tune" paradigm for SciML problems, demonstrating a path towards building SciML foundation models. We open-source our code for reproducibility.

---

Studies the transfer learning and scaling properties of a FNO model over the parameters of 3 PDEs : Poisson, Advection-Diffusion, and Helmoltz.

Constitutes a dataset on these three PDEs with various parameters and initial conditions and shows the capacity of a model pre-trained on a large quantity of trajectories to generalize to OOD equation parameters with only a few examples, as well as its capacity to learn multiple PDEs at once.

Adds the parameters of the equation as an input to the model.