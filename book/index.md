---
layout: default
title: 阅读 
---
{% include JB/setup %}

<div id="content">
    <div class="text-post posts">

	{% for post in site.categories.book limit:5 %}

		<h2><a class="post_title" href="{{ post.url }}">{{post.title}}</a></h2>

		<div class="caption rich-content">
			{{ post.content }}
		</div>

	{% endfor %}

    </div>
</div>
