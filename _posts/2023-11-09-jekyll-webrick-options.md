---
title: Jekyll WEBrick配置
date: 2023-11-09 22:18:51 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']
---

> [参考原文](https://jekyllrb.com/docs/configuration/webrick/)

## WEBrick Options

您可以通过将它们添加到 `_config.yml` 来为您的网站提供自定义标题

```yaml
# File: _config.yml
webrick:
  headers:
    My-Header: My-Value
    My-Other-Header: My-Other-Value
```

### Defaults

Jekyll默认提供了 `Content-Type` 和 `Cache-Control` 响应头：

- `Content-Type`是动态的，用以指定所服务的数据的性质
- `Cache-Control`是静态的，用以禁用缓存