- date: 2024-07-31
- url: [http://arxiv.org/abs/2403.07187](http://arxiv.org/abs/2403.07187)
- DOI: [10.48550/arXiv.2403.07187](https://doi.org/10.48550/arXiv.2403.07187)
- citekey: Shen2024_UPSEfficientlyBuilding
---

# UPS: Efficiently Building Foundation Models for PDE Solving via Cross-Modal Adaptation

#### Junhong Shen, Tanya Marwah, Ameet Talwalkar

### Abstract

We present Unified PDE Solvers (UPS), a data- and compute-efficient approach to developing unified neural operators for diverse families of spatiotemporal PDEs from various domains, dimensions, and resolutions. UPS embeds different PDEs into a shared representation space and processes them using a FNO-transformer architecture. Rather than training the network from scratch, which is data-demanding and computationally expensive, we warm-start the transformer from pretrained LLMs and perform explicit alignment to reduce the modality gap while improving data and compute efficiency. The cross-modal UPS achieves state-of-the-art results on a wide range of 1D and 2D PDE families from PDEBench, outperforming existing unified models using 4 times less data and 26 times less compute. Meanwhile, it is capable of few-shot transfer to unseen PDE families and coefficients.

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

Proposes a novel method to adapt pretrained LLMs to PDE solving.

Does so by first projecting the input to a common superspace that unifies all dimensions and pysical quantities, passing it through an FNO with <span class="math">$l$</span> channels to obtain <span class="math">$l$</span> tokens that are fed to an LLM along with the PDE information as a text (`[PDE family][coefficients]`), with a final projection to generate the prediction.

The training is carefully designed as a two-stage align-then-refine process to first minimize the distance between the tokens distributions.

Shows transferability to unseen coefficients and PDEs with less computation because of the use of a pretrained language model.

Also shows that the pretraining of the Transformer on language is necessary, but it has to be noted that the model has to understand the textual information on the PDE in order to understand the dynamics, as it only has access to one previous timestep <span class="math">$t$</span>.