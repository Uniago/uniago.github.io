---
title: Jekyll Collections
date: 2023-11-12 01:28:35 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll内容']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/collections/)



## Collections

集合是对相关内容进行分组的好方法，例如团队成员或会议上的谈话。

## 设置

要使用Collection，您首先需要在 `_config.yml` 中定义它。例如，这里有一个工作人员的集合：

```yaml
collections:
  - staff_members
```

在这种情况下， `collections` 被定义为序列（即，数组），没有为每个集合定义额外的元数据。您可以通过将 `collections` 定义为映射（即，hashmap）而不是sequence，然后在其中定义额外的字段：

```yaml
collections:
  staff_members:
    people: true
```

> 将集合定义为序列时，默认情况下不会呈现其页。要启用此功能，必须在集合上指定 `output: true` ，这需要将集合定义为映射。有关详细信息，请参阅[Output](https://jekyllrb.com/docs/collections/#output)一节。
{: .prompt-info }

> Collections
您可以选择使用 `collections_dir: my_collections` 指定一个目录，将所有集合存储在同一个位置。
然后Jekyll会在 `my_collections/_books` 中查找 `books` 集合，在 `my_collections/_recipes` 中查找 `recipes` 集合。
{: .prompt-warning }

> 请务必将草稿和帖子移动到自定义集合目录中
如果你指定一个目录来存储你所有的收藏在同一个地方与 `collections_dir: my_collections` ，那么你将需要移动你的 `_drafts` 和 `_posts` 目录到 `my_collections/_drafts` 和 `my_collections/_posts` 。请注意，集合目录的名称不能以下划线（`_`）开头。
{: .prompt-danger }

## 添加内容

创建相应的文件夹（例如 `<source>/_staff_members` ）并添加文档。如果Front Matter存在，则处理Front Matter，并将Front Matter之后的所有内容推入文档的 `content` 属性中。如果没有提供Front Matter，Jekyll将认为它是一个[static file](https://jekyllrb.com/docs/static-files/)，内容将不会进行进一步的处理。如果提供了front matter，Jekyll将把文件内容处理成预期的输出。

无论是否存在front matter，只有在集合的元数据中设置了 `output: true` 时，Jekyll才会写入目标目录（例如 `_site` ）。

例如，以下是如何将职员添加到上面的集合集中。文件名为 `./_staff_members/jane.md` ，内容如下：

```markdown
---
name: Jane Doe
position: Developer
---
Jane has worked on Jekyll for the past *five years*.
```

请注意，尽管在内部被认为是一个集合，上述内容并不适用于[posts](https://jekyllrb.com/docs/posts/)。具有有效文件名格式的帖子将被标记为处理，即使它们不包含封面。

> 请确保正确命名目录
该文件夹的名称必须与您在 `_config.yml` 文件中定义的集合相同，并在前面添加 `_` 字符。
{: .prompt-info }

## 输出

现在，您可以在页面上覆盖 `site.staff_members` 并为每个员工输出内容。与posts类似，使用 `content` 变量访问文档的主体：

```
{% for staff_member in site.staff_members %}
  <h2>{{ staff_member.name }} - {{ staff_member.position }}</h2>
  <p>{{ staff_member.content | markdownify }}</p>
{% endfor %}
```

如果您希望Jekyll为集合中的每个文档创建呈现页面，则可以在集合元数据中的将 `output` 键设置为 `true` ：

```yaml
collections:
  staff_members:
    output: true
```

您可以使用 `url` 属性链接到生成的页面：

```
{% for staff_member in site.staff_members %}
  <h2>
    <a href="{{ staff_member.url }}">
      {{ staff_member.name }} - {{ staff_member.position }}
    </a>
  </h2>
  <p>{{ staff_member.content | markdownify }}</p>
{% endfor %}
```

## Permalinks

集合有特殊的[permalink变量](https://jekyllrb.com/docs/permalinks/#collections)来帮助你控制整个集合的输出url。

## 自定义文档排序

默认情况下，当集合中的两个文档都在其前页中具有 `date` 键时，它们将按其 `date` 属性进行排序。但是，如果其中一个或两个文档在其前面的内容中没有 `date` 键，则它们将按各自的路径进行排序。

您可以通过集合的元数据控制这种排序。

### 按Front Matter关键字排序

通过将 `sort_by` 元数据设置为前体键字符串，可以基于前体键对文档进行排序。例如，要基于键 `lesson` 对教程集合进行排序，配置将是：

```yaml
collections:
  tutorials:
    sort_by: lesson
```

文档按键值的递增顺序排列。如果一个文档没有定义前页关键字，那么这个文档将被直接放置在排序后的文档之后。当多个文档没有定义前页关键字时，这些文档将按其日期或路径排序，然后直接放在排序后的文档之后。

### 手动排序

您还可以通过设置 `order` 元数据来手动对文档进行排序，其中文件名按所需顺序列出。例如，教程集合将配置为：

```yaml
collections:
  tutorials:
    order:
      - hello-world.md
      - introduction.md
      - basic-concepts.md
      - advanced-concepts.md
```

任何文件名与列表条目不匹配的文档都会被放置在重新排列的文档之后。如果文档嵌套在子目录下，则也将它们包含在条目中：

```yaml
collections:
  tutorials:
    order:
      - hello-world.md
      - introduction.md
      - concepts/basics.md
      - concepts/advanced.md
```

如果两个元数据键都已正确定义，则 `order` list优先。

## Liquid属性

### Collections

集合也可在 `site.collections` 下使用，其中包含您在 `_config.yml` 中指定的元数据（如果存在）和以下信息：

| VARIABLE             | DESCRIPTION                             |
| -------------------- | --------------------------------------- |
| `label`              | collections的名称，例如 `my_collection` |
| `docs`               | documents数组                           |
| `files`              | 集合中的静态文件数组                    |
| `relative_directory` | 相对于网站源的集合源目录的路径          |
| `directory`          | 集合的源目录的完整路径                  |
| `output`             | 集合的文档是否将作为单个文件输出        |

> 硬编码集合
除了您自己创建的任何集合之外， `posts` 集合也被硬编码到Jekyll中。不管你有没有 `_posts` 目录，它都存在。这是在迭代 `site.collections` 时需要注意的，因为你可能需要过滤掉它。
您可能希望使用过滤器来查找您的收藏： `{{ site.collections | where: "label", "myCollection" | first }}`
{: prompt-info }

> Collections and Time
除了硬编码的默认集合 `posts` 中的文档外，您创建的集合中的所有文档都可以通过Liquid访问，无论其分配的日期如何（如果有），因此都可以呈现。
仅当相关集合元数据具有 `output: true` 时，才尝试将文档写入磁盘。此外，只有当 `site.future` 也为真时，才编写未来日期的文档。
通过在文档的前页中设置 `published: false` （默认为 `true` ），可以对写入磁盘的文档进行更细粒度的控制。
{: prompt-info }

### Documents 

除了在文档的相应文件中提供的任何封面内容外，每个文档还具有以下属性：

| VARIABLE        | DESCRIPTION                                                  |
| --------------- | ------------------------------------------------------------ |
| `content`       | 文档的（未呈现的）内容。如果没有提供前件，Jekyll将不会在您的集合中生成该文件。如果使用了front matter，则这是在front matter的终止符`-`之后文件的所有内容。 |
| `output`        | 文档的呈现输出，基于 `content` 。                            |
| `path`          | 文档源文件的完整路径。                                       |
| `relative_path` | 文档源文件相对于网站源的路径。                               |
| `url`           | 呈现的集合的URL。仅当文件所属的集合在站点配置中具有 `output: true` 时，文件才会写入目标。 |
| `collection`    | 文档collection的名称                                         |
| `date`          | 文档collection的日期                                         |
