# 在线文档制作

如果想要制作一个在线文档并链接到 `Github` 项目主页，可以通过 `Sphinx + ReadTheDocs` 的方法来实现，下面我将就 `Sphinx` 首先做一个简单的介绍。

## Sphinx 教程

[Sphinx](https://www.sphinx-doc.org/en/master/) 是一个从 `*.rst` 和/或 `*.md` 文件制作精美在线文档的生产力工具。

### Sphinx 安装

`Sphinx` 可以通过 `conda`进行快速的安装, 使用下面的命令：

```bash
conda install sphinx
```

### Markdown 支持

Sphinx 默认使用 `reStructuredText` 标记语言制作在线文档，因此若想使用 `markdown` 作为替代品，需要使用一些第三方工具来实现。

[recommonmark](https://recommonmark.readthedocs.io/en/latest/) 和 [myst-parser](https://myst-parser.readthedocs.io/en/latest/) 都是不错的实现工具，但后者表现更佳。因此本教程只介绍如何利用 `myst-parser` 来扩展 `Sphinx`。

你可以使用下面的命令来安装 `myst-parser`：

```bash
pip install myst-parser
```

### Sphinx 主题

当使用 Sphinx 来生成 `*.html` 文件时, 默认的网页主题不是特别美观，因此若想制作精美的在线文档，首先需要更换 Sphinx 的默认主题。

使用下述命令安装 `sphinx_rtd_theme` 主题：

```bash
pip install sphinx_rtd_theme
```

该主题的示例如下：

```{image} sphinx.png
:width: 700
:align: center
```

### Sphinx 配置

快速开始一个 Sphinx 项目非常简单，只需要执行下述命令：

```bash
sphinx-quickstart
```

然后根据向导即可完成首次配置。

在开始书写文档之前，我们首先需要完成 Sphinx 和插件的配置，只需要修改 `conf.py` （该文件位于 `/source` 目录下）即可。

```python
# Configuration file for the Sphinx documentation builder.
#
# This file only contains a selection of the most common options. For a full
# list see the documentation:
# https://www.sphinx-doc.org/en/master/usage/configuration.html

# -- Path setup --------------------------------------------------------------

# If extensions (or modules to document with autodoc) are in another directory,
# add these directories to sys.path here. If the directory is relative to the
# documentation root, use os.path.abspath to make it absolute, like shown here.
#

# -- Project information -----------------------------------------------------

project = 'CodeNote'
copyright = '2022, hui_zhou'
author = 'hui_zhou'

# The full version, including alpha/beta/rc tags
release = '0.0.1'


# -- General configuration ---------------------------------------------------

# Add any Sphinx extension module names here, as strings. They can be
# extensions coming with Sphinx (named 'sphinx.ext.*') or your custom
# ones.
extensions = ['sphinx.ext.autodoc', 'sphinx.ext.viewcode', 'myst_parser']

myst_enable_extensions = [
    "amsmath",
    "colon_fence",
    "deflist",
    "dollarmath",
    "fieldlist",
    "html_admonition",
    "html_image",
    "linkify",
    "replacements",
    "smartquotes",
    "strikethrough",
    "substitution",
    "tasklist",
]

# Add any paths that contain templates here, relative to this directory.
templates_path = ['_templates']

# List of patterns, relative to source directory, that match files and
# directories to ignore when looking for source files.
# This pattern also affects html_static_path and html_extra_path.
exclude_patterns = []


# -- Options for HTML output -------------------------------------------------

# The theme to use for HTML and HTML Help pages.  See the documentation for
# a list of builtin themes.
#
source_suffix = ['.rst', '.md', '.MD']

html_theme = 'sphinx_rtd_theme'

# Add any paths that contain custom static files (such as style sheets) here,
# relative to this directory. They are copied after the builtin static files,
# so a file named "default.css" will overwrite the builtin "default.css".
html_static_path = ['_static']
```

其中 `extensions` 列出了我们需要开启的扩展， `myst_enable_extensions` 表示 `myst-parser` 的子扩展，`source_suffix` 列出了作为文档的后缀，`html_theme` 指明我们想要使用的网页主题。

:::{note}
如果你在 `myst_enable_extensions` 中列出了 `linkify`，请提前安装 `myst-parser[linkify]`。
:::

### reStructuredText 语法

`reStructuredText` 标记语言的语法可以看[这里](https://ebf-contribute-guide.readthedocs.io/zh_CN/latest/rest-syntax/base-syntax.html)。

### 生成 html

在完成文档的书写之后，可以通过运行下述命令（在 `/docs` 目录下面执行）来制作离线文档并预览。

```bash
make html
```

## ReadTheDocs 教程

### 文档部署

将 [Github](https://github.com/) 和 [readthedocs](https://readthedocs.org/) 联用可以实现文档的自动生成和部署，详细步骤如下：

1. 在 `根目录` 下面准备一个 `.readthedocs.yaml` 文件；

2. 将文档推送到 `Github`；

3. 将 `Github` 项目与 `readthedocs` 网站关联。

`.readthedocs.yaml` 示例:

```yaml
# .readthedocs.yaml
# Read the Docs configuration file
# See https://docs.readthedocs.io/en/stable/config-file/v2.html for details

# Required
version: 2

# Set the version of Python and other tools you might need
build:
  os: ubuntu-20.04
  tools:
    python: "3.9"
    # You can also specify other tool versions:
    # nodejs: "16"
    # rust: "1.55"
    # golang: "1.17"

# Build documentation in the docs/ directory with Sphinx
sphinx:
  configuration: docs/source/conf.py
# If using Sphinx, optionally build your docs in additional formats such as PDF
# formats:
#    - pdf

# Optionally declare the Python requirements required to build your docs
# python:
#   install:
#     - requirements: requirements.txt
#     - method: setuptools
#       path: .
```

### 文档 pdf 下载

通过下面的步骤可以将在线文档以 pdf 格式下载：

1. 在 `.readthedocs.yaml` 中添加 `format` 和 `apt_packages` 键；

```yaml
# Set the version of Python and other tools you might need
build:
  os: ubuntu-20.04
  tools:
    python: "3.9"
    # You can also specify other tool versions:
    # nodejs: "16"
    # rust: "1.55"
    # golang: "1.17"
  apt_packages:
    - inkscape

# If using Sphinx, optionally build your docs in additional formats such as PDF
formats:
  - pdf
```

其中，`format`表示我们想制作 `pdf` 版本的文档， `apt_packages` 是为了安装 `inkscape` 软件，该工具可以将 `svg` 图片进行转换以使 `latexpdf` 支持。

2. 在 `conf.py` 中添加扩展 `sphinxcontrib.inkscapeconverter`;

```python
extensions = ['sphinx.ext.autodoc', 'sphinx.ext.viewcode', 'sphinxcontrib.inkscapeconverter']
```

3. 修改 `requirements.txt` 安装该扩展；

4. 至此，便可以实现 `pdf` 格式的文档下载。
