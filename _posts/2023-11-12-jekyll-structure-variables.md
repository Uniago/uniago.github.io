---
title: Jekyll Variables
date: 2023-11-12 07:53:09 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll站点结构']
---

> [参考原文](https://jekyllrb.com/docs/variables/)



## 变量

Jekyll遍历您的网站以寻找要处理的文件。带有[front matter](https://jekyllrb.com/docs/front-matter/)元数据的文件都会被处理。对于这些文件，Jekyll通过[Liquid](https://jekyllrb.com/docs/liquid/)提供了各种可用的数据。以下是可用数据的参考。


## 全局变量

| VARIABLE    | DESCRIPTION                                                  |
| ----------- | ------------------------------------------------------------ |
| `site`      | 网站全局信息+配置设置来自 `_config.yml` 。详情请见下方。     |
| `page`      | 页面特定信息+ [front matter](https://jekyllrb.com/docs/front-matter/)。通过front matter设置的自定义变量将在此处可用。有关详细信息，请参见下文。 |
| `layout`    | 布局特定信息+ [front matter](https://jekyllrb.com/docs/front-matter/)。通过布局中的前置内容设置的自定义变量将在此处可用。 |
| `theme`     | 根据主题的gemspec文件定义的主题宝石特定信息。例如，在主题演示的“关于”页面中呈现信息非常有用。有关详细信息，请参见下文。 |
| `content`   | 在布局文件中，包装的帖子或页面的渲染内容。在帖子或页面文件中未定义。 |
| `paginator` | 当设置了 `paginate` 配置选项时，此变量可供使用。有关详细信息，请参阅[Pagination](https://jekyllrb.com/docs/pagination/)。 |



## 站点变量

| VARIABLE                    | DESCRIPTION                                                  |
| --------------------------- | ------------------------------------------------------------ |
| `site.time`                 | 当前时间（运行 `jekyll` 命令时）。                           |
| `site.pages`                | 所有页面的列表。                                             |
| `site.posts`                | A reverse chronological list of all Posts. 所有帖子的逆时间顺序列表。 |
| `site.related_posts`        | 如果正在处理的页面是一个帖子，这个帖子包含最多十个相关的帖子列表。默认情况下，这些是最近的十个帖子。对于高质量但计算速度较慢的结果，请使用 `jekyll` 命令并带上 `--lsi` （[latent semantic indexing](https://en.wikipedia.org/wiki/Latent_semantic_analysis#Latent_semantic_indexing)）选项。还请注意，GitHub Pages在生成网站时不支持 `lsi` 选项。 |
| `site.static_files`         | 所有 [static files](https://jekyllrb.com/docs/static-files/) 的列表（即不由Jekyll的转换器或Liquid渲染器处理的文件）。每个文件有五个属性： `path` ， `modified_time` ， `name` ， `basename` 和 `extname` 。 |
| `site.html_pages`           | 列出`site.pages`中以 `.html` 结尾的子集。                    |
| `site.html_files`           | 列出`site.static_files` 中以 `.html` 结尾的子集。            |
| `site.collections`          | 所有collection的列表（包括page）。                           |
| `site.data`                 | 包含从位于 `_data` 目录中的YAML文件加载的数据的列表。        |
| `site.documents`            | collections中的所有document清单。                            |
| `site.categories.CATEGORY`  | 分类 `CATEGORY` 中的所有帖子列表。                           |
| `site.tags.TAG`             | 所有带有标签 `TAG` 的帖子列表。                              |
| `site.url`                  | 包含您网站的URL，其在 `_config.yml` 中配置。例如，如果您在配置文件中有 `url: http://mysite.com` ，则在Liquid中可以访问为 `site.url` 。对于开发环境，有一个 [例外](https://jekyllrb.com/news/2016/10/06/jekyll-3-3-is-here/#3-siteurl-is-set-by-the-development-server)，如果您在开发环境中运行 `jekyll serve` ，则 `site.url` 将设置为 `host` ， `port` 和SSL相关选项的值。默认为 `url: http://localhost:4000` 。 |
| `site.[CONFIGURATION_DATA]` | 通过命令行和您的 `_config.yml` 设置的所有变量都可以通过 `site` 变量访问。例如，如果您在配置文件中有 `foo: bar` ，那么在Liquid中可以访问它作为 `site.foo` 。Jekyll不会解析 `watch` 模式中 `_config.yml` 的更改，您必须重新启动Jekyll才能看到变量的更改。 |



## 页面变量

| VARIABLE          | DESCRIPTION                                                  |
| ----------------- | ------------------------------------------------------------ |
| `page.content`    | 页面的内容根据正在处理的Liquid和 `page` 的情况，可以呈现或不呈现。 |
| `page.title`      | 页面的标题。                                                 |
| `page.excerpt`    | 未渲染的文档摘录。                                           |
| `page.url`        | 帖子的URL不包含域名，但以斜杠开头，例如 `/2008/12/14/my-post.html` |
| `page.date`       | 帖子的日期。可以通过在帖子的正文中指定一个新的日期/时间来覆盖它，格式为 `YYYY-MM-DD HH:MM:SS` （假设为UTC），或 `YYYY-MM-DD HH:MM:SS +/-TTTT` （使用与UTC的偏移来指定时区，例如 `2008-12-14 10:30:00 +0900` ）。 |
| `page.id`         | 在集合或帖子中唯一标识一个文档的标识符（在RSS订阅中很有用）。例如： `/2008/12/14/my-post`、 `/my-collection/my-document` |
| `page.categories` | 该帖子所属的类别列表。类别是从上述目录结构中派生出来的。例如，位于 `_posts` 目录下的帖子将设置该字段为 `['work', 'code']` 。这些也可以在 [front matter](https://jekyllrb.com/docs/front-matter/)元数据中指定。 |
| `page.collection` | 该文档所属的集合标签。例如，对于一个帖子，标签为 `posts` ，对于路径为 `_puppies/rover.md` 的文档，标签为 `puppies` 。如果不属于任何集合，则返回空字符串。 |
| `page.tags`       | 这篇文章所属的标签列表。可以在[front matter](https://jekyllrb.com/docs/front-matter/)元数据中指定。 |
| `page.dir`        | 源目录和帖子或页面文件之间的路径，例如 `/pages/` 。这可以通过[front matter](https://jekyllrb.com/docs/front-matter/)元数据中的 `permalink` 进行覆盖。 |
| `page.name`       | 帖子或页面的文件名，例如 `about.md`                          |
| `page.path`       | 原始帖子或页面的路径。示例用法：链接回GitHub上页面或帖子的源。这可以在[front matter](https://jekyllrb.com/docs/front-matter/)元数据中被覆盖。 |
| `page.next`       | 下一篇与当前帖子在 `site.posts` 中的位置相关。对于最后一篇，返回 `nil` 。 |
| `page.previous`   | 当前帖子相对于之前的帖子的位置。对于第一条记录返回 `nil` 。  |

> 使用自定义前置内容
您指定的任何自定义前置内容将在 `page` 下可用。例如，如果您在页面的前置内容中指定 `custom_css: true` ，该值将作为 `page.custom_css` 可用。
{ :prompt-warning }

If you specify front matter in a layout, access that via `layout`. For example, if you specify `class: full_page` in a layout’s front matter, that value will be available as `layout.class` in the layout and its parents.
如果您在布局中指定了前置内容，可以通过 `layout` 访问。例如，如果您在布局的前置内容中指定了 `class: full_page` ，那么该值将作为 `layout.class` 在布局及其父级中可用。



## 主题变量

| VARIABLE             | DESCRIPTION                                  |
| -------------------- | -------------------------------------------- |
| `theme.root`         | theme-gem的绝对路径                          |
| `theme.authors`      | 由theme-gem的作者组成的逗号分隔字符串。      |
| `theme.description`  | 在主题gemspec中指定的theme-gem的描述或摘要。 |
| `theme.version`      | 当前主题的版本字符串。                       |
| `theme.dependencies` | 主题的运行时依赖列表。                       |
| `theme.metadata`     | 在主题gemspec中定义的键值对的映射。          |



## 分页器

| VARIABLE                       | DESCRIPTION                              |
| ------------------------------ | ---------------------------------------- |
| `paginator.page`               | 当前页的编号                             |
| `paginator.per_page`           | 每页帖子数量                             |
| `paginator.posts`              | 当前页面上可用的帖子                     |
| `paginator.total_posts`        | 帖子总数                                 |
| `paginator.total_pages`        | 总页数                                   |
| `paginator.previous_page`      | 前一页的页码，如果没有前一页则为 `nil`   |
| `paginator.previous_page_path` | 如果没有上一页，则为 `nil` 的路径        |
| `paginator.next_page`          | 下一页的页码，如果没有下一页则为 `nil`   |
| `paginator.next_page_path`     | 下一页的路径，如果没有后续页面则为 `nil` |

> 分页器变量可用性
这些只能在索引文件中找到，但它们可以位于子目录中，例如 `/blog/index.html` 。
{: .prompt-info }