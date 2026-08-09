---
layout: page
permalink: /publications/
title: publications
description: Peer-reviewed publications, most recent first.
nav: true
nav_order: 3
---

<!-- _pages/publications.md -->

<style>
  /* Author emphasis. `_layouts/bib.liquid` (al_folio_core) wraps the site owner's
     name in <em> and leaves every co-author as plain text, so the two can be
     styled apart here without touching the gem. Theme tokens keep dark mode working.
     `max_author_limit` in _config.yml controls how many names show before the
     "N more authors" toggle — it is set to 4 so the owner's name is always visible. */
  .publications ol.bibliography li .author {
    color: var(--global-text-color-light);
  }
  .publications ol.bibliography li .author em {
    color: var(--global-theme-color);
    font-style: normal;
    font-weight: 600;
  }
  .publications ol.bibliography li .author a {
    color: var(--global-text-color-light);
  }
</style>

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

{% bibliography %}

</div>
