---
title: "关于我"
permalink: /about/
layout: single
author_profile: true
---

<style>
.page__title {
  display: none;
}
.about-intro {
  text-align: center;
  padding: 2rem 0;
}
.about-intro img {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  margin-bottom: 1rem;
}
.about-intro h1 {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}
.about-intro .location {
  color: #666;
  margin-bottom: 1rem;
}
.about-section {
  margin-bottom: 2rem;
}
.about-section h2 {
  font-size: 1.3rem;
  border-left: 4px solid #667eea;
  padding-left: 0.75rem;
  margin-bottom: 1rem;
}
.about-section p {
  line-height: 1.8;
  color: #444;
}
.skills-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
.skill-tag {
  background: #667eea;
  color: white;
  padding: 0.4rem 0.8rem;
  border-radius: 4px;
  font-size: 0.9rem;
}
.contact-links {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-top: 1rem;
}
.contact-links a {
  color: #667eea;
  font-size: 1.2rem;
}
</style>

<div class="about-intro">
  <img src="/images/xiaopu_person.jpg" alt="Avatar" onerror="this.style.display='none'">
  <h1>{{ site.author.name }}</h1>
  <p class="location">📍 {{ site.author.location }}</p>
  <p>{{ site.author.bio }}</p>
  <div class="contact-links">
    {% for link in site.author.links %}
    <a href="{{ link.url }}" title="{{ link.title }}" target="_blank">
      <i class="{{ link.icon }}"></i>
    </a>
    {% endfor %}
  </div>
</div>

<div class="about-section">
  <h2>关于我</h2>
  <p>Java后端开发者，热爱技术，专注后端开发领域。喜欢探索新技术，分享学习心得。</p>
</div>

<div class="about-section">
  <h2>技能</h2>
  <div class="skills-list">
    <span class="skill-tag">Java</span>
    <span class="skill-tag">Spring Boot</span>
    <span class="skill-tag">JVM</span>
    <span class="skill-tag">并发编程</span>
    <span class="skill-tag">数据库</span>
    <span class="skill-tag">Redis</span>
    <span class="skill-tag">微服务</span>
  </div>
</div>

<div class="about-section">
  <h2>联系方式</h2>
  <p>欢迎通过以下方式联系我：</p>
  <div class="contact-links">
    {% for link in site.author.links %}
    <a href="{{ link.url }}" title="{{ link.title }}" target="_blank">
      <i class="{{ link.icon }}"></i> {{ link.title }}
    </a>
    {% endfor %}
  </div>
</div>