---
layout: default
title: "Projects"
permalink: /projects/
author_profile: true
css: /assets/css/sidepanel.css
---

<link rel="stylesheet" href="/my-website/assets/css/sidepanel.css">

<div class="projects-list">
  {% for post in site.categories.projects %}
    <div class="project-link" data-project-url="{{ post.url }}">
      <a href="javascript:void(0);"><strong>{{ post.title }}</strong></a><br>
      <span>{{ post.excerpt }}</span>
    </div>
  {% endfor %}
</div>

<div id="side-panel" class="hidden">
  <button id="close-panel">✖</button>
  <div id="panel-content">Loading...</div>
</div>

<script>
document.querySelectorAll('.project-link').forEach(link => {
  link.addEventListener('click', function() {
    const url = '{{ site.baseurl }}' + this.dataset.projectUrl;
    fetch(url)
      .then(response => response.text())
      .then(html => {
        const parser = new DOMParser();
        const doc = parser.parseFromString(ht
