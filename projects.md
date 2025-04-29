---
layout: default
title: "Projects"
permalink: /projects/
author_profile: true
css: /assets/css/sidepanel.css
---

<div class="projects-list">
  {% assign project_posts = site.pages | where_exp: "page", "page.categories contains 'projects'" %}
  {% for post in project_posts %}
    <div class="project-link" data-project-url="{{ post.url | relative_url }}">
      <strong>{{ post.title }}</strong><br>
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
  link.addEventListener('click', function(event) {
    event.preventDefault();
    const url = this.getAttribute('data-project-url');
    fetch(url)
      .then(response => response.text())
      .then(html => {
        document.getElementById('panel-content').innerHTML = html;
        document.getElementById('side-panel').classList.remove('hidden');
      })
      .catch(error => {
        document.getElementById('panel-content').innerHTML = "Error loading content.";
        document.getElementById('side-panel').classList.remove('hidden');
      });
  });
});

document.getElementById('close-panel').addEventListener('click', function() {
  document.getElementById('side-panel').classList.add('hidden');
});
</script>
