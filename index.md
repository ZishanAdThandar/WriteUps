---
layout: default
title: "InfoSec WriteUps"
sitemap:
  changefreq: "daily"
  priority: 1.0
---

<div class="hero-terminal">
  <div class="terminal-header">
    <div class="terminal-dots">
      <span class="dot red"></span>
      <span class="dot yellow"></span>
      <span class="dot green"></span>
    </div>
    <span class="terminal-title">zishan@blog:~</span>
  </div>
  
  <div class="terminal-body">
    <p><span class="prompt">$</span> whoami</p>
    <p class="output">Zishan Ahamed Thandar</p>
    
    <p><span class="prompt">$</span> cat bio.txt</p>
    <p class="output">Cybersecurity Researcher | Bug Bounty Hunter | CTF Player</p>
    
    <p><span class="prompt">$</span> ls -la posts/</p>
    <div class="directory">
      {% assign categories = "bugbounty,hackthebox,tryhackme,vulnhub,offsec,blog" | split: "," %}
      {% for category in categories %}
        {% assign posts = site.pages | where_exp: "p", "p.path contains 'posts/" | where_exp: "p", "p.path contains category" | size %}
        {% if posts > 0 %}
        <div class="dir-item">
          <span class="dir-icon">📁</span>
          <a href="/posts/{{ category }}/">{{ category }}/</a>
          <span class="count">{{ posts }} items</span>
        </div>
        {% endif %}
      {% endfor %}
    </div>
    
    <p><span class="prompt">$</span> <span class="cursor">█</span></p>
  </div>
</div>

<div class="cta-buttons">
  <a href="{{ site.linktree }}" class="btn primary" target="_blank">
    <i class="fas fa-external-link-alt"></i>
    All Links @ LinkTree
  </a>
  <a href="{{ site.portfolio }}" class="btn secondary" target="_blank">
    <i class="fas fa-user-secret"></i>
    View Portfolio
  </a>
</div>

<div class="stats">
  {% assign total_posts = 0 %}
  {% for category in categories %}
    {% assign posts = site.pages | where_exp: "p", "p.path contains 'posts/" | where_exp: "p", "p.path contains category" | size %}
    {% assign total_posts = total_posts | plus: posts %}
  {% endfor %}
  
  <div class="stat">
    <div class="number">{{ total_posts }}+</div>
    <div class="label">Writeups</div>
  </div>
  <div class="stat">
    <div class="number">1000+</div>
    <div class="label">Code Snippets</div>
  </div>
  <div class="stat">
    <div class="number">4.9/5</div>
    <div class="label">Quality</div>
  </div>
  <div class="stat">
    <div class="number">∞</div>
    <div class="label">Learning</div>
  </div>
</div>

<div id="featured" class="featured">
  <h2><i class="fas fa-fire"></i> Recent Writeups</h2>
  
  <div class="writeup-grid">
    {% assign recent_posts = site.pages | where_exp: "p", "p.path contains 'posts/" | where_exp: "p", "p.path != 'posts/'" | sort: "date" | reverse | slice: 0, 6 %}
    
    {% for post in recent_posts %}
      {% if post.title and post.url %}
        {% assign category = post.path | split: '/' | shift | first %}
        <div class="card">
          <div class="category {{ category }}">{{ category | upcase }}</div>
          <h3><a href="{{ post.url }}">{{ post.title | default: post.path | split: '/' | last | replace: '.md', '' }}</a></h3>
          {% if post.excerpt %}
            <p>{{ post.excerpt | strip_html | truncate: 100 }}</p>
          {% else %}
            <p>Detailed walkthrough and analysis...</p>
          {% endif %}
          <div class="meta">
            {% if post.date %}
            <span><i class="far fa-calendar"></i> {{ post.date | date: "%b %d" }}</span>
            {% endif %}
            <span><i class="far fa-clock"></i> {% include read-time.html content=post.content %}</span>
          </div>
        </div>
      {% endif %}
    {% endfor %}
    
    {% if recent_posts.size == 0 %}
      <!-- Fallback content -->
      <div class="card">
        <div class="category bugbounty">BUG BOUNTY</div>
        <h3><a href="/posts/bugbounty/1">Stored XSS on Edmodo.com</a></h3>
        <p>First bug bounty find with detailed methodology</p>
        <div class="meta">
          <span><i class="far fa-calendar"></i> Recent</span>
          <span><i class="far fa-clock"></i> 8 min</span>
        </div>
      </div>
      
      <div class="card">
        <div class="category hackthebox">HACKTHEBOX</div>
        <h3><a href="/posts/hackthebox/lame">Lame - Easy Linux</a></h3>
        <p>SMB exploitation and privilege escalation</p>
        <div class="meta">
          <span><i class="far fa-calendar"></i> Recent</span>
          <span><i class="far fa-clock"></i> 10 min</span>
        </div>
      </div>
      
      <div class="card">
        <div class="category tryhackme">TRYHACKME</div>
        <h3><a href="/posts/tryhackme/blue">Blue - Windows</a></h3>
        <p>EternalBlue exploit and post-exploitation</p>
        <div class="meta">
          <span><i class="far fa-calendar"></i> Recent</span>
          <span><i class="far fa-clock"></i> 15 min</span>
        </div>
      </div>
    {% endif %}
  </div>
  
  <div class="view-all">
    <a href="/posts/" class="view-all-btn">
      <i class="fas fa-book-open"></i>
      Browse All Writeups
    </a>
  </div>
</div>

<div class="linktree-section">
  <h2><i class="fas fa-link"></i> Connect With Me</h2>
  
  <div class="linktree-card">
    <div class="avatar">
      <i class="fas fa-user-secret"></i>
    </div>
    <h3>Zishan Ahamed Thandar</h3>
    <p class="subtitle">Cybersecurity Researcher</p>
    
    <div class="links">
      <a href="{{ site.linktree }}" class="link main" target="_blank">
        <i class="fas fa-external-link-alt"></i>
        LinkTree (All Links)
      </a>
      <a href="{{ site.portfolio }}" class="link" target="_blank">
        <i class="fas fa-user-secret"></i>
        Portfolio
      </a>
      <a href="{{ site.github }}" class="link" target="_blank">
        <i class="fab fa-github"></i>
        GitHub
      </a>
      <a href="{{ site.linkedin }}" class="link" target="_blank">
        <i class="fab fa-linkedin"></i>
        LinkedIn
      </a>
    </div>
    
    <div class="sponsor">
      <a href="https://github.com/sponsors/ZishanAdThandar" class="sponsor-btn" target="_blank">
        <i class="fas fa-heart"></i>
        Sponsor My Work
      </a>
    </div>
  </div>
</div>
