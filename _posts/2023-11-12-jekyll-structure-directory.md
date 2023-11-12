---
title: Jekyll Directory
date: 2023-11-12 07:34:58 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll站点结构']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/structure/)



## 目录结构

一个基本的Jekyll网站通常看起来像这样：

```text
.
├── _config.yml
├── _data
│   └── members.yml
├── _drafts
│   ├── begin-with-the-crazy-ideas.md
│   └── on-simplicity-in-technology.md
├── _includes
│   ├── footer.html
│   └── header.html
├── _layouts
│   ├── default.html
│   └── post.html
├── _posts
│   ├── 2007-10-29-why-every-programmer-should-play-nethack.md
│   └── 2009-04-26-barcamp-boston-4-roundup.md
├── _sass
│   ├── _base.scss
│   └── _layout.scss
├── _site
├── .jekyll-cache
│   └── Jekyll
│       └── Cache
│           └── [...]
├── .jekyll-metadata
└── index.html # can also be an 'index.md' with valid front matter
```

> 使用基于gem的主题的Jekyll网站的目录结构
> 自从3.2版本开始，使用 `jekyll new` 引导的新Jekyll项目使用[基于gem的主题](https://jekyllrb.com/docs/themes/)来定义网站的外观。这导致默认的目录结构更轻：默认情况下， `_layouts` 、 `_includes` 和 `_sass` 存储在主题gem中。
{: .prompt-info }


[minima](https://github.com/jekyll/minima)是当前的默认主题， `bundle info minima` 将会显示minima主题文件在您的计算机上的存储位置。


每个的功能概述：

| FILE / DIRECTORY                                          | DESCRIPTION                                                  |
| --------------------------------------------------------- | ------------------------------------------------------------ |
| `_config.yml`                                             | 存储[configuration](https://jekyllrb.com/docs/configuration/) 数据。许多这些选项可以从命令行可执行文件中指定，但在这里指定它们更容易，这样你就不必记住它们。 |
| `_drafts`                                                 | 草稿是未发布的帖子。这些文件的格式没有日期： `title.MARKUP` 。了解如何[work with drafts](https://jekyllrb.com/docs/posts/#drafts)。 |
| `_includes`                                               | 这些是可以由您的布局和帖子混合和匹配以促进重用的部分。液体标签 `{% include file.ext %}` 可以用于在 `_includes/file.ext` 中包含部分。 |
| `_layouts`                                                | 这些是包装帖子的模板。布局是根据每个帖子的[front matter](https://jekyllrb.com/docs/front-matter/)（在下一节中描述）选择的。液体标签 `{{ content }}` 用于将内容注入到网页中。 |
| `_posts`                                                  | 您的动态内容，可以这么说。这些文件的命名约定很重要，必须遵循以下格式： `YEAR-MONTH-DAY-title.MARKUP` 。[permalinks](https://jekyllrb.com/docs/permalinks/) 可以针对每篇文章进行自定义，但日期和标记语言仅由文件名确定。 |
| `_data`                                                   | 格式良好的网站数据应放在这里。Jekyll引擎将自动加载此目录中的所有数据文件（使用 `.yml` 、 `.yaml` 、 `.json` 、 `.csv` 或 `.tsv` 格式和扩展名），并可以通过`site.data`访问它们。如果目录下有一个 `members.yml` 文件，则可以通过 `site.data.members` 访问文件的内容。 |
| `_sass`                                                   | 这些是可以导入到您的 `main.scss` 中的Sass部分，然后将其处理为一个定义了您网站使用的样式的单个样式表 `main.css` 。了解[how to work with assets](https://jekyllrb.com/docs/assets/)。 |
| `_site`                                                   | 这是生成的网站将被放置的位置（默认情况下），一旦Jekyll完成转换。将其添加到您的 `.gitignore` 文件中可能是个好主意。 |
| `.jekyll-cache`                                           | 保留生成的页面和标记的副本（例如：markdown），以便更快地提供服务。在使用时创建，例如： `jekyll serve` 。可以通过[an option and/or flag](https://jekyllrb.com/docs/configuration/options/)禁用。此目录不会包含在生成的网站中。将其添加到您的 `.gitignore` 文件中可能是个好主意。 |
| `.jekyll-metadata`                                        | 这有助于Jekyll跟踪自上次构建站点以来未被修改的文件，以及下一次构建时需要重新生成的文件。仅在使用[incremental regeneration](https://jekyllrb.com/docs/configuration/incremental-regeneration/) 时创建（例如：使用 `jekyll serve -I` ）。此文件不会包含在生成的站点中。将其添加到您的 `.gitignore` 文件中可能是个好主意。 |
| `index.html` or `index.md` and other HTML, Markdown files | 只要文件有一个 [front matter](https://jekyllrb.com/docs/front-matter/) 部分，它将被Jekyll转换。对于站点根目录或未在上述列表中列出的目录中的任何 `.html` ， `.markdown` ， `.md` 或 `.textile` 文件，也将发生相同的情况。 |
| Other Files/Folders                                       | 除了上面列出的特殊情况，其他所有目录和文件（例如 `css` 和 `images` 文件夹， `favicon.ico` 文件等等）都将完全复制到生成的网站中。如果你想了解它们的布局，[已经有很多使用Jekyll的网站](https://jekyllrb.com/showcase/)可以参考。 |


在 `source` 目录中，以以下字符开头的每个文件或目录： `.` ， `_ ` ， `#` 或 `~` ，将不会包含在 `destination` 文件夹中。这样的路径必须通过配置文件中的 `include` 指令明确指定，以确保它们被复制过去。

```yaml
include:
 - _pages
 - .htaccess
```
