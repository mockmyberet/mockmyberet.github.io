---
title: My Repos
layout: page
sidebar_link: true
---
Below are a list of my Github repositories.

<ul class="posts-list">
{% for repository in site.github.public_repositories %}
{% unless repository.fork %}
<li>
  <h3>
    <a href="{{ repository.html_url }}">
      {{ repository.name }}</a><br>
      <small>{{repository.description}}</small>
  </h3>
</li>
{% endunless %}
{% endfor %}
