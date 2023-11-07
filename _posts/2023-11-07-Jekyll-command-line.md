---
title: Jekyll命令行
date: 2023-11-07 20:49:14 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']
---



>[参考原文](https://jekyllrb.com/docs/usage/)



## 命令行用法

Jekyll gem在终端中提供了一个 `jekyll` 可执行文件。

`jekyll` 参数：

```shell
jekyll command [argument] [option] [argument_to_option]

Examples:
    jekyll new site/ --blank
    jekyll serve --config _alternative_config.yml
```

在本地开发时使用 `jekyll serve` ，在生产站点时使用 `jekyll build` 。
选项及其参数的[完整列表](https://jekyllrb.com/docs/configuration/options/#build-command-options)

以下是一些常用的命令：

- `jekyll new PATH` -  在指定路径创建一个新的Jekyll站点，默认为基于gem的主题。将根据需要创建目录。
- `jekyll new PATH --blank` -  在指定路径创建一个新的空白Jekyll站点脚手架。
- `jekyll build` or `jekyll b` - 执行一次性构建您的网站到 `./_site` （默认）。
- `jekyll serve` or `jekyll s` -  构建您的站点并在本地提供服务。
- `jekyll clean` -  删除所有生成的文件包括目标文件夹，元数据文件，Sass和Jekyll缓存。
- `jekyll help` - 显示帮助，可选地针对给定的子命令，例如 `jekyll help build` 。
- `jekyll new-theme` -  创建一个新的Jekyll主题脚手架。
- `jekyll doctor` -  查看一些弃用或配置问题。

要更改Jekyll的默认构建行为，请查看[配置选项](https://jekyllrb.com/docs/configuration/)。