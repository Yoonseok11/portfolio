---
layout: page
title: LLM Priors for Causal Discovery
description: A framework that integrates LLM-based causal reasoning into data-driven causal discovery, resulting in improved and robust performance.
img: assets/img/research/llm-causal-discovery/framework.png
importance: 3
venue: IEEE Access
year: 2025
authors: Chanhui Lee*, Juhyeon Kim*, Yongjun Jeong, Yoonseok Yeom, Juhyun Lyu, Junghee Kim, Sangmin Lee, Sangjun Han, Hyeokjun Choe, Soyeon Park, Woohyung Lim, Sungbin Lim, Sanghack Lee
paper_url: https://ieeexplore.ieee.org/document/11220238/
---

**On Incorporating Prior Knowledge Extracted from Large Language Models into Causal Discovery**
Published in **IEEE Access**

## Abstract

LLMs can reason about causality by leveraging vast pre-trained knowledge and text descriptions of datasets,
**even when data is scarce**. But current LLM-based causal reasoning has two crucial limitations:

1. Prompting is **inherently inefficient for large tabular datasets** once context-length consumption is accounted for.
2. The methods are **not adept at comprehending whole interconnected causal structures** — they reason pairwise.

Data-driven causal discovery has the opposite profile: it discovers the structure **as a whole**, but works well
only when the number of observations is sufficiently large. To overcome the limitations of each approach, we
propose a framework that **integrates LLM-based causal reasoning into data-driven causal discovery**, resulting in
improved and robust performance. The framework further extends to time-series data.

## Contributions

1. We demonstrate that **LLM-based pairwise causal reasoning is not suitable for holistically eliciting a causal
   structure**.
2. We propose a framework that **integrates LLM-based causal reasoning with both static and time-series
   data-driven causal discovery**.
3. The framework not only outperforms baselines but shows **superior robustness in mitigating LLM hallucination**
   over concurrent hybrid frameworks.
4. We propose a **recipe for extending LLM-integrated causal discovery to time-series data** — LLM-based causal
   reasoning on time-series, plus evaluation graph annotation and data generation that require only PDEs
   constructed by domain experts.

## Method

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/llm-causal-discovery/framework.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Framework overview" %}
  </div>
</div>
<div class="caption">
  Given a dataset, LLM-based causal reasoning returns an adjacency matrix as prior. Utilizing the prior, a
  data-driven causal discovery algorithm returns a learned structural coefficient matrix, which is mapped to a
  binary matrix.
</div>

Variable pairs are selected from the tabular dataset and posed to the LLM as multiple-choice prompts; the
individual answers are assembled into a **prior knowledge matrix** $$\mathbf{K}$$. That prior then shapes the
data-driven optimization in three ways:

**1. Graph initialization** — initialize the structural coefficients from the prior:

$$\mathbf{W} = \lambda \cdot \mathbf{K}$$

**2. Regularization** — penalize divergence from the prior:

$$\sum_{i,j} \left\lvert \sigma(\mathbf{W}_{i,j}) - \mathbf{K}_{i,j} \right\rvert$$

**3. Optimization boundaries** — constrain the search space:

$$B_{\text{lower}} \leq \lvert \mathbf{W}_{i,j} \rvert \leq B_{\text{upper}}$$

The paper also analyzes where LLM causal reasoning breaks down: the limits of prompt engineering, the difficulty of
leveraging causal structural information, an ablation of **cumulative versus complete** prompting, and the
challenges of extending LLM reasoning to time-series.

## Experiments

### Static datasets

Two real-world datasets of contrasting size:

- **Arctic Sea Ice** — small scale, 12 Earth-science variables and 486 instances; its causal graph has 48 edges and
  is **not constrained to be acyclic**.
- **Sachs** — large scale, over 7,000 observations.

Arctic Sea Ice challenges causal discovery in two ways: **learning structure from limited samples**, and **learning
cyclic structures**. We conjecture LLMs are less affected — LLM-based reasoning does not require many samples, and
each edge in the annotated graph corresponds to findings reported in published literature, which LLMs are likely to
have seen during training. So when the data alone is insufficient for structural learning, the LLM can still supply
meaningful prior knowledge.

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/llm-causal-discovery/sachs-heatmaps.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Heatmaps on the Sachs dataset" %}
  </div>
</div>
<div class="caption">
  Heatmaps in Sachs dataset by a) GPT-4, b) NOTEARS, c) NOTEARS with GPT-4 prior, d) DAG-GNN, and e) DAG-GNN with
  GPT-4 prior.
</div>

### Mitigating hallucination, keeping explainability

- **GPT-4 alone** often generates false predictions **with full confidence**, reflected in its discrete-valued
  structural coefficients. These hallucinations are hard to audit: LLMs can emit natural-language explanations, but
  those often do not align with the actual reasoning process and may themselves contain hallucinated causal claims.
- **DAG-GNN alone** predicts continuous-valued coefficients — better interpretability and a reduced gap between
  true and false predictions — but its overall performance, especially **TPR**, remains inferior to GPT-4.
- **Our framework** combines both: it **corrects GPT-4's hallucinations** while preserving the explainability of
  data-driven methods through continuous structural coefficient estimation.

### Time-series datasets

Existing time-series benchmarks either lack the real-world semantic interpretability needed for LLM priors, or
provide no ground-truth causal graph. We therefore simulate datasets grounded in well-established PDEs from finance
and epidemiology — **Black-Scholes** and **SEIHR** — with a time lag of 1. Grounding the generation in PDEs with
clear semantics yields both domain-aligned variable relationships and reliable ground-truth structures.

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/llm-causal-discovery/seihr-heatmaps.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Heatmaps on the SEIHR dataset" %}
  </div>
</div>
<div class="caption">
  Heatmaps in SEIHR dataset by a) GPT-4, b) NTS-NOTEARS, c) NTS-NOTEARS with GPT-4 prior, d) DYNOTEARS, and
  e) DYNOTEARS with GPT-4 prior.
</div>

Applicability to high-dimensional real-world data is further demonstrated on the **S&P 100** dataset.

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/llm-causal-discovery/synthetic-metrics.png" class="img-fluid rounded z-depth-1" zoomable=true alt="SHD, FDR and TPR on physical knowledge-based synthetic datasets" %}
  </div>
</div>
<div class="caption">
  SHD, FDR, and TPR of NOTEARS and CGNN on the physical knowledge-based synthetic datasets with and without LLM
  prior.
</div>

## Resources

- **[Paper — IEEE Xplore](https://ieeexplore.ieee.org/document/11220238/)**
