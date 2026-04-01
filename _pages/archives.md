---
permalink: /archives/
title: "Archives"
author_profile: true
layout: single
---

{% for post in site.archives %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}
