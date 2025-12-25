---
layout: default
title: Home
---

<img src="/banner.png" alt="ZishanHack Banner" style="width:100%;max-height:260px;object-fit:cover;margin-bottom:2rem;" />

## About This Space

This site is a curated knowledge base focused on practical offensive security, real-world bug bounty workflows, and structured certification-oriented notes.  
Content here prioritizes clarity, repeatability, and methodology over noise or trend-driven material.

Everything published is written from hands-on experience and intended to be usable as reference material during active learning or real engagements.

---

## Start Here

<div class="post-grid">

<article class="post-card">
  <h3><a href="/posts/blog/">Technical Blog</a></h3>
  <p>Long-form explanations, deep dives, and structured notes on security concepts, tooling, and workflows.</p>
</article>

<article class="post-card">
  <h3><a href="/posts/bugbounty/">Bug Bounty Notes</a></h3>
  <p>Realistic bug hunting notes, checklists, and patterns focused on modern web applications.</p>
</article>

<article class="post-card">
  <h3><a href="/posts/hackthebox/">Hack The Box</a></h3>
  <p>Machine walkthroughs and methodology-driven approaches for labs and CTF-style environments.</p>
</article>

</div>

---

## Recent Notes

<div class="post-grid">
{% for post in site.posts limit:6 %}
  <article class="post-card">
    <h3>
      <a href="{{ post.url | relative_url }}">
        {{ post.title }}
      </a>
    </h3>
    {% if post.description %}
      <p>{{ post.description }}</p>
    {% endif %}
  </article>
{% endfor %}
</div>

---

## Navigation

You can browse all content through the category sections above or access specific notes directly if you already know what you’re looking for.  
This site is intentionally minimal and optimized for long-term reference rather than feed-style consumption.

