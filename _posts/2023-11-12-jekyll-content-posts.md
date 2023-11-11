---
title: Jekyll Posts
date: 2023-11-12 00:41:18 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll内容']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/posts/)


## Posts

博客在Jekyll中,你把博客文章写成文本文件，Jekyll提供了你把它变成博客所需的一切。

## Posts文件夹

`_posts` 文件夹是您的博客文章所在的位置。可以用[Markdown](https://daringfireball.net/projects/markdown/)写文章，也支持HTML。

##  创建Posts

要创建帖子，请使用以下格式将文件添加到您的 `_posts` 目录：

```text
YEAR-MONTH-DAY-title.MARKUP
```

其中 `YEAR` 是四位数， `MONTH` 和 `DAY` 都是两位数， `MARKUP` 是表示文件中使用的格式的文件扩展名。例如，以下是有效的post文件名的示例：

```text
2011-12-31-new-years-eve-is-awesome.md
2012-09-12-how-to-write-a-blog.md
```

所有博客文章文件必须以[front matter](https://jekyllrb.com/docs/front-matter/)开头，这是通常用于设置布局或其他Meta数据。如果没有内容,它可以是空的.

```markdown
---
layout: post
title:  "Welcome to Jekyll!"
---

# Welcome

**Hello world**, this is my first Jekyll blog post.

I hope you like it!
```


> 链接到其他帖子
使用 [`post_url`](https://jekyllrb.com/docs/liquid/tags/#linking-to-posts) 标签链接到其他帖子，而不必担心当网站永久链接样式更改时URL会中断。
{: .prompt-tip }

> 注意字符集
内容处理器可以修改某些字符，使它们看起来更漂亮。例如，Redcarpet中的 `smart` 扩展将标准的ASCII引号字符转换为卷曲的Unicode字符。为了让浏览器正确显示这些字符，请通过在布局的 `<head>` 中包含 `<meta charset="utf-8">` 来定义字符集Meta值。
{: .prompt-info }

##  图片和资源

在某些时候，您可能希望在文本内容中包含图像、下载内容或其他数字资产沿着。一个常见的解决方案是在项目目录的根目录中创建一个名为 `assets` 的文件夹，其中放置任何图像，文件或其他资源。然后，从任何帖子中，可以使用站点的根目录作为要包含的资产的路径来链接它们。最好的方法取决于您的网站的（子）域和路径的配置方式，但这里有一些简单的Markdown示例：

在帖子中包含图像资源：

```markdown
... which is shown in the screenshot below:
![My helpful screenshot](/assets/screenshot.jpg)
```

链接到PDF供读者下载：

```markdown
... you can [get the PDF](/assets/mydoc.pdf) directly.
```

##  展示Posts索引

由于[Liquid](https://shopify.github.io/liquid/)及其标签，在另一个页面上创建帖子索引应该很容易。下面是一个简单的例子，说明如何创建指向博客文章的链接列表：

```
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
```

您可以完全控制如何（以及在哪里）显示您的帖子，以及[how templates work](https://jekyllrb.com/docs/templates/)。如果你想了解更多，你应该阅读更多关于模板如何与Jekyll一起工作的信息。

请注意， `post` 变量只存在于上面的 `for` 循环中。如果你想访问当前呈现的页面/文章的变量（包含 `for` 循环的文章/页面的变量），请使用 `page` 变量。

## 标签和类别

Jekyll对博客文章中的标签和类别提供了一流的支持。

### Tags 标签

文章的标签在文章的前页中定义，使用键 `tag` 表示单个条目，或使用键 `tags` 表示多个条目。  
由于Jekyll期望多个项映射到键 `tags` ，因此如果包含空格，它将自动拆分字符串条目。例如，虽然前体 `tag: classic hollywood` 将被处理成单个实体 `"classic hollywood"` ，但前体 `tags: classic hollywood` 将被处理成条目 `["classic", "hollywood"]` 的阵列。

不管选择的是什么样的front matter，Jekyll都会存储映射到复数键的元数据，该复数键会暴露给Liquid模板。

所有在当前站点注册的标签都通过 `site.tags` 暴露给Liquid模板。在页面上迭代 `site.tags` 将产生另一个包含两个项目的数组，其中第一个项目是标签的名称，第二个项目是具有该标签的帖子的数组。

```
{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
```

### 类别

文章的类别与上面的标签类似：

- 它们可以通过使用键 `category` 或 `categories` （遵循与标记相同的逻辑）的前体来定义。
- 在网站中注册的所有类别都通过 `site.categories` 暴露给Liquid模板，可以迭代（类似于上面的标签循环）。

与标签不同，文章的类别也可以通过文章的文件路径来定义。任何高于 `_posts` 的目录都将被读入为类别。例如，如果帖子位于路径 `movies/horror/_posts/2019-05-21-bride-of-chucky.markdown` ，则 `movies` 和 `horror` 会自动注册为该帖子的类别。

当文章也有定义类别的前件时，如果还没有出现，它们只是被添加到现有列表中。

类别和标签之间的标志性区别是，帖子的类别可以被合并到为帖子[the generated URL](https://jekyllrb.com/docs/permalinks/#global)中，而标签不能。

因此，根据封面是 `category: classic hollywood` 还是 `categories: classic hollywood` ，上面的示例帖子的URL将分别为 `movies/horror/classic%20hollywood/2019/05/21/bride-of-chucky.html` 或 `movies/horror/classic/hollywood/2019/05/21/bride-of-chucky.html` 。

##  Posts目录

您可以通过在帖子上使用 `excerpt` 变量来访问帖子内容的片段。默认情况下，这是文章中内容的第一段，但可以通过在前页或 `_config.yml` 中设置 `excerpt_separator` 变量来自定义。

```markdown
---
excerpt_separator: <!--more-->
---

Excerpt with multiple paragraphs

Here's another paragraph in the excerpt.
<!--more-->
Out-of-excerpt
```

下面是一个输出带有摘录的博客文章列表的示例：

```
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
```

##  草稿

草稿是文件名中没有日期的帖子。他们是你还在写的帖子，还不想发布。要启动并运行草稿，请在网站的根目录下创建一个 `_drafts` 文件夹，然后创建第一个草稿：

```text
.
├── _drafts
│   └── a-draft-post.md
...
```

若要预览您的网站草稿，请使用 `--drafts` 开关运行 `jekyll serve` 或 `jekyll build` 。每一个都将被分配到草稿文件的修改时间值，因此您将看到当前编辑的草稿作为最新的帖子。