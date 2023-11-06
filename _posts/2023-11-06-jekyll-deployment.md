---
title: 使用Jekyll搭建博客(十)
date: 2023-11-06 13:52:02 +0800
categories: ['Tech', 'Jekyll']
tags: ['建站']
render_with_liquid: false
---

> 本文参照[Jekyll官网](https://jekyllrb.com/docs/)分步教程,用以练手写博客



## Deployment

在这最后一步中，需要为生产做好准备。



## Gemfile

有一个[Gemfile](https://jekyllrb.com/docs/ruby-101/#gemfile) 在网站上是一个很好的做法。确保了Jekyll和其他gem的版本在不同的环境中保持一致。

在根目录中创建一个 `Gemfile` 。或者可以使用Bundler生成一个Gemfile，然后添加 `jekyll` gem：

```ruby
bundle init
bundle add jekyll
```

文件应该看起来像这样：

```ruby
# frozen_string_literal: true
source "https://rubygems.org"

gem "jekyll"
```

Bundler安装了gem并创建一个 `Gemfile.lock` ，它锁定了当前的gem版本，以备将来使用 `bundle install` 。如果你想更新你的gem版本，你可以运行 `bundle update` 。

当使用 `Gemfile` 时，将运行带有 `bundle exec` 前缀的 `jekyll serve` 命令。所以完整的命令是：

```ruby
bundle exec jekyll serve
```

这限制了Ruby环境只能使用 `Gemfile` 中的gem集。

注意：如果使用GitHub Pages发布您的网站，您可以通过在 `Gemfile` 中使用 `github-pages` gem而不是 `jekyll` 来匹配Jekyll的生产版本。在这种情况下，您可能还希望从存储库中排除 `Gemfile.lock` ，因为GitHub Pages会忽略该文件。

## Plugins 插件

Jekyll插件允许您创建特定于您网站的自定义生成内容。这里有很多[plugins](https://jekyllrb.com/docs/plugins/) 可以使用，甚至可以自己编写。

有三个官方插件在任何Jekyll网站上都很有用：

- [jekyll-sitemap](https://github.com/jekyll/jekyll-sitemap)  - 创建一个sitemap文件来帮助搜索引擎索引内容

- [jekyll-feed](https://github.com/jekyll/jekyll-feed) - 为文章创建一个RSS源

- [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag) - 添加Meta标签以帮助SEO

要使用它们，需要将它们添加到您的 `Gemfile` 。如果将其放在 `jekyll_plugins` 组中，他们将自动添加到Jekyll：

```
source 'https://rubygems.org'

gem 'jekyll'

group :jekyll_plugins do
  gem 'jekyll-sitemap'
  gem 'jekyll-feed'
  gem 'jekyll-seo-tag'
end
```

然后将这些行添加到 `_config.yml` ：

```
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag
```

现在通过运行 `bundle update` 来安装它们。

`jekyll-sitemap` 不需要任何设置，它将在构建时创建您的网站地图。

对于 `jekyll-feed` 和 `jekyll-seo-tag` ，需要将标签添加到 `_layouts/default.html` ：

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
    <link rel="stylesheet" href="/assets/css/styles.css">
    {% feed_meta %}
    {% seo %}
  </head>
  <body>
    {% include navigation.html %}
    {{ content }}
  </body>
</html>
```

重新启动Jekyll服务器，检查这些标签是否已添加到 `<head>` 。



## Environments 环境

有时候，你可能想在生产环境中输出一些东西，而不是在开发环境中。例如分析脚本。

可以使用[environments在运行命令时使用 `JEKYLL_ENV` 环境变量来设置环境。举例来说：

```shell
JEKYLL_ENV=production bundle exec jekyll build
```

 `JEKYLL_ENV` 默认是开发环境。可以使用 `jekyll.environment` 以Liquid形式使用 `JEKYLL_ENV` 。因此要仅在生产环境中输出分析脚本，您需要执行以下操作：

```html
{% if jekyll.environment == "production" %}
  <script src="my-analytics-script.js"></script>
{% endif %}
```



## Deployment 部署

最后一步是将网站放到生产服务器上。最简单的方法就是运行生产构建：

```shell
JEKYLL_ENV=production bundle exec jekyll build
```

然后将 `_site` 的内容复制到您的服务器。



>  目标文件夹在站点构建时将会被清理
>
>  默认情况下，在构建站点时， `_site` 的内容会自动清除。将删除不是由站点的生成过程创建的文件或文件夹。
>
>  某些文件可以通过在 `keep_files` 配置指令中指定来保留。其他文件可以通过将它们保存在assets目录中来保留。
>
>  更好的方法是使用 [CI](https://jekyllrb.com/docs/deployment/automated/) or [第三方自动化](https://jekyllrb.com/docs/deployment/third-party/)。
{: .prompt-danger }