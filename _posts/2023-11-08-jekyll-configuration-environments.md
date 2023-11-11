---
title: Jekyll Environment配置
date: 2023-11-08 20:02:56 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']
render_with_liquid: false
---



> [参考原文](https://jekyllrb.com/docs/configuration/environments/)



## Environments

在 `build` （或 `serve` ）参数中，您可以指定Jekyll环境和值。然后，构建将在内容中的任何条件语句中应用此值。

例如，假设您在代码中设置了以下条件语句：

```ruby
{% if jekyll.environment == "production" %}
   {% include disqus.html %}
{% endif %}
```

当你构建你的Jekyll站点时，除非你在build命令中指定一个 `production` 环境，否则 `if` 语句中的内容将不会运行，如下所示：

```
JEKYLL_ENV=production jekyll build
```

通过设置环境值，您可以使某些内容仅在特定环境中可用。

JEKYLL_ENV` 的默认值为 `development` 。因此，如果您从构建参数中省略 `JEKYLL_ENV` ，则默认值将为 `JEKYLL_ENV=development` 。 `{% if jekyll.environment == "development" %}` 标签中的任何内容都将自动出现在构建中。

您的环境值可以是您想要的任何值（不仅仅是 `development` 或 `production` ）。您可能希望在开发环境中隐藏的一些元素包括Disqus评论表单或Google Analytics。您可能希望在开发环境中公开“在GitHub中编辑我”按钮，但不将其包含在生产环境中。

通过在build命令中指定该选项，可以避免在从一个环境移动到另一个环境时更改配置文件中的值。

> 要根据环境切换部分配置设置，请使用[build command option](https://jekyllrb.com/docs/configuration/options/#build-command-options),，例如 `--config _config.yml,_config_development.yml` 。以后的文件中的设置将覆盖以前的文件中的设置。
{: .prompt-tip }

