---
layout: page
title: 목차
permalink: /getstart/
---

# 목차

{{ site.title }} 이곳에서는 사이트 사용법하고 신청하는 방법에 대해 설명을 해볼것입니다.

<div class="section-index">
    <hr class="panel-line">
    {% for post in site.getstart  %}        
    <div class="entry">
    <h5><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h5>
    <p>{{ post.description }}</p>
    </div>{% endfor %}
</div>
