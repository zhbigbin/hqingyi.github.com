---
layout: default
title: 首页
---
{% include JB/setup %}
<div class="row">
    <div class="span9">
        {% for post in site.categories.blog | limit:5 %}
        <div class="well">
            <div class="row">
                <div class="span3 {% cycle 'pull-left','pull-right' %}"/>
                    <h1>{{ post.date | date: "%B %e,%Y"}}</h1>
                    <h2>《{{ post.title }}》</h2>
                </div>
                <div class="span5">
                    {% if post.content.length > 256 %}
                        {{ post.content | truncate: 256}} </p>
                    {% else %}
                        {{ post.content }}
                    {% endif %}
                    <p><a href="{{ post.url }}" class="btn btn-primary pull-right">查看全文</a></p>
                </div>
            </div>
        </div>
        {% endfor %}
    </div>
    <div class="span3">
        {% tag_cloud %}
    </div>
</div>


