---
permalink: /archives/
title: "Archives"
author_profile: true
layout: single
---

<div class="entries-grid">
{% for post in site.archives %}
  <div class="grid__item">
    <article class="archive__item">
      <h2 class="archive__item-title no_toc" style="margin-top: 0;">
        <a href="{{ post.url }}" rel="permalink">{{ post.title }}</a>
      </h2>
      {% if post.excerpt %}
        <p class="archive__item-excerpt">{{ post.excerpt | markdownify | strip_html | truncate: 160 }}</p>
      {% endif %}
    </article>
  </div>
{% endfor %}
</div>
