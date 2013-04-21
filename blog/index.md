---
layout: default
title: 我的文章
---
{% include JB/setup %}
<div class="row">
    <div class="span9">
        <h2>我的文章【{{ site.categories.blog | size }}】</h2>
        {% assign blogs_list = site.categories.blog %}
        {% include JB/blogs_list %}
    </div>
    <div class="span3">
        {% include JB/public %}
        <hr>
        <div class="span3">
            <h2>所有标签</h2>
            <ul class="tag_box inline">
            {% assign tags_list = site.tags %}
            {% include JB/tags_list %}
            </ul>
        </div>
        <hr>
        <div class="span3">
            <h2>所有文章</h2>
            <ol>
            {% for post in site.categories.blog %}
            <li><a href="{{ post.url }}">{{ post.title }}</a></li>
            {% endfor %}
            </ol>
        </div>
    </div>
</div>


