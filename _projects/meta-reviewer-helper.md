---
layout: page
title: Meta-Reviewer Helper
description: An LLM-based framework that generates meta-reviews of academic papers — summarizing reviews, reviewer disagreements, and author–reviewer discussions.
img: assets/img/projects/meta-reviewer-helper/framework.png
importance: 1
category: llm
period: 2024.09 – 2024.12
tags: Large Language Model, RAG, Prompt Engineering, Streamlit
code_pending: not public
---

**LLM-based automatic meta-review generation framework**
Graduate School of Data Science, Seoul National University · Data Science Project (Team 8) · 2024.09 – 2024.12

<div class="row justify-content-center mt-4 mb-2">
  <div class="col-12">
    {% include figure.liquid path="assets/img/projects/meta-reviewer-helper/framework.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Meta-Reviewer Helper architecture" %}
  </div>
</div>
<div class="caption">
  Architecture: papers and reviews are pulled from OpenReview.net, prepared for retrieval, and passed through three
  summarization paths, with results served in a Streamlit web page.
</div>

## Motivation

A meta-reviewer has to absorb a large volume of papers, reviews, disagreements and discussions within a limited
amount of time. The goal was to use an LLM to summarize reviews, inconsistencies and the discussion process, so
that the decision-making itself is supported.

## What it does

The framework produces three summaries:

1. **Review summary** — scores, strengths, weaknesses, and opinions
2. **Inconsistency summary** — where reviewers contradict each other
3. **Discussion summary** — the main points of contention between authors and reviewers, and the conclusion

## How it works

- Papers and reviews are collected and chunked, then retrieved with **RAG** to build the LLM context.
- **Special-token-based prompt design** produces the review summary, the inconsistency summary and the discussion
  summary in a structured form.
- Passages driving an inconsistency are **highlighted directly in the paper PDF** through the `pymupdf` API.
- Results are visualized in a **Streamlit** web page.

The prompt for inconsistency extraction is structured so the output can be parsed:

```text
Identify any inconsistencies among reviewers and summarize them using the following format
for each inconsistency:
<inconsistency>Inconsistency number</inconsistency>
<inconsistency summary> ... </inconsistency summary>
<reviewer>Reviewer ID</reviewer>
<comments> ... </comments>
If there is no inconsistency at all, simply respond with: No inconsistencies found.
```

## Results

- Applying the summarization features **improved meta-review similarity** compared with not using them.
- The service runs at roughly **$0.049 per paper**.
- Evaluation compared meta-reviews generated with and without the helper against the actual ground-truth
  meta-review, using **cosine similarity at the embedding-vector level**.

## My contribution

- **Dataset research** — selected the ICLR 2023 OpenReview dataset, which supports Python API usage, has
  consistent formatting, and contains a mix of structured and unstructured data formats.
- **LLM prompt engineering** — designed the special-token-based prompts that produce the three summaries in a
  structured form.
- **Evaluation development** — designed the comparison in which meta-reviews generated with and without the
  helper are each measured against the ground-truth meta-review by embedding-level cosine similarity.

## Resources

- Code — _not public_
