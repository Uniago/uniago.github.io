---
title: Jekyll增量加载配置
date: 2023-11-09 22:24:53 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']
render_with_liquid: false
---



> [参考原文](https://jekyllrb.com/docs/configuration/incremental-regeneration/)



## 增量加载

> 增量加载仍然是一个实验性的功能
>
> 虽然热加载属于非常常见的，但它不会在每个场景中都正确工作。请谨慎使用该功能，如果遇到问题,请通过在[GitHub上打开一个问题](https://github.com/jekyll/jekyll/issues/new)来报告未列出的问题。
{: .prompt-danger }

热加载仅生成自上次生成以来更新的文档和页面，有助于缩短生成时间。它通过跟踪文件修改时间和 `.jekyll-metadata` 文件中的文档间依赖关系来实现这一点。

在当前实现下，热加载仅在文档或页面或其依赖项之一被修改时才生成文档或页面。目前，跟踪的依赖项类型只有包含（使用 `{% include %}` 标记）和布局。这意味着对其他文档的普通引用（例如，在帖子列表页面中迭代 `site.posts` 的常见情况）将不会被检测为依赖项。

为了弥补其中的一些不足，将 `regenerate: true` 放在文档的前面，将迫使Jekyll重新生成它，而不管它是否被修改过。请注意，这将只生成指定的文档;对其他文档内容的引用将不起作用，因为它们不会被重新加载。

可以通过命令行中的 `--incremental` 标志（简称 `-I` ）或在配置文件中设置 `incremental: true` 来启用增量加载。
