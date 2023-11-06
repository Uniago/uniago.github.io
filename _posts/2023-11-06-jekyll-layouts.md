---
title: 使用Jekyll搭建博客(四)
date: 2023-11-06 10:48:36 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
render_with_liquid: false
typora-root-url: ../
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客



## Layouts

Jekyll在构建页面时除了支持HTML之外还支持Markdown。Markdown对于内容结构简单的页面（只有段落、标题和图像）来说是一个很好的选择，因为它比原始HTML更简洁。

在网站的根文件夹中创建一个名为 `about.md` 的新Markdown文件。

您可以复制 `index` 的内容，并在About页面中对其进行修改。但这会创建重复的代码，必须为您添加到站点的每个新页面进行自定义。
例如，向您的网站添加新样式表将涉及将指向样式表的链接添加到每个页面的 `<head>` 。

对于有很多页面的网站来说，这就是浪费时间。



## 创建Layouts

布局是可以由站点中的任何页面使用页面内容的模板。它们存储在名为 `_layouts` 的目录中。

在站点的根文件夹中创建 `_layouts` 目录，并创建一个包含以下内容的新 `default.html` 文件：

```markdown
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    {{ content }}
  </body>
</html>
```

这个HTML几乎与 `index.html` 相同，除了没有front matter的内容，页面的内容被 `content` 变量取代。

`content` 是一个特殊的变量，它返回调用它的页面的呈现内容。



## 使用Layouts

要使 `index.html` 使用新的布局页面(default.html)，请在front matter的内容中设置 `layout` 变量。文件应该如下所示：

```markdown
---
layout: default
title: Home
---
<h1>{{ "Hello World!" | downcase }}</h1>
```

重新加载站点时，输出保持不变。

由于布局页面default.html包含了页面上的内容，您可以在布局文件中调用front matter，如 `page` 。

当您将布局应用于页面时，它将使用该页面上的正面内容。

下图完美的说明了他们的关系:

![Concept of Jekyll layouts](/assets/images/jekylllayoutconcept.png)



## 创建about页面

将以下内容添加到 `about.md` 以在“about”页面中使用新布局：

```markdown
---
layout: default
title: About
---
# About page

This page tells you a little bit about me.
```

在浏览器中打开<http://localhost:4000/about.html>并查看新页面。
