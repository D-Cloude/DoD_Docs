---
layout: page
title: 목차
permalink: /docs/
---

# 목차

{{ site.title }} 이 문서에서는 기초적으로 설정해야하는 절차와 기타 라이브러리를 설치하는 방법에 대해 알아볼것입니다.

###  ✅ 우분투 기준으로 설명되기 때문에 다른 리눅스를 쓰는 분들은 웹에서 검색부탁드림니다.
<div class="section-index">
    <hr class="panel-line">
    {% for post in site.docs  %}        
    <div class="entry">
    <h5><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h5>
    <p>{{ post.description }}</p>
    </div>{% endfor %}
</div>
