---
title: Jekyll Inclueds
date: 2023-11-12 08:16:18 +0800
categories: ['Tech', 'Jekyll']
tags: ['Jekyll站点结构']
render_with_liquid: false
---

> [参考原文](https://jekyllrb.com/docs/includes/)


## Includes

`include` 标签允许您包含存储在 `_includes` 文件夹中的另一个文件的内容

```
{% include footer.html %}
```

Jekyll将在源目录的根目录下的 `_includes` 目录中查找引用的文件（在本例中为 `footer.html` ），并插入其内容。

### include相对于另一个文件的文件

您可以选择使用 `include_relative` 标签来相对于当前文件包含文件片段

```
{% include_relative somedir/footer.html %}
```

您不需要将您包含的内容放在 `_includes` 目录中。相反，包含是相对于使用该标签的文件的。例如，如果 `_posts/2014-09-03-my-file.markdown` 使用 `include_relative` 标签，那么被包含的文件必须在 `_posts` 目录或其子目录中。
请注意，您不能使用 `../` 语法来指定引用较高级目录的包含位置。
所有其他 `include` 标签的功能都可以在 `include_relative` 标签中使用，例如变量。

### 使用变量名作为include文件的名称

您想嵌入的文件名可以指定为变量，而不是实际的文件名。例如，假设您在页面的前置元数据中定义了一个变量，如下所示：

```
---
title: My page
my_variable: footer_company_a.html
---
```

您可以在您的包含文件中引用该变量

```
{% if page.my_variable %}
  {% include {{ page.my_variable }} %}
{% endif %}
```

在这个例子中，include指令会将 `footer_company_a.html` 文件从 `_includes/footer_company_a.html` 目录插入。

### 传递参数给include文件

您还可以向include传递参数。例如，假设您的 `_includes` 文件夹中有一个名为 `note.html` 的文件，其中包含以下格式:

```html
<div markdown="span" class="alert alert-info" role="alert">
<i class="fa fa-info-circle"></i> <b>Note:</b>
{{ include.content }}
</div>
```

`{{ include.content }}` 是一个参数，在调用include并为该参数指定一个值时会被填充，就像这样：
```
{% include note.html content="This is my sample note." %}
```

将 `content` 的值（即 `This is my sample note` ）插入到 `{{ include.content }}` 参数中。

将参数传递给包含文件在你想要将复杂格式化从你的Markdown内容中隐藏起来时特别有帮助。

例如，假设您有一种特殊的图像语法，具有复杂的格式，而您不希望作者记住这些复杂的格式。因此，您决定通过使用带有参数的包含来简化格式。以下是您可能希望使用包含填充的特殊图像语法的示例：

```html
<figure>
   <a href="https://jekyllrb.com">
   <img src="logo.png" style="max-width: 200px;"
      alt="Jekyll logo" />
   </a>
   <figcaption>This is the Jekyll logo</figcaption>
</figure>
```

你可以在你的include文件中将这个内容模板化，并将每个值作为参数提供，像这样：

```html
<figure>
   <a href="{{ include.url }}">
   <img src="{{ include.file }}" style="max-width: {{ include.max-width }};"
      alt="{{ include.alt }}"/>
   </a>
   <figcaption>{{ include.caption }}</figcaption>
</figure>
```

这个包含包含5个参数：
- `url`
- `max-width`
- `file`
- `alt`
- `caption`

这是一个将所有参数传递给此包含文件的示例（包含文件名为 `image.html` ）：

```
{% include image.html url="http://jekyllrb.com"
max-width="200px" file="logo.png" alt="Jekyll logo"
caption="This is the Jekyll logo." %}
```

结果是之前显示的原始HTML代码。

为了保护用户没有为参数提供值的情况，您可以使用[Liquid的默认过滤器](https://shopify.github.io/liquid/filters/default/)。

总的来说，您可以创建包含文件，作为各种用途的模板 - 插入音频或视频剪辑、警报、特殊格式等等。请注意，应避免使用过多的包含文件，因为这会减慢您网站的构建时间。例如，不要在每次插入图像时都使用包含文件。（上述技术展示了特殊图像的用例。）

### 传递参数变量给include文件

假设您想要传递给include的参数是一个变量而不是一个字符串。例如，您可能正在使用 `{{ site.product_name }}` 来引用您产品的每个实例，而不是实际的硬编码名称。（在这种情况下，您的 `_config.yml` 文件将具有一个名为 `product_name` 的键，其值为您产品的名称。）

您传递给include参数的字符串不能包含花括号。例如，您不能传递包含此内容的参数： `"The latest version of {{ site.product_name }} is now available."`

如果您想将此变量包含在传递给include的参数中，您需要在将其传递给include之前将整个参数存储为变量。您可以使用 `capture` 标签来创建变量：

```
{% capture download_note %}
The latest version of {{ site.product_name }} is now available.
{% endcapture %}
```

然后将这个捕获的变量传递给include的参数。省略参数内容周围的引号，因为它不再是一个字符串（而是一个变量）。

```
{% include note.html content=download_note %}
```
