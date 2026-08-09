---
layout: page
title: research
permalink: /research/
description: Causal AI — causal discovery, causal generative models, and causal effect estimation.
nav: true
nav_order: 2
---

<!-- pages/research.md — one horizontal card per row, with resource buttons -->

<style>
  /* The theme scopes its bib-button border to `.publications ol.bibliography li .links a.btn`,
     so it cannot be reused here. These rules reproduce that look with the same theme tokens,
     which keeps light/dark mode working. Scoped to this page only. */
  .research-list .btn {
    display: inline-block;
    margin: 0 0.4rem 0.4rem 0;
    padding: 0.25rem 1rem;
    font-size: 0.8rem;
    border: 1px solid var(--global-text-color);
    border-radius: 0.25rem;
    color: var(--global-text-color);
    text-decoration: none;
  }
  .research-list a.btn:hover {
    color: var(--global-theme-color);
    border-color: var(--global-theme-color);
  }
  .research-list .btn.disabled {
    opacity: 0.5;
    cursor: default;
    pointer-events: none;
  }
  .research-list .card-title a {
    color: var(--global-theme-color);
    text-decoration: none;
  }
  .research-list .card-img {
    padding: 0.75rem;
  }
</style>

<div class="research-list">
  {% assign sorted_research = site.research | sort: "importance" %}
  {% for item in sorted_research %}
    <div class="card hoverable mb-4">
      <div class="row no-gutters align-items-center">
        {% if item.img %}
          <div class="col-md-4">
            <a href="{{ item.url | relative_url }}">
              {% include figure.liquid loading="eager" path=item.img alt=item.title class="card-img" sizes="(min-width: 768px) 33vw, 100vw" %}
            </a>
          </div>
        {% endif %}
        <div class="{% if item.img %}col-md-8{% else %}col-md-12{% endif %}">
          <div class="card-body">
            <h3 class="card-title mb-1">
              <a href="{{ item.url | relative_url }}">{{ item.title }}</a>
            </h3>
            {% if item.venue %}
              <p class="card-text mb-2"><small class="text-muted">{{ item.venue }}</small></p>
            {% endif %}
            <p class="card-text">{{ item.description }}</p>
            <div class="mt-2">
              <a href="{{ item.url | relative_url }}" class="btn btn-sm z-depth-0" role="button">Details</a>
              {% if item.paper_url %}
                <a href="{{ item.paper_url }}" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener">Paper</a>
              {% elsif item.paper_pending %}
                <span class="btn btn-sm z-depth-0 disabled" aria-disabled="true" title="{{ item.paper_pending }}">Paper — {{ item.paper_pending }}</span>
              {% endif %}
              {% if item.code_url %}
                <a href="{{ item.code_url }}" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener">Code</a>
              {% elsif item.code_pending %}
                <span class="btn btn-sm z-depth-0 disabled" aria-disabled="true" title="{{ item.code_pending }}">Code — {{ item.code_pending }}</span>
              {% endif %}
            </div>
          </div>
        </div>
      </div>
    </div>
  {% endfor %}
</div>
