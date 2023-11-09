---
title: 使用Jekyll搭建博客(三)
date: 2023-11-06 10:28:33 +0800
categories:
  - Tech
  - Jekyll
tags:
  - 建站
render_with_liquid: false
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客

## Front Matter

Front Matter是位于文件开头的两条三重虚线之间的[YAML](https://yaml.org/)片段。

您可以使用front matter为页面设置变量：

```markdown
---
my_number: 5
---
```

在页面中可以使用 `page` 变量在Liquid中调用front matter变量。

例如，要输出上述 `my_number` 变量的值：

```markdown
{{ page.my_number }}
```



## 使用Front Matter

更改您网站上的 `<title>` 以使用front matter：

```
---
title: Home
---
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    <h1>{{ "Hello World!" | downcase }}</h1>
  </body>
</html>
```

您需要在页面上包含front matter，以便Jekyll处理页面上的Liquid标签。

要让Jekyll处理页面并且不带任何变量front matter可以这样做：

```
---
---
```
