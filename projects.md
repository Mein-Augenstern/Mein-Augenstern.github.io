---
title: "项目"
layout: single
---

<style>
.page__title {
  display: none;
}
.projects-header {
  text-align: center;
  padding: 2rem 0;
  margin-bottom: 2rem;
}
.projects-header h1 {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
}
.project-card {
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 1.5rem;
  text-align: center;
  transition: all 0.3s;
}
.project-card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  transform: translateY(-2px);
}
.project-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}
.project-card h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.2rem;
}
.project-card h3 a {
  text-decoration: none;
  color: #333;
}
.project-card h3 a:hover {
  color: #667eea;
}
.project-desc {
  color: #666;
  font-size: 0.9rem;
  line-height: 1.5;
  margin-bottom: 1rem;
}
.project-link {
  display: inline-block;
  background: #667eea;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  text-decoration: none;
  font-size: 0.9rem;
}
.project-link:hover {
  background: #5568d3;
}
</style>

<div class="projects-header">
  <h1>项目</h1>
  <p>我的开源项目和个人作品</p>
</div>

<div class="projects-grid">
  <div class="project-card">
    <div class="project-icon">📝</div>
    <h3><a href="#">博客项目</a></h3>
    <p class="project-desc">个人技术博客，用于记录工作与学习经历</p>
    <a href="https://github.com/Mein-Augenstern/Mein-Augenstern.github.io" class="project-link" target="_blank">查看源码</a>
  </div>

  <div class="project-card">
    <div class="project-icon">🔧</div>
    <h3><a href="#">Java工具库</a></h3>
    <p class="project-desc">日常开发中积累的工具类封装</p>
    <a href="#" class="project-link">查看详情</a>
  </div>

  <div class="project-card">
    <div class="project-icon">📚</div>
    <h3><a href="#">面试笔记</a></h3>
    <p class="project-desc">Java八股文面试题整理</p>
    <a href="#" class="project-link">查看详情</a>
  </div>
</div>