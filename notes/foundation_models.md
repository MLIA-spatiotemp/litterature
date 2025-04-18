# Foundation Models

- [BCAT: A Block Causal Transformer for PDE Foundation Models for Fluid Dynamics](/papers/Liu2025_BCATBlockCausal.md) - [arxiv.org/abs/2501.18972](http://arxiv.org/abs/2501.18972)
- [Masked Autoencoders are PDE Learners](/papers/Zhou2024_MaskedAutoencodersAre.md) - [arxiv.org/abs/2403.17728](http://arxiv.org/abs/2403.17728)
- [Pretraining Codomain Attention Neural Operators for Solving Multiphysics PDEs](/papers/Rahman2024_PretrainingCodomainAttention.md) - [arxiv.org/abs/2403.12553](http://arxiv.org/abs/2403.12553)
- [In-Context Operator Learning with Data Prompts for Differential Equation Problems](/papers/Yang2023_InContextOperatorLearning.md) - [arxiv.org/abs/2304.07993](http://arxiv.org/abs/2304.07993)
- [Towards a Foundation Model for Partial Differential Equations: Multi-Operator Learning and Extrapolation](/papers/Sun2025_FoundationModelPartial.md) - [arxiv.org/abs/2404.12355](http://arxiv.org/abs/2404.12355)
- [Poseidon: Efficient Foundation Models for PDEs](/papers/Herde2024_PoseidonEfficientFoundation.md) - [arxiv.org/abs/2405.19101](http://arxiv.org/abs/2405.19101)
- [Universal Physics Transformers: A Framework For Efficiently Scaling Neural Operators](/papers/Alkin2024_UniversalPhysicsTransformers.md) - [arxiv.org/abs/2402.12365](http://arxiv.org/abs/2402.12365)
- [UPS: Efficiently Building Foundation Models for PDE Solving via Cross-Modal Adaptation](/papers/Shen2024_UPSEfficientlyBuilding.md) - [arxiv.org/abs/2403.07187](http://arxiv.org/abs/2403.07187)
- [DPOT: Auto-Regressive Denoising Operator Transformer for Large-Scale PDE Pre-Training](/papers/Hao2024_DPOTAutoRegressiveDenoising.md) - [arxiv.org/abs/2403.03542v4](https://arxiv.org/abs/2403.03542v4)
- [Multiple Physics Pretraining for Physical Surrogate Models](/papers/McCabe2023_MultiplePhysicsPretraining.md) - [arxiv.org/abs/2310.02994](http://arxiv.org/abs/2310.02994)
- [Towards Foundation Models for Scientific Machine Learning: Characterizing Scaling and Transfer Behavior](/papers/Subramanian2023_FoundationModelsScientific.md) - [arxiv.org/abs/2306.00258](http://arxiv.org/abs/2306.00258)

## Questions

- what is the purpose of foundation models ?
  - learning primitives ? (spatial / dynamics)
  - modeling different dynamics ?
- what amount of data ?
- generalisation (in/out distribution)
- quality of encodings ?
- adaptation / in context / finetuning

## Synthesis

The notion of Foundation Models started appearing in the context of deep learning for PDE solving in 2023, when the promises of such models for this field were highlighted ; the lightweightness of solving PDEs through neural networks over formal methods without the weight of retraining them on large quantities of data for every new PDE.
A few attempts were made (see [above](#foundation-models)), here we make a review of these.

### Data

All these methods adapt to unseen physics or parameters through finetuning, so the first question that comes to mind is on the quantity of data needed to pre-train the model as well as finetune it.

The amount varies around **$10^4$ trajectories for pre-training and $10^2$ samples for finetuning**. An exception being made by [Herde2024_PoseidonEfficientFoundation](/papers/Herde2024_PoseidonEfficientFoundation.md) which uses **$10^5$ trajectories (which amounts to $10^6$ samples) for pre-training**, and evaluates the generalization capacities of the model **from $1$ to $10^3$ finetuning samples** (good performance starting to be observed around $10^2$).  

### Parameters count

In NLP, Foundation Models' performances tend to scale with the parameter count of the model, a tendancy that was verified in PDE solving by [Subramanian2023_FoundationModelsScientific](/papers/Subramanian2023_FoundationModelsScientific.md) for parameters counts ranging from $64\text K$ to $256\text M$.

Later developped methods increased these counts, testing different model sizes each time ; the smallest models range from $8\text M$ to $20\text M$, while the largest range from $68\text M$ ([Alkin2024_UniversalPhysicsTransformers](/papers/Alkin2024_UniversalPhysicsTransformers.md)) to $1\text B$ ([Hao2024_DPOTAutoRegressiveDenoising](/papers/Hao2024_DPOTAutoRegressiveDenoising.md)) - the others keeping their maximum around $500\text M$.

### Encoding

The architecture of these methods follows (either implicitly or explicitly) an *"Encode - Process - Decode"* approach, where the *Process* evolves a latent lightweight representation of the system at reduced cost from $t$ to $t+\delta t$. The consequence is that the quality of the *Encoder* is crucial as the rest of the chain is built upon it.

Though in this study **most of the methods simply apply a patching + embedding** (for ViT-based approaches [Herde2024_PoseidonEfficientFoundation](/papers/Herde2024_PoseidonEfficientFoundation.md), [McCabe2023_MultiplePhysicsPretraining](/papers/McCabe2023_MultiplePhysicsPretraining.md)) **or FNO** ([Subramanian2023_FoundationModelsScientific](/papers/Subramanian2023_FoundationModelsScientific.md), [Shen2024_UPSEfficientlyBuilding](/papers/Shen2024_UPSEfficientlyBuilding.md)) to the input.

Only two methods stand out with a **temporal aggregation layer** for the former ([Hao2024_DPOTAutoRegressiveDenoising](/papers/Hao2024_DPOTAutoRegressiveDenoising.md), but which cannot handle irregular geometries as it uses FNO) and a **tailor-made process of message-parsing, transformers and perceivers** which can handle irregular meshes for the latter ([Alkin2024_UniversalPhysicsTransformers](/papers/Alkin2024_UniversalPhysicsTransformers.md), but it was not evaluated on different physics).

### What is learned

As for any deep neural network, it is difficult to have an idea of what is learned by a PDE solving foundation model ; wether it learns a way to model different dynamics within the same framework, primitives that can be adapted to new dynamics, or something else. Each author make more or less elaborated guesses.

First, in [Subramanian2023_FoundationModelsScientific](/papers/Subramanian2023_FoundationModelsScientific.md) the authors indicate that "*the input coefficient channels are sufficient to prompt the model to predict the correct downstream solution*", suggesting that **a single network can model different dynamics and choose which one to use** depending on which source coefficients are set to $0$ (each channel corresponds to a physical quantity, these quantities are different in each system). This could also be the case in [McCabe2023_MultiplePhysicsPretraining](/papers/McCabe2023_MultiplePhysicsPretraining.md) where the model could learn to separate each physical quantity in its shared encoded projection.

Nevertheless, the authors of [Hao2024_DPOTAutoRegressiveDenoising](/papers/Hao2024_DPOTAutoRegressiveDenoising.md) observe that "*even though only 2D data is used in pre-training, the learned representations still improve performance on 3D problems*", hinting that the model learned **general primitives** that helped it handling data with a completely different structure. The authors of [Shen2024_UPSEfficientlyBuilding](/papers/Shen2024_UPSEfficientlyBuilding.md) would even argue that **primitves learned for transformer-based LLMs on text** can help improve performance on PDE solving problems.

[Herde2024_PoseidonEfficientFoundation](/papers/Herde2024_PoseidonEfficientFoundation.md) investigate this even further to find that their model still manages to adapt to unseen physics when the *Process* parameters are frozen, only finetuning the *Encode* and *Decode* parameters (which represent only 0.5% of the total paramters). This suggests that the model learned **a universal latent representation as well as a system which can apply a temporal evolution to it** ; and all that needs to be learned for a new system is how to encode it into this universal latent representation.

### Comparison

A more quantitative [comparison](/tables/foundation_models_comparison.csv) of these methods has been made.