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
