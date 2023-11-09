---
title: Jekyll-minima主题分析
date: 2023-11-03 09:34:12 +0800
categories: ['Tech', 'Jekyll']
tags: ['学习', 'todo', 'jekyll']
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

