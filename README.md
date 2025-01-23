# spatiotemp_bib
Literature resources for spatiotemp

## Foundation Models

- [Poseidon: Efficient Foundation Models for PDEs](/notes/Herde2024_PoseidonEfficientFoundation.md) - [arxiv.org/abs/2405.19101](http://arxiv.org/abs/2405.19101)
- [Universal Physics Transformers: A Framework For Efficiently Scaling Neural Operators](/notes/Alkin2024_UniversalPhysicsTransformers.md) - [arxiv.org/abs/2402.12365](http://arxiv.org/abs/2402.12365)
- [UPS: Efficiently Building Foundation Models for PDE Solving via Cross-Modal Adaptation](/notes/Shen2024_UPSEfficientlyBuilding.md) - [arxiv.org/abs/2403.07187](http://arxiv.org/abs/2403.07187)
- [DPOT: Auto-Regressive Denoising Operator Transformer for Large-Scale PDE Pre-Training](/notes/Hao2024_DPOTAutoRegressiveDenoising.md) - [arxiv.org/abs/2403.03542v4](https://arxiv.org/abs/2403.03542v4)
- [Multiple Physics Pretraining for Physical Surrogate Models](/notes/McCabe2023_MultiplePhysicsPretraining.md) - [arxiv.org/abs/2310.02994](http://arxiv.org/abs/2310.02994)
- [Towards Foundation Models for Scientific Machine Learning: Characterizing Scaling and Transfer Behavior](/notes/Subramanian2023_FoundationModelsScientific.md) - [arxiv.org/abs/2306.00258](http://arxiv.org/abs/2306.00258)

### Comparison

| paper    | problem    | OOD                   | range         | dims     | geometry  | encoding                                                                                                    | backbone                              | data                                                                 | beats          |
| -------- | ---------- | --------------------- | ------------- | -------- | --------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------- | -------------- |
| Poseidon | regression | finetuning            | multi-physics | 2D       | regular   | patched, embedded                                                                                           | ViT (SwinV2) in a UNet shape          | Euler, Navier-Stokes                                                 | MPP            |
| UPT      | regression | none                  | none          | 2D (3D?) | irregular | embedding, message passing, transformer, perceiver                                                          | transformer                           | ShapeNet-Car, Navier-Stokes, TGV3D (Lagrangian formulation)          | FNO            |
| UPS      | regression | zero-shot, finetuning | multi-physics | 1D, 2D   | regular   | projection into a superspace through zero-padding, FNO, tokenized along with PDE description text tokenized | pre-trained LLM                       | Burgers, Advection, Diffusion-Sportion, Shallow-Water, Navier-Stokes | DPOT, MPP, FNO |
| DPOT     | regression | finetuning            | multi-physics | 2D       | irregular | resolutions unified through interpolation, mask for irregular grids, patched, temporal aggregation          | FNO (on steroids)                     | PDEBench, PDEArena , CFDBench                                        | MPP, FNO       |
| MPP      | regression | zero-shot, finetuning | multi-physics | 2D       | regular   | ReVIN, shared embedding space for each field, patched                                                       | ViT (axial attention)                 | PDEBench                                                             |                |
| FNO      | regression | finetuning            | parametric    | 2D       | regular   | none                                                                                                        | FNO (with per-instance normalization) | Poisson, Advection-Diffusion, Helmoltz                               |                |

## Team

- [GEPS: Boosting Generalization in Parametric PDE Neural Solvers through Adaptive Conditioning](/notes/Koupai2024_GEPSBoostingGeneralization.md)
- [Zebra: In-Context and Generative Pretraining for Solving Parametric PDEs](/notes/Serrano2024_ZebraInContextGenerative.md)
- [AROMA: Preserving Spatial Structure for Latent PDE Modeling with Local Neural Fields](/notes/Serrano2024_AROMAPreservingSpatial.md)