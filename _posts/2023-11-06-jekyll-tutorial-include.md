---
title: 使用Jekyll搭建博客(五)
date: 2023-11-06 12:13:01 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
render_with_liquid: false
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客



## Includes

网站正在整合,然而没有办法在页面之间导航。导航应该在每个页面上，因此将其添加到布局中是正确的位置。但是直接将其添加到布局中，不如将其作为学习includes的机会。



## Include 标签

`include` 标签允许您包含存储在 `_includes` 文件夹中的其他文件的内容。

Includes对于使源代码在站点中重复使用或提高可读性非常有用。

导航源代码可能会变得很复杂，所以有时候将其移动到包含中会更好。



## 使用Include

在 `_includes/navigation.html` 创建一个导航文件，内容如下：

```html
<nav>
  <a href="/">Home</a>
  <a href="/about.html">About</a>
</nav>
```

使用include标签将导航添加到 `_layouts/default.html` ：

```markdown
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    {% include navigation.html %}
    {{ content }}
  </body>
</html>
```

在浏览器中打开<http://localhost:4000>并尝试在页面之间切换。



## 高亮显示按钮

让我们更进一步，在导航中突出显示当前页面。

`_includes/navigation.html` 需要知道它插入的页面的URL，以便添加样式。Jekyll提供了一些有用的[变量](https://jekyllrb.com/docs/variables/) ，其中之一是 `page.url` 。

使用 `page.url` ，你可以检查每个链接是否是当前页面，如果是，则将其涂成红色：

```
<nav>
  <a href="/" {% if page.url == "/" %}style="color: red;"{% endif %}>
    Home
  </a>
  <a href="/about.html" {% if page.url == "/about.html" %}style="color: red;"{% endif %}>
    About
  </a>
</nav>
```

访问<http://localhost:4000>，可以发现当前页面的红色链接按钮。