---
title: "文章"
layout: single
---

<style>
.page__title {
  display: none;
}
.blog-header {
  text-align: center;
  padding: 2rem 0;
  margin-bottom: 2rem;
}
.blog-header h1 {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}
.blog-header .post-count {
  color: #666;
}
.categories-filter {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 2rem;
  justify-content: center;
}
.category-btn {
  background: #f0f0f0;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  text-decoration: none;
  color: #333;
  font-size: 0.9rem;
  transition: all 0.3s;
}
.category-btn:hover, .category-btn.active {
  background: #667eea;
  color: white;
}
.posts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}
.post-card {
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 1.5rem;
  transition: box-shadow 0.3s;
}
.post-card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
.post-card h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.2rem;
}
.post-card h3 a {
  text-decoration: none;
  color: #333;
}
.post-card h3 a:hover {
  color: #667eea;
}
.post-card .post-meta {
  color: #999;
  font-size: 0.85rem;
  margin-bottom: 0.5rem;
}
.post-card .post-excerpt {
  color: #666;
  font-size: 0.95rem;
  line-height: 1.6;
}
.post-card .post-tags {
  margin-top: 1rem;
}
.post-card .tag {
  background: #e8e8e8;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
  font-size: 0.8rem;
  color: #666;
  margin-right: 0.5rem;
}
</style>

<div class="blog-header">
  <h1>文章</h1>
  <p class="post-count">共 {{ site.posts.size }} 篇</p>
</div>

<div class="categories-filter">
  <a href="/blog/" class="category-btn active">全部</a>
  {% for category in site.categories %}
  <a href="/blog/#{{ category[0] }}" class="category-btn">{{ category[0] }}</a>
  {% endfor %}
</div>

<div class="posts-grid">
  {% for post in site.posts %}
  <div class="post-card" id="{{ post.categories | first }}">
    <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
    <div class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</div>
    {% if post.tags %}
    <div class="post-tags">
      {% for tag in post.tags %}
      <span class="tag">{{ tag }}</span>
      {% endfor %}
    </div>
    {% endif %}
  </div>
  {% endfor %}
</div>