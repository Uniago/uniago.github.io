---
title: Jekyll Sass配置
date: 2023-11-09 22:15:17 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']
---



> [参考原文](https://jekyllrb.com/docs/configuration/sass/)



## Sass/SCSS Options

Jekyll自带[jekyll-sass-converter](https://github.com/jekyll/jekyll-sass-converter) 插件。默认情况下，Jekyll将在相对于站点的 `source` 目录的 `_sass` 目录中查找Sass部分。

您可以通过在Jekyll配置中的 `sass` 属性下添加选项来进一步配置插件。可以参阅[插件文档](https://github.com/jekyll/jekyll-sass-converter#usage) 。

> 在 `sass` 配置中指定的目录路径是相对于站点的 `source` 而不是相对于 `_config.yml` 文件的位置进行解析的。
{: .prompt-info }