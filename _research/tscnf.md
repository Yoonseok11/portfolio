---
layout: page
title: Time-Series Causal Normalizing Flows
description: A framework that extends causal normalizing flows to time-series, enabling simulation-based interventional density estimation over time.
img: assets/img/research/tscnf/architecture.png
importance: 1
venue: UAI 2026
year: 2026
authors: Yoonseok Yeom*, Jonghwan Kim*, Taehui Yun, Juhyun Lyu, Jung-Hee Kim, Sangmin Lee, Jinseok Yang, Hyemin Jung, Woohyung Lim, Sanghack Lee
paper_pending: proceedings pending
code_pending: release planned
---

**Estimating Interventional Outcomes over Time with Causal Normalizing Flow**
Accepted at **UAI 2026** · joint work with **LG AI Research**

## Abstract

Estimating outcome distributions under time-varying treatments is an essential task for personalized
decision-making, particularly in domains such as healthcare. Most prior work focuses on **point predictions**,
which fail to capture the inherent variability in outcomes. Recent efforts have begun integrating generative models
to estimate interventional distributions — but existing approaches, **including causal normalizing flows**, are
restricted to **static** settings and are not well suited to sequential data.

We propose a framework that extends causal normalizing flows to time-series, enabling **simulation-based
interventional density estimation over time**. The method learns representations of treatment and covariate history
that capture temporal dependencies; conditioned on these and guided by a causal graph, the flow generates
interventional samples, allowing simulation of outcome trajectories under alternative treatment strategies.

## Contributions

1. **Extend causal normalizing flows** to the time-series setting.
2. **Design a learning and inference framework** for multi-step interventional distribution estimation.
3. **Generalize across query types** without requiring query-specific design.

## Problem setup

We consider a multivariate time-series $$\mathcal{X}=(\mathbf{X}_1,\ldots,\mathbf{X}_T)\in\mathbb{R}^{T\times d}$$
whose data-generating process follows a **Markovian structural causal model (SCM)**. At each time step, every
component is generated from its causal parents and a corresponding exogenous disturbance, with the disturbances
jointly independent. We further assume **causal stationarity**: the causal relationships governing the process are
invariant under shifts in time, so the same dependency pattern recurs throughout the series.

With a maximum time lag of $$\tau$$, this recurring structure can be summarized by a **window causal graph**
$$\mathcal{G}$$ over a window of length $$\tau+1$$. The graph is induced by the SCM and assumed to be given. For a
variable $$X^j_t$$, it distinguishes two kinds of parents:

- $$\mathrm{Pa}^{<}_{\mathcal{G}}(X^j_t)$$ — **time-lagged parents**
- $$\mathrm{Pa}^{=}_{\mathcal{G}}(X^j_t)$$ — **contemporaneous parents**

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/tscnf/window-causal-graph.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Window causal graph with tau = 3" %}
  </div>
</div>
<div class="caption">
  An example of a window causal graph with $$\tau = 3$$ highlighting contemporaneous (pink) and time-lagged (red)
  parents of a target variable (blue).
</div>

Suppose that the history $$\bar{\mathbf{x}}_m=(\mathbf{x}_1,\ldots,\mathbf{x}_m)$$ has been observed through time
$$m$$ and that a treatment sequence $$\underline{a}_{m+1}=(a_{m+1},\ldots,a_t)$$ is imposed thereafter. Our target
estimand is the interventional distribution of the future outcome $$Y_t$$ under this treatment sequence:

$$P\!\left(Y_t(\underline{a}_{m+1}) \mid \bar{\mathbf{x}}_m\right), \qquad t \geq m+1$$

More generally, the framework supports both **conditional queries**, obtained by conditioning on the full observed
history or selected pre-intervention information, and **marginal queries**, obtained by integrating over that
information. The experiments below evaluate two representative cases: the marginal query
$$P(Y_t(\underline{a}_{m+1}))$$ and the history-adjusted query shown above.

## Method

### Model architecture

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/tscnf/architecture.png" class="img-fluid rounded z-depth-1" zoomable=true alt="TSCNF architecture" %}
  </div>
</div>

**Causal History Encoder** — encodes only the time-lagged parents $$\mathrm{Pa}^{<}_{\mathcal{G}}(X^j_t)$$ into an
embedding $$h^j_{\phi,t}$$, using a feature-wise **causal (parent) mask** $$\mathbf{M}^j$$:

$$h^j_{\phi,t} = E^j_{\phi}\!\left(\mathbf{M}^j \odot \mathbf{X}_{t-\tau:t-1}\right)$$

**Causal Conditional Normalizing Flows** — model contemporaneous causal dependencies, with each $$X^j_t$$
conditioned on its own embedding:

$$x^j_t = f^j_\theta\!\left(z^j_t;\ \mathrm{Pa}^{=}_{\mathcal{G}}(X^j_t),\ h^j_{\phi,t}\right), \qquad z^j_t \overset{\text{i.i.d.}}{\sim} P_z \quad \forall j$$

**Training** — maximize the observational log-likelihood:

$$\max_{\theta,\,\phi}\ \sum_{t=1}^{T}\sum_{j=1}^{d} \log P_\theta\!\left(X^j_t \mid \mathrm{Pa}^{=}_{\mathcal{G}}(X^j_t),\ h^j_{\phi,t}\right)$$

### Estimation of interventional distribution

The target estimand admits the following form:

$$P\!\left(Y_t(\underline{a}_{m+1}) \mid \bar{\mathbf{x}}_m\right) = \int_{\mathbf{x}'_{m+1:t-1}} P\!\left(Y_t \mid do(a_t), \bar{\mathbf{x}}_{t-1}\right) \prod_{k=m+1}^{t-1} P\!\left(\mathbf{x}'_k \mid do(a_k), \bar{\mathbf{x}}_{k-1}\right)$$

This is **analytically intractable**, so we approximate it by **Monte Carlo simulation** with TSCNF.

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/tscnf/inference.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Simulation-based estimation procedure" %}
  </div>
</div>
<div class="caption">
  At <em>k = t</em>, retain <em>Y<sub>t</sub></em> samples from <strong>x</strong>′<sub><em>t</em></sub> to form the
  empirical distribution.
</div>

## Theoretical guarantee

Does a TSCNF trained on **observational** data recover the exogenous variables needed to implement interventions?
We show that matching the observational distribution isolates each exogenous variable up to a single invertible,
component-wise transformation that is **invariant across time and observed histories**.

> **Theorem (Identifiability of TSCNF).**
> Suppose we fit TSCNF $$f_\theta(\cdot \mid \mathbf{h}_{\phi,t})$$ with fixed base distribution $$P_z$$. If
>
> 1. at $$t=1$$ the conditioning history is empty, so TSCNF reduces to a static causal NF that induces the same
>    initial distribution as the SCM of the initial time step, and
> 2. for almost every history $$\bar{\mathbf{x}}_{t-1}$$ sufficiently encoded by $$\mathbf{h}_{\phi,t}$$, TSCNF induces
>    the same conditional distribution as the history-conditioned contemporaneous SCM,
>
> then there exists a component-wise invertible mapping
>
> $$\mathbf{a}(\mathbf{z}_t)=\bigl(a_1(z^1_t),\ldots,a_d(z^d_t)\bigr),$$
>
> where $$\mathbf{a}$$ is **independent of time and history**, such that
>
> $$f_\theta^{-1}\bigl(f_{\bar{\mathbf{x}}}(\mathbf{z}_t)\mid \mathbf{h}_{\phi,t}\bigr)=\mathbf{a}(\mathbf{z}_t)$$
>
> for almost every history, for all $$t=2,\ldots,T$$.

**Why it matters.** The mapping $$\mathbf{a}(\cdot)$$ links each latent of TSCNF to a single exogenous variable, so
conditional interventions can be implemented by modifying **only the latents of the intervened variables**. Since
the mapping is invariant across time and history, this relation stays stable during multi-step rollouts —
TSCNF can therefore estimate sequential interventional distributions and recover the target estimand at the model
level.

## Experiments

**Setup.** Fully synthetic time-series from linear and non-linear DGPs, plus a simulated PK–PD tumor-growth
benchmark. Metrics: **MMD** and **1-D Wasserstein** for distributional similarity at a given step, and **RMSE** for
multi-step accuracy. Two query types throughout:

- **Marginal** — population-level effects under a hypothetical treatment sequence
- **History-adjusted** — conditioned on each subject's covariate and treatment history

### Ablation study

The full model consistently performs best, and the two mechanisms are complementary:

- **Causal graph masking** alone gives a notable improvement — filtering the history to causally relevant variables
  avoids spurious signals.
- **Feature-wise conditioning** on top of it improves further — each flow dimension conditions solely on its causal
  parents.

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/tscnf/ablation.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Ablation study distribution plots" %}
  </div>
</div>
<div class="caption">
  Marginal and history-adjusted interventional densities under <em>do</em>(X²<sub>t</sub>, X⁴<sub>t</sub>) for
  TSCNF and its ablations in the non-linear dataset (τ = 3).
</div>

<div style="overflow-x: auto;" markdown="1">

| Metric     | Method                 | $$do(X^2_t)$$   | $$do(X^4_t)$$   | $$do(X^2_t, X^4_t)$$ |
| ---------- | ---------------------- | --------------- | --------------- | -------------------- |
| Mean Dist. | w/o both               | 1.98 (1.32)     | 2.03 (1.10)     | 1.61 (1.16)          |
|            | w/o feature-wise cond. | 1.48 (1.05)     | 1.30 (0.88)     | 1.18 (0.86)          |
|            | **TSCNF**              | **0.72 (0.57)** | **0.66 (0.61)** | **0.51 (0.47)**      |
| MMD        | w/o both               | 0.53 (0.29)     | 0.46 (0.23)     | 0.48 (0.25)          |
|            | w/o feature-wise cond. | 0.38 (0.25)     | 0.21 (0.18)     | 0.40 (0.23)          |
|            | **TSCNF**              | **0.11 (0.14)** | **0.08 (0.12)** | **0.13 (0.21)**      |
| Wass.      | w/o both               | 2.19 (1.26)     | 2.39 (1.03)     | 1.79 (1.10)          |
|            | w/o feature-wise cond. | 1.57 (1.03)     | 1.42 (0.87)     | 1.26 (0.85)          |
|            | **TSCNF**              | **0.74 (0.57)** | **0.74 (0.63)** | **0.53 (0.47)**      |

</div>

### A. Synthetic datasets — non-linear DGPs

**Marginal query.** TSCNF and G-Net consistently outperform MSCVAE. MSCVAE relies on inverse propensity weighting,
multiplying treatment probabilities across time — numerically unstable over long horizons. TSCNF and G-Net instead
follow a g-computation-style approach that avoids explicit propensity modeling.

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/tscnf/density-marginal.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Marginal interventional densities" %}
  </div>
</div>
<div class="caption">
  Marginal: $$P(Y_{m+5}(\underline{a}_{m+1}))$$
</div>

**History-adjusted query.** TSCNF is comparable or superior to G-Net across linear and non-linear settings. The gap
widens under **sparser time-lagged structures**: G-Net incorporates all past covariates and treatments without
filtering, which injects noise, whereas TSCNF selectively attends to the relevant time-lagged parents.

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/tscnf/density-history-adjusted.png" class="img-fluid rounded z-depth-1" zoomable=true alt="History-adjusted interventional densities" %}
  </div>
</div>
<div class="caption">
  History-adjusted: $$P(Y_{m+5}(\underline{a}_{m+1}) \mid \bar{\mathbf{x}}_m)$$
</div>

### B. Tumor growth — PK–PD simulation

Point accuracy holds as the prediction horizon $$k$$ grows. Normalized RMSE (%) at each horizon
($$\gamma_c = 5,\ \gamma_d = 5$$; best in **bold**, second best <u>underlined</u>):

<div style="overflow-x: auto;" markdown="1">

| Model      | $$k=1$$            | $$k=2$$            | $$k=3$$            | $$k=4$$            | $$k=5$$            |
| ---------- | ------------------ | ------------------ | ------------------ | ------------------ | ------------------ |
| RMSN       | 0.63 (0.11)        | 0.74 (0.12)        | 0.79 (0.11)        | 0.83 (0.11)        | <u>0.88 (0.11)</u> |
| G-Net      | **0.52 (0.03)**    | <u>0.64 (0.04)</u> | <u>0.72 (0.06)</u> | <u>0.79 (0.09)</u> | 0.92 (0.24)        |
| Causal CPC | 0.70 (0.10)        | 0.77 (0.10)        | 0.81 (0.10)        | 0.84 (0.12)        | 0.88 (0.14)        |
| DoFlow     | 1.01 (0.05)        | 1.54 (0.06)        | 1.90 (0.06)        | 2.19 (0.07)        | 2.40 (0.09)        |
| **TSCNF**  | <u>0.55 (0.09)</u> | **0.58 (0.10)**    | **0.63 (0.10)**    | **0.67 (0.10)**    | **0.71 (0.11)**    |

</div>

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/research/tscnf/tumor-trajectories.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Tumor volume trajectories under different treatment strategies" %}
  </div>
</div>
<div class="caption">
  Tumor volume trajectories sampled from TSCNF for a single patient under various treatment strategies.
</div>

## Resources

- Paper link — _to be added once UAI 2026 proceedings are published_
- Code — _release planned_
