---
title: 使用Jekyll搭建博客(二)
date: 2023-11-05 13:36:01 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
render_with_liquid: false
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客



## Liquid

Liquid是一种模板语言，有三个主要组成部分：

- [objects 对象](#objects-对象)
- [tags 标签](#tags-标签)
- [filters 过滤器](#filters-过滤器)



## Objects 对象

对象告诉Liquid将预定义的变量作为页面上的内容输出。对对象使用双花括号：`{{`和`}}`  。

例如， `{{page.title}}` 显示 `page.title` 变量。



## Tags 标签

标记定义模板的逻辑和控制流。使用花括号和百分号标记： `{%` 和 `%}` 。

例如：


```
{% if page.show_sidebar %}
  <div class="sidebar">
    sidebar content
  </div>
{% endif %}
```


如果 `show_sidebar` page变量的值为true，则会显示侧边栏。

更多关于Jekyll中可用的[标签](https://jekyllrb.com/docs/liquid/tags/)。

​	

## Filters 过滤器

过滤器更改Liquid对象的输出。它们在输出中使用，并由 `|` 分隔。

例如：

```ruby
{{ "hi" | capitalize }}
```

显示 `Hi` 而不是 `hi` 。
更多关于可用过滤器的[信息](https://jekyllrb.com/docs/liquid/filters/) 。



## 使用Liquid

现在，使用Liquid修改 `Hello World!` 文本：

```ruby
...
<h1>{{ "Hello World!" | downcase }}</h1>
...
```

要让Jekyll处理您的更改，请在页面顶部添加front matter：

```markdown
---
# front matter tells Jekyll to process Liquid
---
```

你的HTML文档应该看起来像这样：

```html
---
---

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Home</title>
  </head>
  <body>
    <h1>{{ "Hello World!" | downcase }}</h1>
  </body>
</html>
```

当您重新加载浏览器时，您应该会看到 `hello world!` 。

Jekyll的大部分来自于将Liquid与其他功能相结合。在页面上添加frontmatter，让Jekyll处理这些页面上的Liquid。