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
<h3> [{{ repository.name }}]({{ repository.html_url }}) </h3>
<h4> {{repository.description}} </h4>
</li>
{% endunless %}
{% endfor %}
