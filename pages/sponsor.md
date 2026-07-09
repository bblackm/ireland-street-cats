---
layout: default
title: Sponsor or Adopt a Cat
permalink: /pages/sponsor.html
---

<div class="page-header">
  <h1>Sponsor or Adopt a Cat</h1>
</div>

<div class="content-body">

## Sponsor a Cat

Can't adopt but want to help? Sponsoring a cat means you help cover the cost of their ongoing care — food, vet visits, and supplies. In return, you'll know you're making a real difference in that specific cat's life.

To inquire about sponsoring a cat, [get in touch]({{ '/pages/contact.html' | relative_url }}).

## Adoptable Cats

Some cats in our colony — particularly those that are friendlier and more socialized — may be available for adoption into loving indoor homes.

{% assign adoptable = site.cats | where: "status", "adoptable" %}
{% if adoptable.size > 0 %}
  <div class="cat-grid">
    {% for cat in adoptable %}
      {% include cat-card.html cat=cat %}
    {% endfor %}
  </div>
{% else %}
  <div class="callout">
    <strong>No cats are listed as adoptable right now.</strong> Most of our colony cats are free-roaming and happy outdoors, but that can change. Check back, or <a href="{{ '/pages/contact.html' | relative_url }}">reach out</a> if you're interested in adopting a community cat in the future.
  </div>
{% endif %}

## About Community Cat Adoptions

Adopting a community cat is a little different from adopting a shelter cat. These cats have spent their lives outdoors, so:

- **Socialization takes time and patience.** Some cats warm up quickly; others may always prefer minimal handling.
- **A secure indoor environment is essential.** A cat that's used to the outdoors needs time to adjust before having any outdoor access.
- **We'll work with you.** We want every adoption to succeed, so we'll be honest about each cat's temperament and what to expect.

</div>
