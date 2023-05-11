# Matplotlib 教程

[Matplotlib](https://matplotlib.org/) 是一个用于在 Python 中创建静态、动画和交互式可视化的 Python 库。

## 图片尺寸和字体大小

matplotlib 中设置图形大小的语句如下：

```python
fig = plt.figure(figsize=(a, b), dpi=dpi)
```

其中，`figsize` 设置图形的大小，a 为图形的宽，b 为图形的高，单位为英寸；`dpi` 为设置图形每英寸的点数，真实图片的像素为 `a*b*dpi`

Matplotlib 中 每英寸点数（ppi）为 72，则宽度为 1 点的线将为 1/72 英寸宽，使用 fontsize 12 点的文本将是 12/72 寸高。更改 dpi 会缩放元素，在 72 dpi 时，1 宽度的线是 1 像素。在 144 dpi 时，这条线就是 2 像素。

:::{note}
改变图片尺寸并不会更改字体的大小，而放大 dpi 一倍则会使得图片和字体都放大一倍。
:::

## 获取图例对象

示例代码：

```python
plt.gca().axes.get_legend_handles_labels()
```

通过上述代码即可获取到图例对象（元组类型（线对象，线标签））。

其中，`plt.gca()` 返回 `Artist` 对象，`plt.gca().axes` 返回 `Axes` 对象。

## 添加字体

对于默认安装的 `matplotlib`，可能会缺少某些常用的字体，如 `Arial`，因此需要添加相关的字体并让 `matplotlib` 使用我们自行添加的字体，具体步骤如下（以 `Arial` 为例, 可在<a href="../_static/arial.ttf" target="_blank">此处</a>下载）：

1. 获取 matplotlib 的字体路径

```python
>>> import matplotlib
>>> matplotlib.matplotlib_fname()
```

2. 将 `Arial.ttf` 放进上述目录（`mpl-data`）的 `fonts` 目录（`ttf` 格式字体放在 `ttf` 目录下面）

3. 清空 matplotlib 的缓存，默认在 `~/.cache/matplotlib`，如果不在，可通过下述命令查看

```python
>>> import matplotlib
>>> matplotlib.get_cachedir()
```

4. 修改 `matplotlibrc` 的文件内容 (取消注释及添加相应字体)

```
font.family         : sans-serif  # 取消注释
font.sans-serif     : DejaVu Sans, Bitstream Vera Sans, Computer Modern Sans Serif, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif # 取消注释, 如没有字体则相应添加
```

### 参考

- [matplotlib 设置图形大小时 figsize 与 dpi 的关系](https://www.cnblogs.com/lijunjie9502/p/10327151.html#:~:text=%E7%BA%BF%E6%9D%A1%EF%BC%8C%E6%A0%87%E8%AE%B0%EF%BC%8C%E6%96%87%E6%9C%AC%E7%AD%89%E5%A4%A7%E5%A4%9A%E6%95%B0%E5%85%83%E7%B4%A0%E9%83%BD%E6%9C%89%E4%BB%A5%E7%A3%85%E4%B8%BA%E5%8D%95%E4%BD%8D%E7%9A%84%E5%A4%A7%E5%B0%8F%E3%80%82%20Matplotlib%20%E4%B8%AD%20%E6%AF%8F%E8%8B%B1%E5%AF%B8%E7%82%B9%E6%95%B0%EF%BC%88ppi%EF%BC%89%20%E4%B8%BA72%EF%BC%8C%E5%88%99%E5%AE%BD%E5%BA%A6%E4%B8%BA%201%20%E7%82%B9%E7%9A%84%E7%BA%BF%E5%B0%86%E4%B8%BA%201%2F72,12%20%E7%82%B9%E7%9A%84%E6%96%87%E6%9C%AC%E5%B0%86%E6%98%AF%2012%2F72%20%E5%AF%B8%E9%AB%98%E3%80%82%20%E4%B8%BA%E4%BA%86%E4%BE%BF%E4%BA%8E%E8%AF%B4%E6%98%8E%EF%BC%8C%E7%94%A8%20matplotlib%E7%BB%98%E5%88%B6%E7%9B%B8%E5%BA%94%E7%9A%84%E5%9B%BE%E5%BD%A2%EF%BC%8C%E5%A6%82%20%E8%A1%A81%20%E6%89%80%E7%A4%BA%E3%80%82)
- [matplotlib 画图字体缺少问题解决方法](https://mp.weixin.qq.com/s/y5UJec9-LAXXt8oHes1m3g)
- [Matplotlib 中的 Artist——你在浪费时间瞎百度之前应该知道的东西](https://zhajiman.github.io/post/matplotlib_artist/#%E6%98%AF%E6%97%B6%E5%80%99%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BD%A0%E7%9A%84%E9%BB%98%E8%AE%A4%E6%A0%B7%E5%BC%8F%E4%BA%86)
