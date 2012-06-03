---
layout: page
title: 
tagline: 
---
{% include JB/setup %}

## Posts
<div class="row"> 
  <div class="span8">
     {% assign first_post = site.posts.first %} 

     {{ first_post.content }}
  </div>

<div class="span4">  

   <a href="https://twitter.com/stevendborrelli" class="twitter-follow-button" data-show-count="false">Follow @stevendborrelli</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0];if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src="//platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
</div>
<div class="span4">
          Recent Posts: 
	  <ul class="posts">
	  {% for post in site.posts %}
	    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	  {% endfor %}
	</ul>
 </div>
</div>
