---
title: 使用Jekyll搭建博客(八)
date: 2023-11-06 13:02:46 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
render_with_liquid: false
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客



## Blogging

你可能想知道你怎么能有一个没有数据库的博客。在真正的Jekyll风格中，博客只由文本文件驱动。



## Posts

博客文章位于名为 `_posts` 的文件夹中。

文章的文件名有一个特殊的格式：`YYYY-mm-dd-title.md`发布日期，然后是标题，然后是扩展名。

在 `_posts/2018-08-20-bananas.md` 创建您的第一篇文章，内容如下

```markdown
---
layout: post
author: jill
---

A banana is an edible fruit – botanically a berry – produced by several
kinds of large herbaceous flowering plants in the genus Musa.

In some countries, bananas used for cooking may be called "plantains",
distinguishing them from dessert bananas. The fruit is variable in size,
color, and firmness, but is usually elongated and curved, with soft
flesh rich in starch covered with a rind, which may be green, yellow,
red, purple, or brown when ripe.
```

它除了有一个author和不同的layout,其他的非常像之前创建的 `about.md` 。 `author` 是一个自定义变量，它不是必需的，可以命名为类似 `creator` 的东西。



## Layout

`post` 布局不存在，因此您需要使用以下内容在 `_layouts/post.html` 创建它：

```html
---
layout: default
---
<h1>{{ page.title }}</h1>
<p>{{ page.date | date_to_string }} - {{ page.author }}</p>

{{ content }}
```

这是布局继承的一个例子。文章布局输出标题、日期、作者和内容主体，这些内容主体由default布局包装。

还请注意 `date_to_string` 过滤器，它将日期格式化为更好的格式。



## Posts 列表

目前无法导航到博客文章。通常一个博客会有一个页面列出所有的博客，我们也做一个这样的。

Jekyll创建的博客会在 `site.posts` 变量中。

在根目录中创建 `blog.html` ，内容如下：

```html
---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
```

对于这段代码，有几件事需要注意：

- `post.url` 是Jekyll为帖子自动设置的输出路径
- `post.title` 是从文章文件名中提取的，可以通过在front matter中设置 `title` 来覆盖
- `post.excerpt` 默认为内容的第一段



还需要通过主导航导航到此页面。打开 `_data/navigation.yml` 并为博客页面添加一个条目：

```yaml
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
```

## 更多Posts

再加几篇文章：

`_posts/2018-08-21-apples.md`:

```markdown
---
layout: post
author: jill
---
An apple is a sweet, edible fruit produced by an apple tree.

Apple trees are cultivated worldwide, and are the most widely grown
species in the genus Malus. The tree originated in Central Asia, where
its wild ancestor, Malus sieversii, is still found today. Apples have
been grown for thousands of years in Asia and Europe, and were brought
to North America by European colonists.
```

`_posts/2018-08-22-kiwifruit.md`:

```markdown
---
layout: post
author: ted
---
Kiwifruit (often abbreviated as kiwi), or Chinese gooseberry is the
edible berry of several species of woody vines in the genus Actinidia.

The most common cultivar group of kiwifruit is oval, about the size of
a large hen's egg (5–8 cm (2.0–3.1 in) in length and 4.5–5.5 cm
(1.8–2.2 in) in diameter). It has a fibrous, dull greenish-brown skin
and bright green or golden flesh with rows of tiny, black, edible
seeds. The fruit has a soft texture, with a sweet and unique flavor.
```

打开<http://localhost:4000>，查看博客文章