---
title: Jekyll Permalinks
date: 2023-11-12 11:44:03 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll站点结构']
---

> [参考原文](https://jekyllrb.com/docs/permalinks/)


## Permalinks

永久链接是您的页面、帖子或集合的输出路径。它们允许您将源代码的目录结构与输出的目录结构不同。

## Front Matter

设置永久链接的最简单方法是使用前置元数据。您可以在前置元数据中设置 `permalink` 变量为您想要的输出路径。

例如，您的网站上可能有一个位于 `/my_pages/about-me.html` 的页面，您希望输出的URL为 `/about/` 。在页面的前置元数据中，您将设置：

```markdown
---
permalink: /about/
---
```

## Global 全局

在您的网站上为每个页面设置永久链接并不好玩。幸运的是，Jekyll允许您在 `_config.yml` 中全局设置永久链接结构。

要设置全局永久链接，您可以在 `permalink` 中使用变量 `_config.yml` 。您可以使用占位符来获得所需的输出。例如：

```yaml
permalink: /:categories/:year/:month/:day/:title:output_ext
```

请注意，页面和集合（不包括 `posts` 和 `drafts` ）没有时间和类别（对于页面而言，上述 `:title` 等同于 `:basename` ），输出时会忽略这些永久链接样式的方面。

例如，对于 `/:categories/:year/:month/:day/:title:output_ext` 集合的永久链接样式，在页面和集合中变为 `/:title.html` （不包括 `posts` 和 `drafts` ）。

### Placeholders 占位符

这是可用的占位符的完整列表：

| VARIABLE               | DESCRIPTION                                                  |
| ---------------------- | ------------------------------------------------------------ |
| `year`                 | 年份从帖子的文件名中提取，使用四位数字表示。可以通过文档的前置元数据进行覆盖。 |
| `short_year`           | 从文件名中提取年份，不包括世纪部分。（00..99）可以通过文档的前置元数据进行覆盖。 |
| `month`                | 从帖子的文件名中提取月份（01..12）。可以通过文档的前置元数据进行覆盖。 |
| `i_month`              | 从帖子文件名中去掉前导零的月份。可以通过文档的前置元数据进行覆盖。 |
| `short_month`          | 三个字母的月份缩写，例如Jan                                  |
| `long_month`           | 月全名,例如:January                                          |
| `day`                  | 从帖子文件名中获取日期。 （01..31）可以通过文档的前置元数据进行覆盖。 |
| `i_day`                | 从帖子文件名中提取的日期，不包含前导零。可以通过文档的前置元数据进行覆盖。 |
| `y_day`                | 从帖子文件名中提取的一年中的序数日期，前面带有零 (001..366)  |
| `w_year`               | 周年可能与月年相差最多三天，从一月初到十二月底               |
| `week`              | 本年度的周数，从第一周开始，该周大部分天数在一月份（01..53） |
| `w_day`                | 星期几，从星期一开始。(1..7)                                 |
| `short_day`            | 周几的三个字母缩写，例如Sun                                  |
| `long_day`             | 星期几的名称，例如Sunday                                     |
| `hour`                 | 小时，以24小时制表示，从帖子的前置内容中补零(00..23)         |
| `minute`               | 分钟,从帖子的front matter获取。(00..59)                      |
| `second`               | 秒,从帖子的front matter获取。 (00..59)                       |
| `title`                | 文件名中的标题。可以通过文档的前置元数据进行覆盖。保留源文的大小写。 |
| `slug`                 | 从文档文件名生成的简化标题（除了数字和字母以外的任何字符都会被替换为连字符）。可以通过文档的前置元数据进行覆盖。 |
| `categories`           | 此帖子的指定类别。如果一篇帖子有多个类别，Jekyll将创建一个层次结构（例如 `/category1/category2` ）。此外，Jekyll会自动解析URL中的双斜杠，所以如果没有类别存在，它将忽略此项。 |
| `slugified_categories` | 指定的类别为此帖子进行了slugify处理。如果一个类别由多个单词组成，Jekyll会将所有字母转换为小写，并用连字符替换任何非字母数字字符。（例如， `"Work 2 Progress"` 将被转换为 `"work-2-progress"` ）<br />如果一个帖子有多个分类，Jekyll会创建一个层次结构（例如 `/work-2-progress/category2` ）。此外，Jekyll会自动解析URL中的双斜杠，所以如果没有分类存在，它会忽略这个。 |
| `:output_ext`          | 输出文件的扩展名。（默认包含且通常不必要。）                 |

### Built-in formats 内置格式

对于文章，Jekyll还提供了以下内置样式以方便使用：

| PERMALINK STYLE | URL TEMPLATE                                                 |
| --------------- | ------------------------------------------------------------ |
| `date`          | `/:categories/:year/:month/:day/:title:output_ext`           |
| `pretty`        | `/:categories/:year/:month/:day/:title/`                     |
| `ordinal`       | `/:categories/:year/:y_day/:title:output_ext`                |
| `weekdate`      | `/:categories/:year/W:week/:short_day/:title:output_ext` (将 `W` 添加到 `:week` 的值前面) |
| `none`          | `/:categories/:title:output_ext`                             |


不要输入 `permalink: /:categories/:year/:month/:day/:title/` ，你可以直接输入 `permalink: pretty` 。

> 通过前置元数据指定永久链接
> 前置元数据中无法识别内置的永久链接样式。因此， `permalink: pretty` 将无法工作。
 {: .prompt-info }



### Collections 集合

对于集合（包括 `posts` 和 `drafts` ），您可以选择在 `_config.yml` 中的集合配置中覆盖全局永久链接

```yaml
collections:
  my_collection:
    output: true
    permalink: /:collection/:name
```


集合有以下可用的占位符：

| VARIABLE      | DESCRIPTION                                                  |
| ------------- | ------------------------------------------------------------ |
| `:collection` | 包含集合的标签。                                             |
| `:path`       | 相对于集合目录的文档路径，包括文档的基本文件名。             |
| `:name`       | 文档的基本文件名，将每个空格和非字母数字字符的序列替换为连字符。 |
| `:title`      | `:title` 模板变量将采用文档中存在的 `slug` [front matter](https://jekyllrb.com/docs/front-matter/) 变量值；如果未定义，则 `:title` 将等同于 `:name` ，即从文件名生成的slug。保留源文的大小写。 |
| `:output_ext` | 输出文件的扩展名。（默认情况下已包含且通常不必要。）         |

### Pages 页面

对于页面，您必须使用前置数据来覆盖全局永久链接，如果您通过前置数据默认设置永久链接，则会被忽略。

页面有以下可用的占位符：

| VARIABLE      | DESCRIPTION                                          |
| ------------- | ---------------------------------------------------- |
| `:path`       | 相对于网站源目录的页面路径，不包括页面的基本文件名。 |
| `:basename`   | 页面的基本文件名                                     |
| `:output_ext` | 输出文件的扩展名。（默认情况下已包含且通常不必要。） |
