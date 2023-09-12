---
layout: page
title: Daily
description: Daily Tasks
---
Here are the pages where I'm enrolled to complete tasks daily:

{% for category in site.data.daily %}
{% assign items = category[1] | sort_natural: "name" %}

### {{ category[0] | capitalize }}:

{% for item in items %}

* [{{ item.name }}]({{ item.link }}){:target="_blank"}

  {% if item.description %}
  * {{ item.description }}
  {% endif %}
  {% if item.current %}
  * [{{ item.current }}]({{ item.currenurl }}){:target="_blank"} ![Progress](https://progress-bar.dev/{{ item.currentprogress }}/)
  {% endif %}

{% endfor %}
{% endfor %}

If you have any suggestions to add, or a better way to organize this page, feel free to let me know in the comments.

----

{% if site.comments_repo %}
{% include comments.html %}
{% endif %}
