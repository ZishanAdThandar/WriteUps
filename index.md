---
layout: default
title: All Posts
---

<div class="post-grid">
{% for post in paginator.posts %}
  <article class="post-card">
    <h2 class="post-title">
      <a href="{{ post.url | relative_url }}">
        {{ post.title }}
      </a>
    </h2>

    {% if post.description %}
    <p class="post-desc">
      {{ post.description }}
    </p>
    {% endif %}

    <span class="post-meta">
      {{ post.date | date: "%Y-%m-%d" }}
    </span>
  </article>
{% endfor %}
</div>

<nav class="pagination">
  {% if paginator.previous_page %}
    <a class="page-btn"
       href="{{ paginator.previous_page_path | relative_url }}">
      ← Newer
    </a>
  {% endif %}

  {% if paginator.next_page %}
    <a class="page-btn"
       href="{{ paginator.next_page_path | relative_url }}">
      Older →
    </a>
  {% endif %}
</nav>

