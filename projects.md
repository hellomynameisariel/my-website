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
    <div class="project-link" data-project-url="{{ site.baseurl }}{{ post.url }}">
      <strong><a href="{{ site.baseurl }}{{ post.url }}" class="project-title-link">{{ post.title }}</a></strong><br>
      <span>{{ post.excerpt }}</span>
    </div>

  {% endfor %}
</div>

<div id="side-panel" class="hidden" style="
  position: fixed;
  top: 60px; /* 🛠 Start just below nav */
  right: 0;
  width: 40%;
  height: calc(100% - 60px); /* 🛠 Shrink height so footer is still visible */
  background: white;
  box-shadow: -2px 0 5px rgba(0,0,0,0.1);
  overflow-y: auto;
  padding: 20px;
  z-index: 9999;
  transform: translateX(100%);
  transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);

">
  <button id="close-panel" style="
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 20px;
    background: none;
    border: none;
    cursor: pointer;
  ">✖</button>
  <div id="panel-content">Loading...</div>
</div>

<script>
document.querySelectorAll('.project-link').forEach(link => {
  link.addEventListener('click', function(event) {
    event.preventDefault();
    const url = this.getAttribute('data-project-url');

    // Clear panel content immediately when clicking
    document.getElementById('panel-content').innerHTML = "Loading...";

    fetch(url)
      .then(response => response.text())
      .then(html => {
        const parser = new DOMParser();
        const doc = parser.parseFromString(html, 'text/html');
        const content = doc.querySelector('.project-content');
        if (content) {
          document.getElementById('panel-content').innerHTML = content.innerHTML;
        } else {
          document.getElementById('panel-content').innerHTML = "Content not found.";
        }
        // Slide open
        document.getElementById('side-panel').style.transform = 'translateX(0)';
      })
      .catch(error => {
        document.getElementById('panel-content').innerHTML = "Error loading content.";
        document.getElementById('side-panel').style.transform = 'translateX(0)';
      });
  });
});

document.getElementById('close-panel').addEventListener('click', function() {
  document.getElementById('side-panel').style.transform = 'translateX(100%)';
});

// ESC key closes side panel
document.addEventListener('keydown', function(event) {
  if (event.key === "Escape") {
    document.getElementById('side-panel').style.transform = 'translateX(100%)';
  }
});
</script>
