---
layout: default
title: 首页
---
{% include JB/setup %}
<div class="row">
    <div class="span9">
        <h2>最新文章 <a href="{{ HOME_PATH }}blog" style="font-size:18px">(更多...)</a></h2>
        {% for post in site.categories.blog | limit:2 %}
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
        <h2>正在阅读 <a href="{{ HOME_PATH }}book" style="font-size:18px">(更多...)</a></h2>
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
                            <h4>作者: {{ book.author }}</h4>
                            <h4>加入时间: {{ book.date | date: "%Y-%m-%d"}}</h4>
                            <h4>评星: {{ book.star }}</h4>
                            <h4>相关链接: 
                                {% for link in book.urls %}
                                <a class="btn btn-primary btn-mini" href="{{ link.url }}">{{ link.text }} </a>
                                {% endfor %}
                            </h4> 
                        </div>
                    </div>
                </li>
            {% endfor %}
            </ul>
        </div>
    </div>
    <div class="span3">
        <hr>
        <div class="span3">
            <h2>青衣秀士</h2>
                <img src="images/hqingyi.jpg" style="width:140px;height:140px" class="img-rounded">
                <div class="caption">
                    <p>所在地：北京</p>
                    <p>从今天起，做一个幸福的程序员，看书编码调试程序。从明天起，关心性能优化，给每一个结构、算法取一个温暖的名字。我有一个工位，背朝厕所春暖花开。</p>
                </div>
        </div>
        <hr>
        <div class="span3">
            <h2>所有标签</h2>
            <ul class="tag_box inline">
            {% assign tags_list = site.tags %}
            {% include JB/tags_list %}
            </ul>
        </div>
    </div>
</div>


