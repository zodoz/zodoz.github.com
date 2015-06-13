---
layout: page
title: Tools
permalink: /tools/
---

List of tools I use and notes about how I use them.

Tools:
{% for tool in site.tools %}
* [{{ tool.title }}]({{ tool.url }})
{% endfor %}
