---
title: Jekyll Data Files
date: 2023-11-12 01:47:25 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll内容']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/datafiles/)


## Data Files

除了Jekyll提供的[built-in variables](https://jekyllrb.com/docs/variables/) 外，您还可以指定自己的自定义数据，这些数据可以通过 [Liquid templating system](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)访问。
Jekyll支持从位于 `_data` 目录中的[YAML](https://yaml.org/), [JSON](https://www.json.org/json-en.html), [CSV](https://en.wikipedia.org/wiki/Comma-separated_values)和 [TSV](https://en.wikipedia.org/wiki/Tab-separated_values) 文件加载数据。请注意，CSV和TSV文件必须包含标题行。
这个强大的功能允许您避免模板中的重复，并在不更改 `_config.yml` 的情况下设置站点特定的选项。
插件/主题也可以利用数据文件来设置配置变量。

## 数据文件夹

`_data` 文件夹是您可以存储Jekyll在生成站点时使用的其他数据的地方。这些文件必须是YAML、JSON、TSV或CSV文件（使用 `.yml` 、 `.yaml` 、 `.json` 、 `.tsv` 或 `.csv` 扩展名），并且可以通过 `site.data` 访问。

## 示例：成员列表

下面是一个使用数据文件避免在Jekyll模板中复制粘贴大块代码的基本示例：
在 `_data/members.yml` 中：
```yaml
- name: Eric Mill
  github: konklone

- name: Parker Moore
  github: parkr

- name: Liu Fengyun
  github: liufengyun
```

或 `_data/members.csv` ：
```csv
name,github
Eric Mill,konklone
Parker Moore,parkr
Liu Fengyun,liufengyun
```
这些数据可以通过 `site.data.members` 访问（注意，文件的bastname决定了变量名，因此应该避免在同一目录中有相同bastname但不同扩展名的数据文件）。


现在可以在模板中呈现成员列表：
```
<ul>
{% for member in site.data.members %}
  <li>
    <a href="https://github.com/{{ member.github }}">
      {{ member.name }}
    </a>
  </li>
{% endfor %}
</ul>
```

## 子文件

数据文件也可以放在 `_data` 文件夹的子文件夹中。每个文件夹级别都将添加到变量的命名空间中。下面的示例展示了如何在 `orgs` 文件夹下的文件中单独定义GitHub组织：
在 `_data/orgs/jekyll.yml` 中：
```yaml
username: jekyll
name: Jekyll
members:
  - name: Tom Preston-Werner
    github: mojombo

  - name: Parker Moore
    github: parkr
```

在 `_data/orgs/doeorg.yml` 中：
```yaml
username: doeorg
name: Doe Org
members:
  - name: John Doe
    github: jdoe
```


然后可以通过 `site.data.orgs` 访问组织，后跟文件名：
```
<ul>
{% for org_hash in site.data.orgs %}
{% assign org = org_hash[1] %}
  <li>
    <a href="https://github.com/{{ org.username }}">
      {{ org.name }}
    </a>
    ({{ org.members | size }} members)
  </li>
{% endfor %}
</ul>
```

## 示例：查找特定作者

页面和帖子也可以访问特定的数据项。下面的示例显示了如何访问特定项目：
`_data/people.yml`:
```yaml
dave:
    name: David Smith
    twitter: DavidSilvaSmith
```

作者可以在文章的前页中被指定为页面变量：
```
---
title: sample post
author: dave
---

{% assign author = site.data.people[page.author] %}
<a rel="author"
  href="https://twitter.com/{{ author.twitter }}"
  title="{{ author.name }}">
    {{ author.name }}
</a>
```

有关如何为您的站点构建健壮的导航的信息（特别是如果您有一个文档网站或其他类型的Jekyll站点，需要组织大量页面），请参阅 [Navigation](https://jekyllrb.com/tutorials/navigation/)

## CSV/TSV解析选项

Ruby解析CSV和TSV文件的方式可以使用 `csv_reader` 和 `tsv_reader` 配置选项进行自定义。每个配置键公开相同的选项：

`converters` ：解析文件时应该使用哪些CSV转换器。可用选项有 `integer` 、 `float` 、 `numeric` 、 `date` 、 `date_time` 和 `all` 。默认情况下，此列表为空。 `encoding` ：文件的编码。安装到站点 `encoding` 配置选项。 `headers` ：是否将文件的第一行解析为头的布尔字段。当 `false` 时，它将第一行视为数据,默认为true

```yaml
csv_reader:
    converters:
      - numeric
      - datetime
    headers: true
    encoding: utf-8
tsv_reader:
    converters:
      - all
    headers: false
```
