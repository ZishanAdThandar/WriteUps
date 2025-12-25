---
layout: default
title: All Posts
---

<div class="post-grid">
{% for post in site.posts limit:9 %}
  <article class="post-card">
    <h2>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </h2>
    {% if post.description %}
      <p>{{ post.description }}</p>
    {% endif %}
  </article>
{% endfor %}
</div>

