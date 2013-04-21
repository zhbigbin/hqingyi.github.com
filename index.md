---
layout: default
title: 首页
---
{% include JB/setup %}
<div class="row">
    <div class="span9">
        <h2>最新文章...</h2>
        {% for post in site.categories.blog | limit:5 %}
        <div class="well">
            <div class="row">
                <div class="span3 {% cycle 'pull-left','pull-right' %}">
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
        <h2>正在阅读...</h2>
        <div class="row-fluid">
            <ul class="thumbnails">
            {% for book in site.categories.book | limit:3 %}
                <li class="span3">
                    <div class="thumbnail">
                        <a href="{{ book.url }}" class="thumbnail"> 
                            <img style="width: 200px; height: 270px;" class="img-rounded" alt="{{book.title}}" src="book/covers/{{ book.cover }}">
                        </a>
                        <div class="caption">
                            <ul class="tag_box inline">
                                {% assign tags_list = book.tags %}
                                {% include JB/tags_list %}
                            </ul>
                            <h3>打卡:</h3>
                            {% for record in book.records | limit:5 %}
                            <p>{{ record.datetime }}</p>
                            {% endfor %}
                        </div>
                    </div>
                </li>
            {% endfor %}
            </ul>
        </div>
    </div>
    <div class="span3">
        <div>
            <h2>所有标签</h2>
            <ul class="tag_box inline">
            {% assign tags_list = site.tags %}
            {% include JB/tags_list %}
            </ul>
        </div>
    </div>
</div>


