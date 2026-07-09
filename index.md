---
layout: default
title: Home
---

<div class="hero">
  <h1 class="hero__title">Welcome to The Ireland Street Cats</h1>
  <p class="hero__subtitle">A community cat colony in Burlington, NC — trapped, neutered, vaccinated, and loved by a neighbor.</p>
  <div class="hero__actions">
    <a href="{{ '/pages/help.html' | relative_url }}" class="btn btn--primary">How to Help</a>
    <a href="{{ '/pages/about-tnr.html' | relative_url }}" class="btn btn--secondary">What is TNR?</a>
  </div>
</div>

<h2 class="section-title">Meet the Cats</h2>

<div class="cat-grid">
  {% assign cats = site.cats | sort: "name" %}
  {% for cat in cats %}
    {% include cat-card.html cat=cat %}
  {% endfor %}
</div>

<div class="callout">
  <strong>These cats are community cats.</strong> They are ear-tipped (a universal sign they've been fixed), vaccinated, and cared for by a neighbor. Please don't remove or relocate them — they have a home here.
</div>
