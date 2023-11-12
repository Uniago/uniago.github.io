---
title: Jekyll Layouts
date: 2023-11-12 08:31:22 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll站点结构']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/layouts/)

## Layouts

布局是包裹在您内容周围的模板。它们允许您将模板的源代码放在一个地方，这样您就不必在每个页面上重复导航和页脚等内容。

布局文件存放在 `_layouts` 目录中。按照惯例，应该有一个名为 `default.html` 的基础模板，并根据需要从这个模板[继承](https://jekyllrb.com/docs/layouts/#inheritance)其他布局。

> 布局目录
Jekyll在您网站的根目录或主题的根目录中寻找 `_layouts` 目录。
虽然您可以通过在配置文件中设置 `layouts_dir` 键来配置布局所在的目录名称，但该目录本身应位于您网站的根目录下的 `source` 目录中。
{: .prompt-waring}

## Usages 使用

第一步是将模板源代码放在 `default.html` 中。 `content` 是一个特殊变量，其值是被包装的帖子或页面的渲染内容。

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
    <nav>
      <a href="/">Home</a>
      <a href="/blog/">Blog</a>
    </nav>
    <h1>{{ page.title }}</h1>
    <section>
      {{ content }}
    </section>
    <footer>
      &copy; to me
    </footer>
  </body>
</html>
```

您可以完全访问原始内容的front matter部分。在上面的示例中， `page.title` 来自页面的front matter部分。

接下来，您需要在页面的前置元数据中指定您使用的布局。您还可以使用[front matter defaults](https://jekyllrb.com/docs/configuration/front-matter-defaults/)，以免在每个页面上都进行设置。

```markdown
---
title: My First Page
layout: default
---

This is the content of my page
```

此页面的渲染输出为：

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>My First Page</title>
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
    <nav>
      <a href="/">Home</a>
      <a href="/blog/">Blog</a>
    </nav>
    <h1>My First Page</h1>
    <section>
      This is the content of my page
    </section>
    <footer>
      &copy; to me
    </footer>
  </body>
</html>
```

## Inheritance 继承

布局继承在您希望为站点上的某些文档部分添加现有布局时非常有用。一个常见的例子是博客文章，您可能希望文章显示日期和作者，但除此之外与基本布局完全相同。

要实现这一点，您需要创建另一个布局，其中在前置元数据中指定您的原始布局。例如，此布局将位于 `_layouts/post.html` ：

```markdown
---
layout: default
---
<p>{{ page.date }} - Written by {{ page.author }}</p>

{{ content }}
```

现在帖子可以使用这个布局，而其他页面则使用默认布局。

## Variables 变量

您可以在布局中设置前置内容，唯一的区别是在使用Liquid时，您需要使用 `layout` 变量而不是 `page` 。例如：

```markdown
---
city: San Francisco
---
<p>{{ layout.city }}</p>

{{ content }}
```