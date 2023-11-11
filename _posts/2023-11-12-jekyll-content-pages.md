---
title: Jekyll Pages
date: 2023-11-12 00:32:59 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll内容']
---

> [参考原文](https://jekyllrb.com/docs/pages/)
## Pages

页面是内容的最基本构建块。它们对于独立内容（不基于日期的内容或不是一组内容，如工作人员或食谱）很有用。

添加页面的最简单方法是在根目录中添加一个HTML文件，并使用合适的文件名。您也可以使用 `.md` 扩展和front matter在Markdown中编写页面，并在构建时转换为HTML。对于具有主页、关于页面和联系人页面的站点，根目录和相关URL可能如下所示：

```text
.
├── about.md    # => http://example.com/about.html
├── index.html    # => http://example.com/
└── contact.html  # => http://example.com/contact.html
```

如果你有很多页面，你可以把它们组织成一个小块。当您的站点构建时，用于在项目源代码中对您的页面进行分组的同一个文件夹将存在于 `_site` 文件夹中。然而，当一个页面在前页中设置了不同的固定链接时， `_site` 的子文件夹会相应地改变。

```text
.
├── about.md          # => http://example.com/about.html
├── documentation     # folder containing pages
│   └── doc1.md       # => http://example.com/documentation/doc1.html
├── design            # folder containing pages
│   └── draft.md      # => http://example.com/design/draft.html
```

##  更改输出URL

您可能希望为源文件设置一个特定的文件夹结构，该结构会随生成的站点而更改。使用[permalinks](https://jekyllrb.com/docs/permalinks/)可以完全控制输出URL。

## Pages目录

从Jekyll 4.1.1开始，可以通过在配置文件中设置 `page_excerpts` 到 `true` 来选择为页面生成摘录。