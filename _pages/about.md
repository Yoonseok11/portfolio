---
layout: about
title: about
permalink: /
subtitle:

profile:
  align: right
  image: prof_pic.jpg
  image_circular: false # crops the image to make it circular
  more_info: >
    <p>Seoul, South Korea</p>

selected_papers: false # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: false # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: false
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

I am an **AI Researcher / AI Engineer at LG CNS**, where I have been part of the Agentic AI Business Team since
January 2026. I conduct AI research across academia and industry and build agentic AI products, including
general-purpose prebuilt agents and MCP tools.

I completed my **M.S. in Data Science at Seoul National University** (Feb 2026) in the Causality Lab under the
supervision of [Prof. Sanghack Lee](https://www.sanghacklee.me/). My research focused on **Causal AI**, including
causal inference using causal generative models for time-series data, scalable causal discovery, and causal discovery
informed by prior knowledge from LLMs. Much of this work was conducted in collaboration with **LG AI Research**. I previously received a
**B.S. in Engineering from Korea University** (Feb 2024), majoring in Biomedical Engineering and completing an
interdisciplinary major in Artificial Intelligence.

<style>
  .home-cards .btn {
    display: inline-block;
    margin: 0 0.4rem 0.4rem 0;
    padding: 0.25rem 1rem;
    font-size: 0.8rem;
    border: 1px solid var(--global-text-color);
    border-radius: 0.25rem;
    color: var(--global-text-color);
    text-decoration: none;
  }
  .home-cards a.btn:hover {
    color: var(--global-theme-color);
    border-color: var(--global-theme-color);
  }
  .home-cards .card-title a {
    color: var(--global-theme-color);
    text-decoration: none;
  }
  .home-cards .card-img {
    padding: 0.6rem;
  }
</style>

<!-- The about layout floats the profile column right; without this the first card
     wraps beside it and renders narrower than the rest. -->
<div style="clear: both;"></div>

## Research

<div class="home-cards">
  {% assign featured_research = site.research | sort: "importance" %}
  {% for item in featured_research %}
    <div class="card hoverable mb-3">
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
              {% endif %}
              {% if item.code_url %}
                <a href="{{ item.code_url }}" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener">Code</a>
              {% endif %}
            </div>
          </div>
        </div>
      </div>
    </div>
  {% endfor %}
</div>

## Projects

<div class="home-cards">
  {% assign featured_projects = site.projects | sort: "importance" %}
  {% for item in featured_projects %}
    <div class="card hoverable mb-3">
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
            {% if item.period %}
              <p class="card-text mb-2"><small class="text-muted">{{ item.period }}</small></p>
            {% endif %}
            <p class="card-text">{{ item.description }}</p>
            <div class="mt-2">
              <a href="{{ item.url | relative_url }}" class="btn btn-sm z-depth-0" role="button">Details</a>
              {% if item.code_url %}
                <a href="{{ item.code_url }}" class="btn btn-sm z-depth-0" role="button" target="_blank" rel="noopener">Code</a>
              {% endif %}
            </div>
          </div>
        </div>
      </div>
    </div>
  {% endfor %}
</div>
