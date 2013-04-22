---
layout: default
title: 我的文章
---
{% include JB/setup %}
<div class="row">
    <div class="span9">
        <h2>我的文章【{{ site.categories.blog | size }}】</h2>
        {% assign blogs_list = site.categories.blog %}
        {% include custom/blogs_abs %}
    </div>
    <div class="span3">
        <hr>
        {% include custom/p404_side %}
        <hr>
        {% include custom/all_tag_list_side %}
        <hr>
        {% include custom/all_blog_list_side %}
    </div>
</div>


