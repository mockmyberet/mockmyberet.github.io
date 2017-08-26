---
layout: page
title: About Me
sidebar_link: true
---

<p class="message">
Just a guy that has an affinity for all things puzzling.
</p>

My LinkedIn Profile:

<script src="//platform.linkedin.com/in.js" type="text/javascript"></script>
<script type="IN/MemberProfile" data-id="https://www.linkedin.com/in/tommybecker" data-format="inline" data-related="false"></script>

{% for repository in site.github.public_repositories %}
{% unless repository.fork %}
  * [{{ repository.name }}]({{ repository.html_url }})
{% endunless %}
{% endfor %}
