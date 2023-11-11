---
title: Jekyll Assets
date: 2023-11-12 01:56:40 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll内容']
---

> [参考原文](https://jekyllrb.com/docs/assets/)

## Assets

Jekyll提供了对[Sass](https://sass-lang.com/)的内置支持，并可以通过Ruby gem与[CoffeeScript](https://coffeescript.org/)一起工作。为了使用它们，您必须首先创建一个具有正确扩展名（ `.sass` ， `.scss` 或 `.coffee` 之一）的文件，并以两行三重破折号开始文件，如下所示：

```markdown
---
---

// start content
.my-definition
  font-size: 1.2em
```

Jekyll将这些文件视为普通页面，因为输出文件将被放置在它来自的同一目录中。例如，如果您在站点的源文件夹中有一个名为 `css/styles.scss` 的文件，Jekyll将处理它并将其放在站点的目标文件夹中的 `css/styles.css` 下。

>  Jekyll处理Assets文件中的所有Liquid过滤器和标签
如果您使用的是[Mustache](https://mustache.github.io/)或其他与 [Liquid template syntax](https://jekyllrb.com/docs/templates/)语法冲突的JavaScript模板语言，则需要在代码周围放置 `{% raw %}` 和 `{% endraw %}` 标记。
{: prompt-tip }

## Sass/SCSS

Jekyll允许您以某些方式自定义您的Sass转换。

Place all your partials in your `sass_dir`, which defaults to `<source>/_sass`. Place your main SCSS or Sass files in the place you want them to be in the output file, such as `<source>/css`. For an example, take a look at [this example site using Sass support in Jekyll](https://github.com/jekyll/jekyll-sass-converter/tree/master/docs).  
将所有的偏音放在你的 `sass_dir` 中，默认为 `<source>/_sass`。将您的主SCSS或Sass文件放在您希望它们在输出文件中的位置，例如 `<source>/css`。[在Jekyll中使用Sass支持的示例站点](https://github.com/jekyll/jekyll-sass-converter/tree/master/docs)。

如果使用Sass `@import` 语句，则需要确保将 `sass_dir` 设置为包含Sass文件的基目录：

```yaml
sass:
    sass_dir: _sass
```

Sass转换器将 `sass_dir` 配置选项默认为 `_sass` 。

> `sass_dir` 仅用于Sass
请注意， `sass_dir` 将成为Sass导入的加载路径，仅此而已。这意味着Jekyll并不直接知道这些文件。这里的任何文件都不应该包含如上所述的空白封面。如果他们这样做，他们不会像上面描述的那样被转换。此文件夹应仅包含导入。
{: prompt-tip }

您也可以在 `_config.yml` 文件中使用 `style` 选项指定输出样式：
```yaml
sass:
    style: compressed
```

这些都传递给Sass，所以Sass支持的任何输出样式选项在这里也是有效的。

有关Sass配置选项的更多信息，请参阅[Sass配置文档](https://jekyllrb.com/docs/configuration/sass/)。

## Coffeescript

要在Jekyll 3.0及更高版本中启用Coffeescript，您必须

- 安装 `jekyll-coffeescript` gem
- 确保您的 `_config.yml` 是最新的，并包括以下内容：
```yaml
plugins:
  - jekyll-coffeescript
```