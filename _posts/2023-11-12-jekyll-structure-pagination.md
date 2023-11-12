---
title: Jekyll Pagination
date: 2023-11-12 12:23:13 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll站点结构']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/pagination/)



## Pagination 分页

对于许多网站，尤其是博客，将主要的帖子列表分成较小的列表，并在多个页面上显示它们是非常常见的。Jekyll提供了一个分页插件，因此您可以自动生成所需的文件和文件夹，以用于分页列表。

对于Jekyll 3或更高版本，请在Gemfile中包含 `jekyll-paginate` 插件，并在 `_config.yml` 下的 `plugins` 中包含。对于Jekyll 2，这是标准配置。

> 分页仅在HTML文件内起作用
> 从您的Jekyll网站的Markdown文件中无法进行分页。当从HTML文件中调用时，分页可以正常工作，该文件名为 `index.html` ，可选择地位于子目录中，并通过 `paginate_path` 配置值生成分页。
{: .prompt-info }

## 启用分页

要在您的博客上启用分页功能，只需在 `_config.yml` 文件中添加一行代码，指定每页显示多少条内容

```yaml
paginate: 5
```

该数字应该是您希望在生成的网站中每页显示的最大帖子数量。

您还可以指定分页页面的目标位置：

```yaml
paginate_path: "/blog/page:num/"
```

这将在 `blog/index.html` 中读取，将其作为 `paginator` 发送到Liquid中的每个分页页面，并将输出写入 `blog/page:num/` ，其中 `:num` 是分页页面的页码，从 `2` 开始。
	
如果一个网站有12篇文章，并指定 `paginate: 5` ，Jekyll将把前5篇文章写入 `blog/index.html` ，接下来的5篇文章写入 `blog/page2/index.html` ，最后2篇文章写入 `blog/page3/index.html` 到目标目录中。

> 不要设置永久链接
在博客页面的前置元数据中设置永久链接将导致分页功能失效。只需省略永久链接即可。
{: .prompt-danger }

> 分类、标签和集合的分页
较新的[jekyll-paginate-v2](https://github.com/sverrirs/jekyll-paginate-v2)插件支持更多功能。请参阅存储库中的[分页示例](https://github.com/sverrirs/jekyll-paginate-v2/tree/master/examples)。此插件不受GitHub Pages支持。
{: .prompt-info }


## 可用的Liquid属性

分页插件公开了 `paginator` Liquid对象，具有以下属性：

| VARIABLE                  | DESCRIPTION                                             |
| ------------------------------ | ------------------------------------------------------------ |
| `paginator.page`               | 当前页的编号                  |
| `paginator.per_page`           | 每页帖子数量                        |
| `paginator.posts`              | 当前页面上可用的帖子    |
| `paginator.total_posts`        | 帖子总数                               |
| `paginator.total_pages`        | 总页数                                 |
| `paginator.previous_page`      | 前一页的页码，如果没有前一页则为 `nil` |
| `paginator.previous_page_path` | 如果没有上一页，则为 `nil` 的路径 |
| `paginator.next_page`          | 下一页的页码，如果没有下一页则为 `nil` |
| `paginator.next_page_path`     | 下一页的路径，如果没有后续页面则为 `nil` |

> 分页不支持标签或分类
分页通过 `posts` 变量中的每篇文章，除非一篇文章在其前置数据中有 `hidden: true` 。目前它不允许对由共同标签或分类链接的一组文章进行分页。它不能包括任何文档集合，因为它仅限于文章。
{: .prompt-info }


## 呈现分页的帖子

下一步，您需要使用现在可用的 `paginator` 变量来实际显示您的帖子列表。您可能希望在您网站的主要页面之一中完成此操作。以下是在HTML文件中呈现分页帖子的简单方法的一个示例：

```
---
layout: default
title: My Blog
---

<!-- This loops through the paginated posts -->
{% for post in paginator.posts %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p class="author">
    <span class="date">{{ post.date }}</span>
  </p>
  <div class="content">
    {{ post.content }}
  </div>
{% endfor %}

<!-- Pagination links -->
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path }}" class="previous">
      Previous
    </a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number ">
    Page: {{ paginator.page }} of {{ paginator.total_pages }}
  </span>
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path }}" class="next">Next</a>
  {% else %}
    <span class="next ">Next</span>
  {% endif %}
</div>
```

> 小心第一页的边缘情况
Jekyll不会生成一个名为'page1'的文件夹，所以上述代码在生成 `/page1` 链接时将无法工作。如果这对你来说是个问题，可以参考下面的方法来处理。
{: .prompt-danger }

以下HTML片段应该处理第一页，并呈现一个包含所有页面链接的列表，除了当前页面。

```
{% if paginator.total_pages > 1 %}
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path | relative_url }}">&laquo; Prev</a>
  {% else %}
    <span>&laquo; Prev</span>
  {% endif %}

  {% for page in (1..paginator.total_pages) %}
    {% if page == paginator.page %}
      <em>{{ page }}</em>
    {% elsif page == 1 %}
      <a href="{{ '/' | relative_url }}">{{ page }}</a>
    {% else %}
      <a href="{{ site.paginate_path | relative_url | replace: ':num', page }}">{{ page }}</a>
    {% endif %}
  {% endfor %}

  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path | relative_url }}">Next &raquo;</a>
  {% else %}
    <span>Next &raquo;</span>
  {% endif %}
</div>
{% endif %}
```
