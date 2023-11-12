---
title: Jekyll Front Matter配置
date: 2023-11-07 22:47:41 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']

---



> [参考原文](https://jekyllrb.com/docs/configuration/front-matter-defaults/)



## Front Matter Defaults

使用[front matter](https://jekyllrb.com/docs/front-matter/) 是一种可以在页面和帖子中指定站点配置的方法。设置默认布局，自定义标题，或者为文章指定更精确的日期/时间都可以添加到您的页面或文章的头版。

很多时候，你会发现你在重复很多配置选项。在每个文件中设置相同的布局，为帖子添加相同的类别等。您甚至可以添加自定义变量，如作者姓名，这可能对您博客上的大多数帖子都是相同的。

Jekyll提供了一种在站点配置中设置这些默认值的方法，而不是在每次创建新帖子或页面时重复此配置。为此，您可以使用项目根目录下 `_config.yml` 文件中的 `defaults` 键指定站点范围的默认值。

`defaults` 键包含一个scope/value对数组，这些scope/value对定义了应该为特定文件路径设置什么默认值，以及该路径中的文件类型（可选）。

假设您想为站点中的所有页面和帖子添加默认布局。你可以把它添加到你的 `_config.yml` 文件中：

```yaml
defaults:
  -
    scope:
      path: "" # an empty string here means all files in the project
    values:
      layout: "default"
```

> 停止并重新运行`jekyll serve`命令
>
> `_config.yml` 主配置文件包含全局配置和变量定义，在执行时读取一次。在自动再生过程中对 `_config.yml` 所做的更改直到下一次执行时才会加载。
>
> [Data Files](https://jekyllrb.com/docs/datafiles/)支持热加载
{: .prompt-info}

在这里，我们将 `values` 的范围限定为路径 `scope` 中存在的任何文件。由于路径设置为空字符串，因此它将应用于项目中的所有文件。你可能不想在你的项目中的每个文件上都设置一个布局,比如css文件,所以你也可以在 `scope` 键下指定一个 `type` 值。

```yaml
defaults:
  -
    scope:
      path: "" # an empty string here means all files in the project
      type: "posts" # previously `post` in Jekyll 2.2.
    values:
      layout: "default"
```

现在，这将只为类型为 `posts` 的文件设置布局。您可以使用的不同类型是 `pages` ， `posts` ， `drafts` 或您网站中的任何集合。虽然 `type` 是可选的，但在创建 `scope/values` 对时必须为 `path` 指定一个值。

如前所述，您可以为 `defaults` 设置多个作用域/值对。

```yaml
defaults:
  -
    scope:
      path: ""
      type: "pages"
    values:
      layout: "my-site"
  -
    scope:
      path: "projects"
      type: "pages" # previously `page` in Jekyll 2.2.
    values:
      layout: "project" # overrides previous default layout
      author: "Mr. Hyde"
```

使用这些默认值，所有页面都将使用 `my-site` 布局。 `projects/` 文件夹中存在的任何html文件都将使用 `project` 布局（如果存在）。这些文件还将 `page.author`  [liquid variable](https://jekyllrb.com/docs/variables/) 设置为 `Mr. Hyde` 。

```yaml
collections:
  my_collection:
    output: true

defaults:
  -
    scope:
      path: ""
      type: "my_collection" # a collection in your site, in plural form
    values:
      layout: "default"
```

在本例中，在名称为 `my_collection` 的[collection](https://jekyllrb.com/docs/collections/) 中， `layout` 被设置为 `default` 。

### Front Matter默认值中的Glob模式

在匹配默认值时，也可以使用glob模式（目前仅限于包含 `*` 的模式）。例如，可以在 `section` 文件夹的任何子文件夹中为每个 `special-page.html` 设置特定的布局。

```yaml
collections:
  my_collection:
    output: true

defaults:
  -
    scope:
      path: "section/*/special-page.html"
    values:
      layout: "specific-layout"
```

> 全局化和性能
>
> 请注意，已知globbing路径会对性能产生负面影响，目前尚未优化，尤其是在Windows上。全局化路径将增加与关联集合目录的大小成比例的构建时间。
{: .prompt-danger }



### 优先级

**default => _config.yaml => Front Matter**

Jekyll将应用您在 `_config.yml` 文件的 `defaults` 部分中指定的所有配置设置。您可以通过为作用域指定更具体的路径来选择覆盖其他作用scope/value对中的设置。

你可以在上面的倒数第二个例子中看到这一点。首先，我们将默认页面布局设置为 `my-site` 。然后，使用更具体的路径，我们将 `projects/` 路径中页面的默认布局设置为 `project` 。这可以通过您在页面或文章封面中设置的任何值来完成。

最后，通过在 `_config.yml` 文件中添加 `defaults` 节来设置站点配置中的默认值，则可以在文章或页面文件中覆盖这些设置。所有你需要做的是指定设置在文章或页面的头版。举例来说：

```yaml
# In _config.yml
...
defaults:
  -
    scope:
      path: "projects"
      type: "pages"
    values:
      layout: "project"
      author: "Mr. Hyde"
      category: "project"
...
# In projects/foo_project.md
---
author: "John Smith"
layout: "foobar"
---
The post text goes here...
```

在构建站点时， `projects/foo_project.md` 将 `layout` 设置为 `foobar` 而不是 `project` ， `author` 设置为 `John Smith` 而不是 `Mr. Hyde` 。
