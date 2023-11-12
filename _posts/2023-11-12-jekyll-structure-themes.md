---
title: Jekyll Themes
date: 2023-11-12 11:56:59 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll站点结构']
---

> [参考原文](https://jekyllrb.com/docs/themes/)



## Themes 主题

Jekyll拥有一个广泛的主题系统，允许您利用社区维护的模板和样式来自定义您的网站的展示。Jekyll主题指定插件，并将资产、布局、包含文件和样式表打包在一起，这些可以被您网站的内容覆盖。

## 选择一个主题

您可以在不同的画廊中找到并预览主题

- [GitHub.com #jekyll-theme repos](https://github.com/topics/jekyll-theme)
- [jamstackthemes.dev](https://jamstackthemes.dev/ssg/jekyll/)
- [jekyllthemes.org](https://jekyllthemes.org/)
- [jekyllthemes.io](https://jekyllthemes.io/)
- [jekyll-themes.com](https://jekyll-themes.com/)

See also: [resources](https://jekyllrb.com/resources/). 

## 基于Gem的主题

当您创建一个新的Jekyll网站（通过运行 `jekyll new <PATH>` 命令），Jekyll会安装一个使用基于gem的主题[Minima](https://github.com/jekyll/minima)的网站。

使用基于 gem 的主题，网站的一些目录（例如 `assets` 、 `_data` 、 `_layouts` 、 `_includes` 和 `_sass` 目录）存储在主题的 gem 中，对于您来说是隐藏的。然而，在 Jekyll 的构建过程中，所有必要的目录都将被读取和处理。

在Minima的情况下，您只能在Jekyll站点目录中看到以下文件：

```text
.
├── Gemfile
├── Gemfile.lock
├── _config.yml
├── _posts
│   └── 2016-12-04-welcome-to-jekyll.markdown
├── about.markdown
└── index.markdown
```

`Gemfile` 和 `Gemfile.lock` 文件被Bundler用来跟踪构建Jekyll网站所需的gems和gem版本。

基于Gem的主题使主题开发人员更容易将更新提供给拥有该主题Gem的任何人。当有更新时，主题开发人员将更新推送到RubyGems。

如果你安装了主题Gem，你可以（如果你愿意的话）运行 `bundle update` 来更新项目中的所有宝石。或者你可以运行 `bundle update <THEME>` ，将 `<THEME>` 替换为主题名称，比如 `minima` ，只更新主题宝石。主题开发者添加的任何新文件或更新（比如样式表或包含文件）都会自动被拉取到你的项目中。

基于Gem的主题的目标是让您享受到一个强大、持续更新的主题的所有好处，而不会让主题的所有文件妨碍您，过度复杂化可能是您的主要关注点：创作内容。

## 覆盖主题默认设置

Jekyll主题设置默认数据、布局、包含文件和样式表。然而，您可以用自己的站点内容覆盖任何主题默认设置。

要替换主题中的布局或包含文件，请在您的 `_layouts` 或 `_includes` 目录中复制您希望修改的特定文件，或者从头开始创建一个与您希望覆盖的文件同名的文件。

例如，如果您选择的主题具有 `page` 布局，您可以通过在 `_layouts` 目录（即 `_layouts/page.html` ）中创建自己的 `page` 布局来覆盖主题的布局。

要在计算机上找到一个主题的文件：

1.  运行 `bundle info --path` ，然后是主题的宝石名称，例如， `bundle info --path minima` 是Jekyll的默认主题。
   这将返回基于 gem 的主题文件的位置。例如，Minima 主题的文件可能位于 macOS 上的 `/usr/local/lib/ruby/gems/2.6.0/gems/minima-2.5.1` 。

2.  在Finder或资源管理器中打开主题的目录

   ```bash
   # On MacOS
   open $(bundle info --path minima)
   
   # On Windows
   # First get the gem's installation path:
   #
   #   bundle info --path minima
   #   => C:/Ruby26-x64/lib/ruby/gems/3.1.3/gems/minima-2.5.1
   #
   # then invoke explorer with above path, substituting `/` with `\`
   explorer C:\Ruby26-x64\lib\ruby\gems\3.1.3\gems\minima-2.5.1
   
   # On Linux
   xdg-open $(bundle info --path minima)
   ```

   打开一个Finder或Explorer窗口，显示主题的文件和目录。Minima主题gem包含以下文件：

   ```text
   .
   ├── LICENSE.txt
   ├── README.md
   ├── _includes
   │   ├── disqus_comments.html
   │   ├── footer.html
   │   ├── google-analytics.html
   │   ├── head.html
   │   ├── header.html
   │   ├── icon-github.html
   │   ├── icon-github.svg
   │   ├── icon-twitter.html
   │   └── icon-twitter.svg
   ├── _layouts
   │   ├── default.html
   │   ├── home.html
   │   ├── page.html
   │   └── post.html
   ├── _sass
   │   ├── minima
   │   │   ├── _base.scss
   │   │   ├── _layout.scss
   │   │   └── _syntax-highlighting.scss
   │   └── minima.scss
   └── assets
       └── main.scss
   ```

通过对主题文件的清晰理解，您现在可以通过在Jekyll站点目录中创建一个同名文件来覆盖任何主题文件。

假设，作为第二个例子，你想要覆盖Minima的页脚。在你的Jekyll网站中，创建一个 `_includes` 文件夹，并在其中添加一个名为 `footer.html` 的文件。Jekyll现在将使用你网站的 `footer.html` 文件，而不是来自Minima主题宝石的 `footer.html` 文件。

要修改任何样式表，您必须额外复制主要的Sass文件（在Minima主题中为 `_sass/minima.scss` ）到您网站源代码的 `_sass` 目录中。

Jekyll在查找以下文件夹中的任何请求文件之前，首先查找您网站的内容，然后再查找主题的默认设置

- `/assets`
- `/_data`
- `/_layouts`
- `/_includes`
- `/_sass`

请注意，复制主题文件将阻止您在这些文件上接收任何主题更新。另一种选择是，在您自己的附加的、原始命名的CSS文件中使用更高特异性的CSS选择器，以便继续获取所有样式表的主题更新。

### 主题与 `_data` 目录

从版本4.3.0开始，Jekyll还考虑了主题的 `_data` 目录。这允许数据在主题之间分布。

一个典型的例子是设计元素中使用的文本。

想象一下，一个主题提供了包含文件 `testimonials.html` 。这个设计元素在页面上创建了一个新的部分，并在推荐列表上方放置了一个h3标题。
一个主题开发者可能会用英语来制定标题，并直接放入HTML源代码中。

消费者可以将主题中包含的文件复制到他们的项目中，并在那里替换标题。

考虑到 `_data` 目录，对于这个标准任务还有另一种解决方案。

设计师不直接将文本输入到设计模板中，而是添加一个对文本目录的引用（例如， `site.data.i18n.testimonials.header` ），并在主题的数据目录中创建一个文件 `_data/i18n/testimonials.yml` 。

在这个文件中，头部被放置在键 `header` 下，Jekyll会处理剩下的部分。

对于主题开发者来说，乍一看，这当然比以前需要更大的努力。

然而，对于主题的消费者来说，定制化大大简化了。

想象一下，这个主题是由一位来自德国的客户使用的。为了让她获得翻译后的推荐设计元素的标题，她只需要在项目目录中创建一个数据文件，其中包含键 `site.data.i18n.testimonials.header` ，在其顶部放置德语翻译或她选择的标题，设计元素就已经定制完成了。

她不再需要将包含的文件复制到她的项目目录中，然后在那里进行自定义，最重要的是，她不再需要放弃主题的所有更新，只因为主题开发者为她提供了通过文本文件集中更改文本模块的可能性。

>数据文件提供了很高的灵活性。主题开发人员放置文本模块的位置可能与主题的使用者不同，这可能会导致意想不到的麻烦！
{: .prompt-danger }

与上述示例相关的主题文件中的覆盖键 `site.data.i18n.testimonials.header` 可以在消费者网站的三个不同位置找到：

- `_data/i18n.yml` 使用`testimonials.header`关键字 
- `_data/i18n/testimonials.yml` 使用`header`关键字（与给定示例的布局相同）
- `_data/i18n/testimonials/header.yml`没有任何关键字，标题可以直接进入文件

主题开发者在支持那些在设置主题提供的设计元素的文本模块时感到迷茫的消费者时，应该牢记这种模糊性。

>使用数据功能时，请问自己，你引入的关键字是否会改变主题的行为，无论是否存在，还是只是显示的数据。如果它改变了主题的行为，应该放在 `site.config` 中，否则可以通过 `site.data` 提供。
{: .prompt-info }

捆绑修改主题行为的数据被认为是一种反模式，强烈不建议使用。主题的作者应确保每个提供的数据都可以被主题的使用者轻松地覆盖，如果他们希望这样做的话。

## 将基于Gem的主题转换为常规主题

假设你想摆脱基于 gem 的主题，并将其转换为常规主题，在这种主题中，所有文件都存在于你的 Jekyll 网站目录中，而不是存储在主题 gem 中。

要做到这一点，将主题 gem 的文件从其目录复制到您的 Jekyll 站点目录中。（例如，将它们复制到 `/myblog` ，如果您在 `/myblog` 创建了 Jekyll 站点。有关详细信息，请参阅前一节。）

然后你必须告诉Jekyll关于主题引用的插件。你可以在主题的gemspec文件中找到这些插件作为运行时依赖项。例如，如果你正在转换Minima主题，你可能会看到：

```ruby
spec.add_runtime_dependency "jekyll-feed", "~> 0.12"
spec.add_runtime_dependency "jekyll-seo-tag", "~> 2.6"
```

你应该以两种方式之一将这些参考资料包含在 `Gemfile` 中。

您可以在 `Gemfile` 和 `_config.yml` 中分别列出它们。

```ruby
# ./Gemfile

gem "jekyll-feed", "~> 0.12"
gem "jekyll-seo-tag", "~> 2.6"
# ./_config.yml

plugins:
  - jekyll-feed
  - jekyll-seo-tag
```

或者你可以在Gemfile中明确列出它们作为Jekyll插件，并且不更新 `_config.yml` ，像这样：

```ruby
# ./Gemfile

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-seo-tag", "~> 2.6"
end
```

无论如何，不要忘记 `bundle update` 。

如果你在GitHub Pages上发布，你应该只更新你的 `_config.yml` ，因为GitHub Pages不通过Bundler加载插件。

最后，在 `Gemfile` 和配置中删除对主题宝石的引用。例如，要删除 `minima` ：

-  打开 `Gemfile` 并移除 `gem "minima", "~> 2.5"` 
-  打开 `_config.yml` 并移除 `theme: minima` 

现在 `bundle update` 将不再获得主题Gem的更新。

## 安装基于Gem的主题

`jekyll new <PATH>` 命令并不是使用基于gem的主题创建新的Jekyll网站的唯一方法。您还可以在网上找到基于gem的主题，并将它们整合到您的Jekyll项目中。

例如，在[RubyGems上搜索jekyll主题](https://rubygems.org/search?utf8=✓&query=jekyll-theme)以找到其他基于gem的主题。（请注意，并非所有主题都在主题名称中使用 `jekyll-theme` 作为约定。）

安装基于gem的主题：

1.  将主题Gem添加到您网站的 `Gemfile` ：

   ```ruby
   # ./Gemfile
   
   # This is an example, declare the theme gem you want to use here
   gem "jekyll-theme-minimal"
   ```

   或者如果你已经使用了 `jekyll new` 命令，将 `gem "minima", "~> 2.0"` 替换为你想要的宝石，例如：

   ```ruby
   # ./Gemfile
   
   - gem "minima", "~> 2.5"
   + gem "jekyll-theme-minimal"
   ```

2. 安装主题：

   ```bash
   bundle install
   ```

3. 将以下内容添加到您网站的 `_config.yml` 中以激活主题：

   ```yaml
   theme: jekyll-theme-minimal
   ```

4. 建立您的网站

   ```bash
   bundle exec jekyll serve
   ```

>您可以在您的网站的 `Gemfile` 中列出多个主题，但在您的网站的 `_config.yml` 中只能选择一个主题。
{: .prompt-info }

如果您在[GitHub Pages](https://pages.github.com/)上发布您的Jekyll网站，请注意GitHub Pages仅支持一些基于gem的[主题](https://pages.github.com/themes/)。GitHub Pages还支持[使用任何托管在GitHub上的主题](https://help.github.com/articles/adding-a-jekyll-theme-to-your-github-pages-site/#adding-a-jekyll-theme-in-your-sites-_configyml-file)，只需将其配置为类似于基于gem的主题即可。

## 创建一个基于Gem的主题

如果你是一个Jekyll主题开发者（而不是主题的使用者），你可以将你的主题打包成RubyGems，并允许用户通过Bundler安装它。

如果你对创建Ruby宝石不熟悉，不用担心。Jekyll将帮助你使用 `new-theme` 命令搭建一个新的主题。运行 `jekyll new-theme` 并将主题名称作为参数。

这里有一个例子：

```text
jekyll new-theme jekyll-theme-awesome
    create /path/to/jekyll-theme-awesome/_layouts
    create /path/to/jekyll-theme-awesome/_includes
    create /path/to/jekyll-theme-awesome/_sass
    create /path/to/jekyll-theme-awesome/_layouts/page.html
    create /path/to/jekyll-theme-awesome/_layouts/post.html
    create /path/to/jekyll-theme-awesome/_layouts/default.html
    create /path/to/jekyll-theme-awesome/Gemfile
    create /path/to/jekyll-theme-awesome/jekyll-theme-awesome.gemspec
    create /path/to/jekyll-theme-awesome/README.md
    create /path/to/jekyll-theme-awesome/LICENSE.txt
    initialize /path/to/jekyll-theme-awesome/.git
    create /path/to/jekyll-theme-awesome/.gitignore
Your new Jekyll theme, jekyll-theme-awesome, is ready for you in /path/to/jekyll-theme-awesome!
For help getting started, read /path/to/jekyll-theme-awesome/README.md.
```

将您的模板文件添加到相应的文件夹中。然后根据您的需求完成 `.gemspec` 和README文件。

### 布局和包含

主题布局和包含文件的工作方式与在任何Jekyll网站中的工作方式相同。将布局放在主题的 `/_layouts` 文件夹中，将包含文件放在主题的 `/_includes` 文件夹中。

例如，如果您的主题有一个 `/_layouts/page.html` 文件，并且一个页面在其前置数据中有 `layout: page` ，Jekyll首先会在站点的 `_layouts` 文件夹中查找 `page` 布局，如果不存在，则使用您的主题的 `page` 布局。

### Assets 

除非用户有一个相同相对路径的文件，否则 `/assets` 中的任何文件都将在构建时复制到用户的站点上。您可以在这里放置任何类型的资源：SCSS、图像、Web字体等。这些文件的行为类似于Jekyll中的页面和静态文件。

- 如果文件顶部有[front matter](https://jekyllrb.com/docs/front-matter/)，它将被渲染。
-  如果文件没有front matter，它将被简单地复制到生成的网站中。

这使得主题创建者可以提供一个默认的 `/assets/styles.scss` 文件，他们的布局可以依赖于 `/assets/styles.css` 。

所有在 `/assets` 中的文件将会按照你在网站上使用Jekyll时所期望的方式，输出到编译后的站点中的 `/assets` 文件夹中。

### 样式表

您的主题样式表应该放在您的主题文件夹中，就像您在编写Jekyll网站时一样。

```
_sass
└── jekyll-theme-awesome.scss
```

您的主题样式可以使用 `@import` 指令包含在用户的样式表中。

```
@import "{{ site.theme }}";
```

### Theme-gem依赖 

Jekyll会自动引用主题宝石中所有被列入白名单的文件，即使它们在站点配置文件的数组中没有明确包含。（注意：只有在使用 `--safe` 选项进行构建或服务时才需要列入白名单。）

使用这个功能，最终用户无需跟踪所需的插件，以便将其配置文件包含在他们的主题宝石中，以便按预期工作。

### 预配置Theme-gems 

Jekyll将读取主题Gem根目录下的 `_config.yml` 文件，并将其数据合并到网站现有的配置数据中。

但与从主题中加载的其他实体不同，加载配置文件有一些限制，如下所总结的：

-  Jekyll的默认设置无法被主题配置所覆盖。这个问题仍然由用户决定。
-  主题配置文件不能是符号链接，无论 `safe mode` 和符号链接指向的文件是否是主题宝石中的合法文件。
-  主题配置应该是一组键值对。一个空的配置文件，一个只列出键下项目的配置文件，或者一个只有简单文本字符串的配置文件将会被静默地忽略。用户将不会收到任何关于这种不一致的警告或日志输出。
-  用户可以覆盖主题配置定义的任何设置。

尽管此功能旨在更容易地采用主题，但限制确保主题配置不会以令人担忧的方式影响构建。主题所需的任何插件都必须由用户手动列出或由主题的 `gemspec` 文件提供。

此功能将使主题Gem能够与特定主题配置变量直接配合使用。

### 记录你的主题

您的主题应该包含一个 `/README.md` 文件，该文件解释了网站作者如何安装和使用您的主题。包括哪些布局？包括哪些内容？他们需要在网站的配置文件中添加任何特殊内容吗？

### 添加截图

主题是可视化的。通过在主题存储库中包含一个屏幕截图，以便可以通过程序检索到它，向用户展示你的主题的外观。你还可以在主题的文档中包含这个屏幕截图。

### 预览您的主题

在您编写主题时，为了预览它，可能有助于添加虚拟内容，例如 `/index.html` 和 `/page.html` 文件。这将允许您使用 `jekyll build` 和 `jekyll serve` 命令来预览您的主题，就像您预览Jekyll网站一样。

如果您在本地预览主题，请确保在分发主题时将 `/_site` 添加到主题的 `.gitignore` 文件中，以防止编译的网站也被包含在内。

### 发布你的主题

主题通过[RubyGems.org](https://rubygems.org/)发布。您需要一个RubyGems账户，您可以免费[创建](https://rubygems.org/sign_up)。

1.  首先，你需要将它放在一个git仓库中：

   ```bash
   git init # Only the first time
   git add -A
   git commit -m "Init commit"
   ```

2.  接下来，通过运行以下命令打包你的主题，将 `jekyll-theme-awesome` 替换为你的主题名称：

   ```bash
   gem build jekyll-theme-awesome.gemspec
   ```

3.  最后，通过运行以下命令将打包的主题推送到RubyGems服务，再次将 `jekyll-theme-awesome` 替换为您的主题名称：

   ```bash
   gem push jekyll-theme-awesome-*.gem
   ```

4. To release a new version of your theme, update the version number in the gemspec file, ( `jekyll-theme-awesome.gemspec` in this example ), and then repeat Steps 1 - 3 above. We recommend that you follow [Semantic Versioning](https://semver.org/) while bumping your theme-version.
   更新主题的新版本，请在gemspec文件中更新版本号（在此示例中为 `jekyll-theme-awesome.gemspec` ），然后重复上述步骤1-3。我们建议您在提升主题版本时遵循[语义化版本控制](https://semver.org/)。
