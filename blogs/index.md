---
layout: page
title: "Blog"
description: ""
group: navigation
---
{% include JB/setup %}

<div>
    {% for post in site.posts limit 4 %}
        <h1><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h1><small>{{ post.date | date_to_long_string }}</small>
        <p>{{ post.excerpt }}</p><br>
    {% endfor %}
</div>