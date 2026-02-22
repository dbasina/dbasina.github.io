---
title: Projects
icon: fas fa-briefcase
order: 2
---

A curated list of my projects. Click any post to read the full write-up.

{% assign projects = site.posts | where_exp: "post", "post.categories contains 'projects'" %}

{% if projects.size > 0 %}
<ul>
  {% for post in projects %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% if post.tags %}<span style="opacity:0.75"> — {{ post.tags | join: ", " }}</span>{% endif %}
    </li>
  {% endfor %}
</ul>
{% else %}
_No projects yet — adding soon._
{% endif %}