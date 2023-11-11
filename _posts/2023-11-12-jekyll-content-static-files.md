---
title: Jekyll Static Files
date: 2023-11-12 02:04:11 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll内容']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/static-files/)


# Static Files

静态文件是不包含任何封面内容的文件。其中包括图像、PDF和其他未呈现的内容。
它们可以通过 `site.static_files` 在Liquid中访问，并包含以下元数据：

| VARIABLE             | DESCRIPTION                                          |
| -------------------- | ---------------------------------------------------- |
| `file.path`          | 文件的相对路径，例如 `/assets/img/image.jpg`         |
| `file.modified_time` | 上次修改文件的时间，例如 `2016-04-01 16:35:26 +0200` |
| `file.name`          | 文件的字符串名称，例如 `image.jpg` 代表 `image.jpg`  |
| `file.basename`      | 文件的字符串基础名称，例如 `image` 表示 `image.jpg`  |
| `file.extname`       | 文件的扩展名，例如 `image.jpg` 为 `.jpg`             |

请注意，在上表中， `file` 可以是任何值。它是一个在你自己的逻辑中使用的任意设置的变量（比如在for循环中）。它不是一个全局站点或页面变量。

## 为静态文件添加Front Matter

虽然不能直接向静态文件中添加Front Matter值，但可以通过配置文件中的 [defaults property](https://jekyllrb.com/docs/configuration/front-matter-defaults/)设置Front Matter值。当Jekyll构建站点时，它将使用您设置的front matter值。

下面是一个例子：

在 `_config.yml` 文件中，将以下值添加到 `defaults` 属性：

```yaml
defaults:
  - scope:
      path: "assets/img"
    values:
      image: true
```

这假设您的Jekyll站点的文件夹路径为 `assets/img` ，您在其中存储了图像（静态文件）。当Jekyll构建网站时，它会将每个图像视为具有 `image: true` 的frontmatter值。

假设您想要列出 `assets/img` 中包含的所有图像资产。你可以使用这个for循环来查看 `static_files` 对象，并获取所有具有这个front matter属性的静态文件：

```
{% assign image_files = site.static_files | where: "image", true %}
{% for myimage in image_files %}
  {{ myimage.path }}
{% endfor %}
```

当您构建站点时，输出将列出满足此前提条件的每个文件的路径。
