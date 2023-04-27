# Citation Style Language (CSL) 教程

使用开源软件 [Zotero](https://www.zotero.org/) 进行文献引用管理时，尽管自带多种期刊的引用格式文件（\*.csl），但有时仍无法满足个性化定制的要求。因此，本教程针对某些常用的格式控制如何通过修改 CSL 文件来实现进行记录。

## CSL 项目地址

官方网站：https://citationstyles.org/

Github 地址：https://github.com/citation-style-language

在线文档：https://docs.citationstyles.org/en/stable/

## 引用格式控制

### Zotero 软件中样式管理器的标题

通过修改 `xml-style-info-title` 元素的文本来实现。

```xml
<info>
  <title>East China University of Science and Technology</title>
</info>
```

具体效果为：

```{image} csl1.png
:width: 500
:align: center
```

### Zotero 软件中样式管理器的最后更新

通过修改 `xml-style-info-updated` 元素的文本来实现。

```xml
<info>
  <updated>2024-04-27T14:01:31+00:00</updated>
</info>
```

具体效果可参见上图。

### 参考文献中的作者信息

通过修改 `xml-style-<macro name="author">` 元素实现。

```xml
<macro name="author">
  <names variable="author" suffix=".">
    <name sort-separator=" " initialize-with=". " name-as-sort-order="all" delimiter=", " delimiter-precedes-last="always"/>
    <label form="short" prefix=", " text-case="capitalize-first"/>
  </names>
</macro>
```

示例效果为：

Li Z., Werner K., Chen L., Jia A., Qian K., Zhong J., You R., Wu L., Zhang L., Pan H., Wu X., Gong X., Shaikhutdinov S., Huang W., Freund H.

具体的属性与对应效果如下所述：

- `<names suffix=".">`：所有作者名字后以 `.` 结束
- `<name sort-separator=" ">`：姓和名以 ` ` 作为分割符
- `<name initialize-with=". ">`：名字缩写后的首字母以 `. ` 作为后缀
- `<name name-as-sort-order="all">`：`所有的`作者名字都遵循姓在前，名在后的顺序
- `<name delimiter=", ">`：作者姓名以 `, `分割
- `<name delimiter-precedes-last="always">`：最后一个作者名字前`总是`有分隔符（如果没有 `and`，该参数无效）

### 正文中的引用编号格式

通过修改 `xml-style-citation` 元素实现。

```xml
<citation collapse="citation-number">
  <sort>
    <key variable="citation-number"/>
  </sort>
  <layout delimiter="," vertical-align="sup" suffix="]" prefix="[">
    <text variable="citation-number"/>
  </layout>
</citation>
```

示例效果为：

<sup>[1]</sup>

具体的属性与对应效果如下所述：

- `<citation collapse="citation-number">`：根据`引用编号` collapse
- `<key variable="citation-number"/>`：根据`引用编号`排序
- `<layout delimiter=",">`：引用编号以 `,` 作为分隔符
- `<layout vertical-align="sup">`：引用编号作为`上标`
- `<layout suffix="]">`：引用编号以 `]` 作为后缀
- `<layout prefix="["`：引用编号以 `[` 作为前缀

### 参考文献中的引用编号格式

通过修改 `xml-style-bibliography-layout-<text variable="citation-number">` 元素实现。

```xml
<bibliography second-field-align="flush" entry-spacing="0">
  <layout suffix=".">
    <text variable="citation-number" prefix="[" suffix="]"/>
  </layout>
</bibliography>
```

示例效果为：

[1]

具体的属性与对应效果如下所述：

- `<bibliography second-field-align="flush">`：规定引用编号后的字段多行时以 `flush` 方式对齐
- `<bibliography entry-spacing="0">`：指定参考文献中的每个条目的垂直距离为 `0`

### 参考文献中的期刊格式

```xml
<text variable="container-title" font-style="italic" form="normal" suffix="." />
```

示例效果为：

_Chemistry – A European Journal._

具体的属性与对应效果如下所述：

- `font-style="italic"`：设置字体`倾斜`
- `form="normal"`：期刊`全称`

### 参考文献中的发表年份格式

```xml
<text macro="issued" font-weight="bold" suffix=","/>
```

示例效果为：

**2021,**

具体的属性与对应效果如下所述：

- `font-weight="bold"`：设置字体`加粗`

:::{note}
此处 macro="issued"代表发表年份
:::

### 参考文献中的发表卷格式

```xml
<text variable="volume" font-style="normal"/>
```

示例效果为：

27

具体的属性与对应效果如下所述：

- `font-style="normal"`：设置字体`正常`

<!-- Li Z., Werner K., Chen L., Jia A., Qian K., Zhong J., You R., Wu L., Zhang L., Pan H., Wu X., Gong X., Shaikhutdinov S., Huang W., Freund H. Interaction of Hydrogen with Ceria: Hydroxylation, Reduction, and Hydride Formation on the Surface and in the Bulk. Chemistry – A European Journal. 2021, 27(16): 5268–5276. -->
