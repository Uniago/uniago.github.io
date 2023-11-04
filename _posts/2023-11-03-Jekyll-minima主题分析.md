---
title: Jekyll-minima主题分析
author: Jan
date: 2023-11-03
---

> 想要引用Jekyll的theme，但不知道是什么原理，现在从这里开始吧，然后再与[Jekyll的分布教程](https://jekyllrb.com/docs/step-by-step/01-setup/)做对比吧!后面会在另一篇博客中分析Jekyll分布教程的源码。

## 目录

*这里的目录开源由tree生成，[参考地址](https://www.cnblogs.com/hsbolg/p/12751820.html)*

```tex
.
├── _includes
│   ├── custom-head.html
│   ├── disqus_comments.html
│   ├── footer.html
│   ├── google-analytics.html
│   ├── head.html
│   ├── header.html
│   ├── social-icons
│   │   ├── devto.svg
│   │   ├── dribbble.svg
│   │   ├── facebook.svg
│   │   ├── flickr.svg
│   │   ├── github.svg
│   │   ├── gitlab.svg
│   │   ├── google_scholar.svg
│   │   ├── instagram.svg
│   │   ├── keybase.svg
│   │   ├── linkedin.svg
│   │   ├── mastodon.svg
│   │   ├── microdotblog.svg
│   │   ├── pinterest.svg
│   │   ├── rss.svg
│   │   ├── stackoverflow.svg
│   │   ├── telegram.svg
│   │   ├── twitter.svg
│   │   ├── x.svg
│   │   └── youtube.svg
│   ├── social-item.html
│   ├── social.html
│   └── svg_symbol.html
├── _layouts	# 布局
│   ├── base.html	# 基础
│   ├── home.html	# 首页
│   ├── page.html	# 页面
│   └── post.html	# 博客
├── _posts
│   ├── 2016-05-19-super-short-article.md
│   ├── 2016-05-20-my-example-post.md
│   ├── 2016-05-20-super-long-article.md
│   ├── 2016-05-20-this-post-demonstrates-post-content-styles.md
│   └── 2016-05-20-welcome-to-jekyll.md
├── _sass
│   └── minima
│       ├── _base.scss
│       ├── _layout.scss
│       ├── custom-styles.scss
│       ├── custom-variables.scss
│       ├── initialize.scss
│       └── skins
│           ├── auto.scss
│           ├── classic.scss
│           ├── dark.scss
│           ├── solarized-dark.scss
│           ├── solarized-light.scss
│           └── solarized.scss
├── assets
│   ├── css
│   │   └── style.scss
│   └── minima-social-icons.liquid
│── script
│   ├── bootstrap
│   ├── build
│   ├── cibuild
│   └── server
├── _config.yml
├── about.md
├── index.md
├── minima.gemspec
├── readme_banner.svg
├── screenshot.png
├── 404.html
├── CODE_OF_CONDUCT.md
├── Gemfile
├── History.markdown
├── LICENSE.txt
└── README.md
```

### 一、布局

```tex
├── _layouts	# 布局
│   ├── base.html	# 基础
│   ├── home.html	# 首页
│   ├── page.html	# 页面
│   └── post.html	# 博客
```

`_layouts` 目录中的文件，用于定义主题的标记

#### base

```html
<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: "en" }}">

  {%- include head.html -%}

  <body>

    {%- include header.html -%}

    <main class="page-content" aria-label="Content">
      <div class="wrapper">
        {{ content }}
      </div>
    </main>

    {%- include footer.html -%}

  </body>

</html>
```

#### home

```html
---
layout: base
---

<div class="home">
  {%- if page.title -%}
    <h1 class="page-heading">{{ page.title }}</h1>
  {%- endif -%}

  {{ content }}


  {% if site.paginate %}
    {% assign posts = paginator.posts %}
  {% else %}
    {% assign posts = site.posts %}
  {% endif %}


  {%- if posts.size > 0 -%}
    {%- if page.list_title -%}
      <h2 class="post-list-heading">{{ page.list_title }}</h2>
    {%- endif -%}
    <ul class="post-list">
      {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
      {%- for post in posts -%}
      <li>
        <span class="post-meta">{{ post.date | date: date_format }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
        {%- if site.show_excerpts -%}
          {{ post.excerpt }}
        {%- endif -%}
      </li>
      {%- endfor -%}
    </ul>

    {% if site.paginate %}
      <div class="pager">
        <ul class="pagination">
        {%- if paginator.previous_page %}
          <li><a href="{{ paginator.previous_page_path | relative_url }}" class="previous-page">{{ paginator.previous_page }}</a></li>
        {%- else %}
          <li><div class="pager-edge">•</div></li>
        {%- endif %}
          <li><div class="current-page">{{ paginator.page }}</div></li>
        {%- if paginator.next_page %}
          <li><a href="{{ paginator.next_page_path | relative_url }}" class="next-page">{{ paginator.next_page }}</a></li>
        {%- else %}
          <li><div class="pager-edge">•</div></li>
        {%- endif %}
        </ul>
      </div>
    {%- endif %}

  {%- endif -%}

</div>
```

#### page

```html
---
layout: base
---
<article class="post">

  <header class="post-header">
    <h1 class="post-title">{{ page.title | escape }}</h1>
  </header>

  <div class="post-content">
    {{ content }}
  </div>

</article>
```



#### post

```html
---
layout: base
---
<article class="post h-entry" itemscope itemtype="http://schema.org/BlogPosting">

  <header class="post-header">
    <h1 class="post-title p-name" itemprop="name headline">{{ page.title | escape }}</h1>
    <p class="post-meta">
      {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
      <time class="dt-published" datetime="{{ page.date | date_to_xmlschema }}" itemprop="datePublished">
        {{ page.date | date: date_format }}
      </time>
      {%- if page.modified_date -%}
        ~ 
        {%- assign mdate = page.modified_date | date_to_xmlschema -%}
        <time class="dt-modified" datetime="{{ mdate }}" itemprop="dateModified">
          {{ mdate | date: date_format }}
        </time>
      {%- endif -%}
      {%- if page.author -%}
        • {% for author in page.author %}
          <span itemprop="author" itemscope itemtype="http://schema.org/Person">
            <span class="p-author h-card" itemprop="name">{{ author }}</span></span>
            {%- if forloop.last == false %}, {% endif -%}
        {% endfor %}
      {%- endif -%}</p>
  </header>

  <div class="post-content e-content" itemprop="articleBody">
    {{ content }}
  </div>

  {%- if site.disqus.shortname -%}
    {%- include disqus_comments.html -%}
  {%- endif -%}

  <a class="u-url" href="{{ page.url | relative_url }}" hidden></a>
</article>
```



