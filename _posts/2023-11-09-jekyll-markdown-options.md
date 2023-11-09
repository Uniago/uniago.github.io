---
title: Jekyll Markdown配置
date: 2023-11-09 21:48:15 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']
---



> [参考原文](https://jekyllrb.com/docs/configuration/markdown/)



## Markdown Options Markdown

Jekyll支持的各种Markdown渲染器有时会提供额外的选项。



## Kramdown

Kramdown是Jekyll的默认Markdown渲染器，通常无需额外配置即可正常工作。但是，它确实支持许多配置选项。



### Kramdown处理器

By default, Jekyll uses the [GitHub Flavored Markdown (GFM) processor](https://github.com/kramdown/parser-gfm) for Kramdown. (Specifying `input: GFM` is fine, but redundant.) GFM supports a couple additional Kramdown options, documented by [kramdown-parser-gfm](https://github.com/kramdown/parser-gfm). These options can be used directly in your Kramdown Jekyll config, like this:
默认情况下，Jekyll为Kramdown使用[GitHub Flavored Markdown (GFM) processor](https://github.com/kramdown/parser-gfm) 。GFM支持一些额外的Kramdown选项[kramdown-parser-gfm](https://github.com/kramdown/parser-gfm)。这些选项可以直接在Kramdown Jekyll配置中使用，如下所示：

```yaml
kramdown:
  gfm_quirks: [paragraph_end]
```

您还可以更改Kramdown使用的处理器（如[Kramdown RDoc](https://kramdown.gettalong.org/rdoc/Kramdown/Document.html#method-c-new)中的 `input` 键所指定的）。例如，要在Jekyll中使用非GFM Kramdown处理器，请将以下内容添加到配置中。

```yaml
kramdown:
  input: Kramdown
```

Kramdown解析器的文档可以在[Kramdown docs](https://kramdown.gettalong.org/parser/kramdown.html)中找到。如果您使用Kramdown或GFM以外的Kramdown解析器，则需要为其添加gem。



### 语法高亮 (CodeRay)

要在Kramdown中使用[CodeRay](http://coderay.rubychan.de/)语法高亮器，您需要添加对 `kramdown-syntax-coderay` gem的依赖。例如， `bundle add kramdown-syntax-coderay` 。然后，您将能够在 `syntax_highlighter` 配置中指定CodeRay：

```yaml
kramdown:
  syntax_highlighter: coderay
```

CodeRay支持几个自己的配置选项，记录在[kramdown-syntax-coderay docs](https://github.com/kramdown/syntax-coderay) 文档，可以像这样传递为 `syntax_highlighter_opts` ：

```yaml
kramdown:
  syntax_highlighter: coderay
  syntax_highlighter_opts:
    line_numbers: table
    bold_every: 5
```



### 高级Kramdown选项

Kramdown支持各种其他相对高级的选项，如 `header_offset` 和 `smart_quotes` 。这些在[Kramdown configuration documentation](https://kramdown.gettalong.org/options.html) 中有记录，可以像这样添加到您的Kramdown配置中：

```yaml
kramdown:
  header_offset: 2
```

> 有几个不受支持的kramdown选项
>
> 请注意，Jekyll使用Kramdown的HTML转换器。仅由其他转换器使用的Kramdown选项，例如 `remove_block_html_tags` （由RemoveHtmlTags转换器使用）将不起作用。
{: .prompt-danger }



## CommonMark

[CommonMark](https://commonmark.org/) 是Markdown语法的合理化版本，在C中实现，因此比Ruby中实现的默认Kramdown更快。它与原始的Markdown[略有不同](https://github.com/commonmark/CommonMark#differences-from-original-markdown) ，并且不支持Kramdown中实现的所有语法元素，如[块内联属性列表](https://kramdown.gettalong.org/syntax.html#block-ials)。

It comes in two flavors: basic CommonMark with [jekyll-commonmark](https://github.com/jekyll/jekyll-commonmark) plugin and [GitHub Flavored Markdown supported by GitHub Pages](https://github.com/github/jekyll-commonmark-ghpages).
它有两种风格：带有[jekyll-commonmark](https://github.com/jekyll/jekyll-commonmark) 插件的基本CommonMark和由GitHub Pages支持的[GitHub Flavorite Markdown](https://github.com/github/jekyll-commonmark-ghpages)。



### 自定义Markdown处理器

创建一个自定义的markdown处理器，可以在 `Jekyll::Converters::Markdown` 命名空间中创建一个新类：

```ruby
class Jekyll::Converters::Markdown::MyCustomProcessor
  def initialize(config)
    require 'funky_markdown'
    @config = config
  rescue LoadError
    STDERR.puts 'You are missing a library required for Markdown. Please run:'
    STDERR.puts '  $ [sudo] gem install funky_markdown'
    raise FatalException.new("Missing dependency: funky_markdown")
  end

  def convert(content)
    ::FunkyMarkdown.new(content).convert
  end
end
```

将其正确设置为 `_plugins` 文件夹中的插件或gem，需要在你的 `_config.yml` 中指定它：

```yaml
markdown: MyCustomProcessor
```