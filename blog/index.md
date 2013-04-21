---
layout: default
title: 博客 
---
{% include JB/setup %}

<div id="content">
    <div class="text-post posts">

	{% for post in site.categories.blog limit:5 %}

		<h2><a class="post_title" href="{{ post.url }}">{{post.title}}</a></h2>

		<div class="caption rich-content">
			{{ post.content }}
		</div>

	{% endfor %}

    </div>
</div>
