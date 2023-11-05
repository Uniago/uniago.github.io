---
title: 使用Jekyll搭建博客(一)
date: 2023-11-05 12:24:15 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程



## 起步	

这里将从头开始构建第一个Jekyll站点，而不依赖于默认的基于gem的主题。



## 安装

**Jekyll是一个Ruby gem,因此需要先安装Ruby**

1、安装Ruby后，从终端安装Jekyll

```shell
gem install jekyll bundler
```

2、创建一个新的 `Gemfile` 以列出项目的依赖项：

```shell
bundle init
```

3、在文本编辑器中编辑 `Gemfile` 并添加jekyll作为依赖项：

```shell
gem "jekyll"
```

4、运行 `bundle` 为项目安装jekyll。

```shell
bundle
```

> 所有jekyll命令前面加上 `bundle exec` ，以确保使用的是在 `Gemfile` 中定义的jekyll版本。
{:.prompt-tip}



## 创建站点

在根目录下创建 `index.html` ，内容如下：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Home</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```



## Build 构建

由于Jekyll是一个静态的网站生成器，它必须在我们查看网站之前构建网站。

运行以下命令之一来构建网站：

- `jekyll build` -构建站点并将静态站点输出到名为 `_site` 的目录。
- `jekyll serve` -执行 `jekyll build` 并在本地Web服务器上运行 `http://localhost:4000` ，在您进行更改时随时重建站点。

> 使用 `jekyll serve --livereload` 可以实时预览修改
{:.prompt-tip}

> `jekyll serve` 在 `_site` 中构建的站点版本并不适合直接部署。
> 
> 使用 `jekyll serve` 创建的站点中的链接和资源URL将使用 `https://localhost:4000` 或命令行配置设置的值，而不是站点配置文件中设置的值。	
{: .prompt-warning}


运行 `jekyll serve` 并在浏览器中转到[http://localhost:4000](http://localhost:4000)。你应该看到"Hello World！"。
