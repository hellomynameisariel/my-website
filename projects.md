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

<div id="side-panel" class="hidden" style="position:fixed; top:0; right:0; width:40%; height:100%; background:white; box-shadow:-2px 0 5px rgba(0,0,0,0.1); overflow-y:auto; padding:20px; z-index:9999;">
  <button id="close-panel" style="position:absolute; top:10px; right:10px; font-size:20px; background:none; border:none; cursor:pointer;">✖</button>
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
        const parser = new DOMParser();
        const doc = parser.parseFromString(html, 'text/html');
        const content = doc.querySelector('.project-content');
        if (content) {
          document.getElementById('panel-content').innerHTML = content.innerHTML;
          document.getElementById('side-panel').classList.remove('hidden');
        } else {
          document.getElementById('panel-content').innerHTML = "Content not found.";
          document.getElementById('side-panel').classList.remove('hidden');
        }
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

// Close side panel on ESC key
document.addEventListener('keydown', function(event) {
  if (event.key === "Escape") {
    document.getElementById('side-panel').classList.add('hidden');
  }
});
</script>
