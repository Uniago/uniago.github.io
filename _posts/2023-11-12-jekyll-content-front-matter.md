---
title: Jekyll Front Matter
date: 2023-11-12 01:11:35 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll内容']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/front-matter/)

## Front Matter

任何包含[YAML](https://yaml.org/) front matter块的文件都将被Jekyll作为特殊文件处理。front matter的内容必须是文件中的第一个内容，并且必须采用三虚线之间的有效YAML集的形式。下面是一个基本的例子：

```markdown
---
layout: post
title: Blogging Like a Hacker
---
```

在这三条虚线之间，您可以设置预定义的变量（参见下面的参考），甚至可以创建自己的自定义变量。然后，这些变量将可供您使用Liquid标签访问，无论是在文件中的更深处，还是在任何布局中，或者在有问题的页面或帖子所依赖的包含中。

> UTF-8字符编码警告
如果你使用UTF-8编码，请确保你的文件中没有 `BOM` 头字符，否则Jekyll会发生非常非常糟糕的事情。如果你在[Jekyll on Windows](https://jekyllrb.com/docs/installation/windows/)，这一点尤其重要。
{: .prompt-danger }

>  Front Matter变量是可选的

如果你想使用[Liquid标签和变量](https://jekyllrb.com/docs/variables/)，但不需要任何东西在你的Front Matter，只是让它空！这组中间没有任何东西的三点划线仍然会让Jekyll处理你的文件。(This对于CSS和RSS提要之类的东西很有用！）

## 预定义全局变量

有许多预定义的全局变量，您可以在页面或文章的Front Matter中设置。

|VARIABLE|DESCRIPTION|
|---|---|
|`layout`|如果设置，则指定要使用的布局文件。使用布局文件名而不使用文件扩展名。布局文件必须放在 `_layouts` 目录中。    使用 `null` 将生成一个不使用布局文件的文件。如果文件是post/document并且在 [front matter defaults](https://jekyllrb.com/docs/configuration/front-matter-defaults/).中定义了布局，则会覆盖此选项。<br>从3.5.0版本开始，在post/document中使用 `none` 将生成一个不使用布局文件的文件，而不管前面的默认值如何。在页面中使用 `none` 会导致Jekyll尝试使用名为“none”的布局。|
|`permalink`|如果你需要你处理的博客文章URL是站点范围样式（默认为 `/year/month/day/title.html` ）以外的样式，那么你可以设置这个变量，它将被用作最终的URL。|
|`published`|如果您不希望在生成站点时显示特定的帖子，请设置为false。|

> 渲染标记为未发布的帖子
要预览未发布的页面，请使用`--unpublished`开关运行`jekyll serve`或`jekyll build`。Jekyll也有一个方便的[drafts](https://jekyllrb.com/docs/posts/#drafts)功能，专门为博客文章量身定制。
{: .prompt-warning }

## 自定义变量

您还可以设置您自己的前体变量，您可以在Liquid中访问。例如，如果你设置了一个名为 `food` 的变量，你可以在你的页面中使用它：

```markdown
---
food: Pizza
---

<h1>{{ page.food }}</h1>
```

## 帖子的预定义变量

这些都是开箱即用的，可以用在文章的前面。

|VARIABLE|DESCRIPTION|
|---|---|
|`date`|这里的日期将覆盖文章名称中的日期。这可以用来确保邮件的正确分类。日期以 `YYYY-MM-DD HH:MM:SS +/-TTTT` 格式指定;小时、分钟、秒和时区偏移是可选的。|
|`category`<br><br>`categories`|您可以指定文章所属的一个或多个类别，而不是将文章放置在文件夹中。当网站生成后，该职位将作为，好像它已与这些类别设置正常。类别（复数键）可以指定为 [YAML list](https://en.wikipedia.org/wiki/YAML#Basic_components) 或空格分隔的字符串。|
|`tags`|与类别类似，一个或多个标签可以添加到文章中。也像类别一样，标签可以指定为 [YAML list](https://en.wikipedia.org/wiki/YAML#Basic_components) 或空格分隔的字符串。|

> 不要重复定义

如果你不想一遍又一遍地重复你经常使用的前台变量，为它们定义默认值，只在必要的时候覆盖它们（或者根本不覆盖）。这对预定义变量和自定义变量都有效。