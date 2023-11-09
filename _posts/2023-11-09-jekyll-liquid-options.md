---
title: Jekyll Liquid配置
date: 2023-11-09 22:03:55 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']
---



> [参考原文](https://jekyllrb.com/docs/configuration/liquid/)



## Liquid Options

Liquid对错误的响应可以通过设置 `error_mode` 进行配置。

- `lax` — 忽略所有错误
- `warn` — 在控制台上为每个错误输出警告。（默认）
- `strict` — 输出错误消息并停止构建

在`_config.yml`中，默认配置如下：

```yaml
liquid:
  error_mode: warn
```

上面的例子配置了“warn”值，默认情况下已经设置了- `error_mode: warn` 。这代表在构建过程中出现任何问题都会输出警告。

您还可以通过分别将 `strict_variables` 和/或 `strict_filters` 设置为 `true` 来配置Liquid的渲染器以捕获未分配的变量和不存在的过滤器。

请注意，当 `error_mode` 配置Liquid的解析器时， `strict_variables` 和 `strict_filters` 选项配置Liquid的渲染器，因此是正交的。

在`_config.yml`中设置这些变量的示例如下：

```yaml
liquid:
  error_mode: strict
  strict_variables: true
  strict_filters: true
```

如上所述的配置将阻止构建/服务的发生，并调用违规错误和停止。当希望通过停止构建或服务过程来捕获与Liquid相关的问题并允许您处理任何问题时，这很有帮助。