---
title: 使用Jekyll搭建博客(七)
date: 2023-11-06 12:42:46 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
render_with_liquid: false
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客



## Assets

Jekyll可以直接使用CSS、JS、images。把它们放在你的站点文件夹中，它们会复制到构建的站点。

Jekyll网站通常使用这种结构来保持assets的结构：

```
.
├── assets
│   ├── css
│   ├── images
│   └── js
...
```

所以可以在assets文件夹中创建名为css、images和js的文件夹。

此外，直接在根目录下创建另一个名为 `_sass` 的文件夹，您很快就会需要它。

## Sass

在 `_includes/navigation.html` 中使用内联样式不是最佳做法。通过css文件样式化当前页面会更好一些。

要执行此操作，需要在 `navigation.html` 文件中引用这个class，方法是删除之前添加的代码并插入以下代码：

```
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}class="current"{% endif %}>{{ item.name }}</a>
  {% endfor %}
</nav>
```

可以直接使用一个标准CSS文件进行样式化，也可以使用[Sass](https://sass-lang.com/)。Sass是一个很棒的CSS扩展，它直接融入了Jekyll。

首先在 `assets/css/styles.scss` 创建一个Sass文件，内容如下：

```markdown
---
---
@import "main";
```

顶部front matter空的内容告诉Jekyll它需要处理文件。 `@import "main"` 告诉Sass在sass目录（ `_sass/` ）中查找一个名为 `main.scss` 的文件。

在这种情况下只会有一个主css文件。对于较大的项目，这是一个很好的方式来保持你的CSS结构。

创建上面提到的当前class以便将当前链接着色为绿色。

 `_sass/main.scss` 内容如下：

```scss
.current {
  color: green;
}
```

需要在布局中引用样式表,打开 `_layouts/default.html` 并将样式表添加到 `<head>` ：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
    <link rel="stylesheet" href="/assets/css/styles.css">
  </head>
  <body>
    {% include navigation.html %}
    {{ content }}
  </body>
</html>
```

这里引用的 `styles.css` 是由Jekyll在 `assets/css/styles.scss` 生成的css文件。

加载<http://localhost:4000>并检查导航中的活动链接是否为绿色。

