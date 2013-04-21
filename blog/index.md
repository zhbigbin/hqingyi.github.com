---
layout: default
title: 我的文章
---
{% include JB/setup %}
<div class="row">
    <div class="span9">
        <h2>我的文章</h2>
        {% for post in site.categories.blog %}
        <div class="well">
            <div class="row">
                <div class="span3 {% cycle 'pull-left','pull-right' %}">
                    <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
                    <h2>{{ post.date | date: "%Y-%m-%d"}}</h2>                                                                                                                                          
                     <ul class="tag_box inline">
                        {% assign tags_list = post.tags %}
                        {% include JB/tags_list %}
                    </ul>
                </div>
                <div class="span5">
                    <p style="font-size:18px">{{ post.description }}</p>
                    <p><a href="{{ post.url }}" class="btn btn-primary pull-right">查看全文</a></p>
                </div>
            </div>
        </div>
        {% endfor %}
    </div>
    <div class="span3">
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


