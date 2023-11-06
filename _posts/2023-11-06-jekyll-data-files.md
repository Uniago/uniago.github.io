---
title: 使用Jekyll搭建博客(六)
date: 2023-11-06 12:34:44 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
render_with_liquid: false
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客



## Data Files

Jekyll可以从 `_data` 目录中加载YAML、JSON和CSV数据文件。数据文件是将内容与源代码分离的好方法，使网站更易于维护。

在这一步中，您将把导航的内容存储在一个数据文件中，然后在导航包含中覆盖它。



## 使用数据文件

[YAML](http://yaml.org/) 是Ruby生态系统中常见的格式。您可以使用它来存储一个导航项数组，每个导航项都有一个名称和链接。

在 `_data/navigation.yml` 创建导航数据文件，内容如下：

```yaml
- name: Home
  link: /
- name: About
  link: /about.html
```

Jekyll在 `site.data.navigation` 上为您提供此数据文件,而不是像像上篇文章一般输出每个链接在 `_includes/navigation.html`

现在你可以使用数据文件 ：

```
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}style="color: red;"{% endif %}>
      {{ item.name }}
    </a>
  {% endfor %}
</nav>
```

输出将完全相同。不同之处在于，您可以更轻松地添加新的导航项和更改HTML结构。

