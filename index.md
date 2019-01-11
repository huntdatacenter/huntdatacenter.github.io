---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

<h2 class="post-list-heading">Posts</h2>

<ul class="post-list">
    {% for post in site.posts %}
        <li><span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
            <h3><a class="post-link" href="{{ post.url | prepend: site.baseurl }}">
                {{ post.title }}
            </a></h3>
            <span>{{ post.content }}</span>
            <!-- Replace content with excerpt only -->
            <!-- <span>{{ post.excerpt }}</span>
            <a href="{{ post.url }}">Read more ..</a> -->
        </li>
    {% endfor %}
</ul>

<p class="rss-subscribe">subscribe <a href="/feed.xml">via RSS</a></p>
