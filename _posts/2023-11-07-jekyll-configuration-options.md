---
title: Jekyll配置项
date: 2023-11-07 21:11:23 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll配置']
---



> [参考原文](https://jekyllrb.com/docs/configuration/options/)



## 配置项

下表列出了Jekyll的可用配置，以及控制它们的各种 `options` （在配置文件中指定）和 `flags` （在命令行中指定）。

### Global Configuration 

| Setting                    | Description                                                  | Options                                                      | Flags                   |
| -------------------------- | :----------------------------------------------------------- | ------------------------------------------------------------ | ----------------------- |
| Site source Site           | 更改Jekyll读取文件的目录,默认_post                           | `source: DIR`                                                | `-s, --source DIR`      |
| Site destination           | 更改Jekyll将写入文件的目录,默认_site                         | `destination: DIR`                                           | `-d, --destination DIR` |
| Safe                       | 禁用[非白名单插件](https://jekyllrb.com/docs/plugins/)，禁用缓存到磁盘，以及忽略符号链接。 | `safe: BOOL`                                                 | `--safe`                |
| Disable disk cache         | 禁用磁盘缓存<br />禁用将内容缓存到磁盘，以跳过在源位置创建 `.jekyll-cache` 或类似目录，从而避免干扰虚拟环境和第三方目录监视器。<br />在 `safe` 模式下，缓存到磁盘始终处于禁用状态。 | `disable_disk_cache: BOOL`                                   | `--disable-disk-cache`  |
| Ignore theme configuration | 忽略主题配置<br />Jekyll 4.0开始允许主题捆绑 `_config.yml` ，以简化新用户的主题加载。<br />在导入捆绑的主题配置会弄乱合并的站点配置的不幸情况下，用户可以配置Jekyll不完全导入主题配置。 | `ignore_theme_config: BOOL`                                  |                         |
| Exclude                    | 从转换中删除目录和(或)文件。这些排除项与站点的源目录相关，不能位于源目录之外。 <br />在Jekyll 3中，`exclude`配置选项取代了默认的排除列表。 <br />在Jekyll 4中，用户提供的条目被添加到默认的排除列表中，并且可以使用“include”选项来覆盖默认的排除列表条目。<br />默认排除在由 `jekyll new` 创建的 `_config.yml` 中找到：<br />`.sass-cache/`<br />`.jekyll-cache/`<br />`gemfiles/`<br />`Gemfile`<br />`Gemfile.lock`<br />`node_modules/`<br />`vendor/bundle/`<br />`vendor/cache/`<br />`vendor/gems/`<br />`vendor/ruby/` | `exclude: [DIR, FILE, ...]`                                  |                         |
| Include                    | 强制在转换中包含目录和(或)文件。 <br />`.htaccess` 是一个很好的例子，因为默认情况下dotfiles被排除在外。<br />在Jekyll 4中，“include”配置选项条目覆盖了“exclude”选项条目。 | `include: [DIR, FILE, ...]`                                  |                         |
| Keep files                 | 当清除目标站点时，保留选定的文件。<br />对于不是由jekyll生成的文件很有用;<br />例如，由构建工具生成的文件或资源。路径是相对于 `destination` 的。 | `keep_files: [DIR, FILE, ...]`                               |                         |
| Time zone                  | 设置站点生成的时区。<br />这将设置 `TZ` 环境变量，Ruby使用该变量来处理时间和日期的创建和操作。<br />[IANA时区数据库 ](https://en.wikipedia.org/wiki/Tz_database) 中的任何条目都是有效的，例如 `America/New_York` 。<br />所有可用值的列表可以在[这里](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)找到。<br />在本地计算机上服务时，默认时区由操作系统设置。<br />但是当在远程主机/服务器上提供时，默认时区取决于服务器的设置或位置。 | `timezone: TIMEZONE`                                         |                         |
| Encoding                   | 按名称设置文件的编码（仅适用于Ruby 1.9或更高版本）。<br />从2.0.0开始，默认值是 `utf-8` ，在2.0.0之前是 `nil` ，这将产生Ruby默认值 `ASCII-8BIT` 。<br />可用的编码可以通过命令 `ruby -e 'puts Encoding::list.join("\n")'` 显示。 | `encoding: ENCODING`                                         |                         |
| Defaults                   | 对[front matter](https://jekyllrb.com/docs/front-matter/)变量设置默认值。 | see [below](https://jekyllrb.com/docs/configuration/front-matter-defaults/) 见下文 |                         |

>  `<destination>` 文件夹在站点构建时被清理
>
> 默认在构建站点时， `<destination>` 的内容会自动清除。不是由您的网站创建的文件或文件夹将被删除。一些文件可以通过在 `<keep_files>` 配置指令中指定它们来保留。
>
> Do not use an important location for `<destination>`; instead, use it as a staging area and copy files from there to your web server.
> 不要把重要文件放在 `<destination>` 中,应该将其视为暂存区域，并将文件从 `<destination>` 复制到Web服务器的其他地方。
{: .prompt-danger }



### Build Command Options 

| Setting                | Description                                                  | Options                          | Flags                           |
| ---------------------- | ------------------------------------------------------------ | -------------------------------- | ------------------------------- |
| Regeneration           | 当文件被修改时启用站点的自动生成                             |                                  | `-w, --[no-]watch`              |
| Configuration          | 使用指定配置文件替代默认的 `_config.yml` 。<br />如果有多个配置文件,后面的配置文件会覆盖掉前面的配置文件 |                                  | `--config FILE1[,FILE2,...]`    |
| Plugins                | 指定插件目录覆盖默认的`_plugins/`                            | `plugins_dir: [ DIR1,... ]`      | `-p, --plugins DIR1[,DIR2,...]` |
| Layouts                | 指定布局目录覆盖默认的`_layouts/`                            | `layouts_dir: DIR`               | `--layouts DIR`                 |
| Drafts                 | 处理和呈现草稿帖子                                           | `show_drafts: BOOL`              | `-D, --drafts`                  |
| Environment            | 在生产中使用特定的环境值。                                   | `JEKYLL_ENV=production`          |                                 |
| Future                 | 发布具有未来日期的帖子或集合文档。                           | `future: BOOL`                   | `--future`                      |
| Unpublished            | 标记为未发布的帖子                                           | `unpublished: BOOL`              | `--unpublished`                 |
| LSI                    | 为相关的帖子制作索引。需要[classifier-reborn](https://jekyll.github.io/classifier-reborn/) 插件 | `lsi: BOOL`                      | `--lsi`                         |
| Limit posts            | 限制要解析和发布的帖子的数量                                 | `limit_posts: NUM`               | `--limit_posts NUM`             |
| Force polling          | 使用轮询强制监视                                             | `force_polling: BOOL`            | `--force_polling`               |
| Verbose output         | 打印详细输出                                                 | `verbose: BOOL`                  | `-V, --verbose`                 |
| Silence output         | 在构建过程中使Jekyll静音输出                                 | `quiet: BOOL`                    | `-q, --quiet`                   |
| Log level              | 指定日志级别debug、info、warn或error                         | `JEKYLL_LOG_LEVEL=info`          |                                 |
| Incremental build      | 启用实验性[incremental build](https://jekyllrb.com/docs/configuration/incremental-regeneration/) 特性。增量构建仅重新构建已更改的帖子和页面，从而显著提高大型网站的性能，但在某些情况下也可能中断网站生成。 | `incremental: BOOL`              | `-I, --incremental`             |
| Disable bundle require | 在`：jekyll_plugins` Gemfile中使用gem                        | `JEKYLL_NO_BUNDLER_REQUIRE=true` |                                 |
| Liquid profiler        | 生成Liquid渲染配置文件用于识别性能瓶颈                       | `profile: BOOL`                  | `--profile`                     |
| Strict front matter    | 如果页面的front matter中存在YAML语法错误，则会导致构建失败   | `strict_front_matter: BOOL`      | `--strict_front_matter`         |
| Base URL               | 指定URL提供服务                                              | `baseurl: URL`                   | `-b, --baseurl URL`             |
| Trace                  | 当错误发生时显示完整的回溯。                                 |                                  | `-t, --trace`                   |

### Serve Command Options

除了下面的选项之外， `serve` 子命令可以接受 `build` 子命令的任何选项，然后将其应用于站点构建，该站点构建发生在您的站点服务之前。

| Setting                      | Description                                | Options                                                      | Flags                                                        |
| ---------------------------- | ------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Local server port            | 指定监听端口。<br />默认值为`4000`。       | `port: PORT`                                                 | `-P, --port PORT`                                            |
| Local server hostname        | 指定监听主机名。<br />默认值为“localhost”  | `host: HOSTNAME`                                             | `-H, --host HOSTNAME`                                        |
| Live reload                  | 在编辑页面内容时，在浏览器上实时显示页面。 | `livereload: BOOL`                                           | `-l, --livereload`                                           |
| Live reload ignore           | 配置忽略要实时预览的glob文件               | `livereload_ignore: [ GLOB1,... ]`                           | `--livereload-ignore GLOB1[,GLOB2,...]`                      |
| Live reload min/max delay    | 自动重新加载页面之前的最小/最大延迟        | `livereload_min_delay: SECONDS` <br />`livereload_max_delay: SECONDS` | `--livereload-min-delay SECONDS`<br /> `--livereload-max-delay SECONDS` |
| Live reload port             | Livestock要监听的端口                      |                                                              | `--livereload-port PORT`                                     |
| Open URL                     | 在浏览器中打开网站的URL                    | `open_url: BOOL`                                             | `-o, --open-url`                                             |
| Detach                       | 从终端分离服务器                           | `detach: BOOL`                                               | `-B, --detach`                                               |
| Skips the initial site build | 跳过在服务器启动之前发生的初始站点构建     | `skip_initial_build: BOOL`                                   | `--skip-initial-build`                                       |
| Show directory listing       | 显示目录列表而不是加载索引文件             | `show_dir_listing: BOOL`                                     | `--show-dir-listing`                                         |
| X.509 (SSL) private key      | 存储SSL私钥到站点源中                      |                                                              | `--ssl-key`                                                  |
| X.509 (SSL) certificate      | 存储SSL证书到站点源中                      |                                                              | `--ssl-cert`                                                 |

> 不要在配置文件中使用制表符
>
> 这将导致解析错误，或者Jekyll将恢复到默认设置。使用空格代替。
{: .prompt-danger }