---
title: 使用Jekyll搭建博客(九)
date: 2023-11-06 13:26:34 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
render_with_liquid: false
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客



## Collections

让我们来充实下作者，让每个作者都有自己的页面，上面有简介和他们发表的文章。

要做到这一点，将会使用到collections。collections与posts类似，只是内容不必按日期分组。



## Configuration 配置

要设置一个collection需要在 `_config.yml` 配置文件中告诉Jekyll。

在根目录中创建 `_config.yml` ，并使用以下命令：

```yaml
collections:
  authors:
```

在终端中按 `Ctrl` + `C` 停止服务器，然后按 `jekyll serve` 重新启动服务器。



## 添加Authors

文档（collections中的项目）位于站点根目录中名为 `_*collection_name*` 的文件夹中。在此例中为 `_authors` 。

为每个作者创建一个文档,保存staff信息：

`_authors/jill.md`:

```markdown
---
short_name: jill
name: Jill Smith
position: Chief Editor
---
Jill is an avid fruit grower based in the south of France.
```

`_authors/ted.md`:

```markdown
---
short_name: ted
name: Ted Doe
position: Writer
---
Ted has been eating fruit since he was baby.
```



## Staff 页面

再添加一个页面，列出网站上的所有作者。Jekyll在 `site.authors` 上提供了该集合。

创建 `staff.html` 并遍历 `site.authors` 输出所有staff信息：

```html
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
  {% for author in site.authors %}
    <li>
      <h2>{{ author.name }}</h2>
      <h3>{{ author.position }}</h3>
      <p>{{ author.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>
```

由于内容是markdown，您需要通过 `markdownify` 过滤器运行它。在布局中会自动使用 `{{ content }}` 输出。

通过主导航导航到此页面。打开 `_data/navigation.yml` 并为staff页面添加一个条目：

```yaml
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
- name: Staff
  link: /staff.html
```



## 输出页面

默认情况下，集合不输出文档的页面。在这种情况下，我们希望每个作者都有自己的页面，所以让我们调整集合配置。

打开 `_config.yml` 并将 `output: true` 添加到作者集合配置中：

```yaml
collections:
  authors:
    output: true
```

重新启动jekyll服务器使配置更改生效。

将 `author.url` 链接到 `staff.html` 页：

```html
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
  {% for author in site.authors %}
    <li>
      <h2><a href="{{ author.url }}">{{ author.name }}</a></h2>
      <h3>{{ author.position }}</h3>
      <p>{{ author.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>
```

就像文章一样，需要为作者创建一个布局。

使用以下内容创建 `_layouts/author.html` ：

```html
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}
```



##  默认Front matter设置

现在您需要配置author文档以使用 `author` 布局。可以像以前那样在前面的内容中这样做，但这会变得重复。

想要让所有的文章都自动拥有post布局，作者拥有author的布局，其他一切都使用默认值。可以通过使用 `_config.yml` 中的 [front matter defaults](https://jekyllrb.com/docs/configuration/front-matter-defaults/)来实现设置一个默认值适用的范围。

将布局的默认值添加到 `_config.yml` ，

```yaml
collections:
  authors:
    output: true

defaults:
  - scope:
      path: ""
      type: "authors"
    values:
      layout: "author"
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "post"
  - scope:
      path: ""
    values:
      layout: "default"
```

现在可以从所有页面和帖子的首页中删除layout。请注意，任何时候更新 `_config.yml` 都需要重新启动Jekyll才能使更改生效。



## 列出作者的文章

列出作者在其页面上发布的帖子。要做到这一点，需要将作者 `short_name` 与帖子 `author` 相匹配。您可以使用它来按author过滤帖子。

在 `_layouts/author.html` 中迭代这个过滤列表以输出作者的帖子：

```html
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}

<h2>Posts</h2>
<ul>
  {% assign filtered_posts = site.posts | where: 'author', page.short_name %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
```



## 链接到作者页面

这些帖子都有作者的引用，所以可以将其链接到作者的页面。

可以在 `_layouts/post.html` 中使用类似的过滤技术来完成此操作：

```html
---
layout: default
---
<h1>{{ page.title }}</h1>

<p>
  {{ page.date | date_to_string }}
  {% assign author = site.authors | where: 'short_name', page.author | first %}
  {% if author %}
    - <a href="{{ author.url }}">{{ author.name }}</a>
  {% endif %}
</p>

{{ content }}
```

打开<http://localhost:4000>，查看staff页面和author在帖子上的链接，检查所有内容是否正确链接在一起。