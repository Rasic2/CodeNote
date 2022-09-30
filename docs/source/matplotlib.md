# Matplotlib 教程

[Matplotlib](https://matplotlib.org/) 是一个用于在 Python 中创建静态、动画和交互式可视化的 Python 库。

## 添加字体

对于默认安装的 `matplotlib`，可能会缺少某些常用的字体，如 `Arial`，因此需要添加相关的字体并让 `matplotlib` 使用我们自行添加的字体，具体步骤如下（以 `Arial` 为例, 可在<a href="arial.ttf" target="_blank">此处</a>下载）：

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

4. 修改 `matplotlibrc` 的文件内容 (取消注释及添加响应字体)

```
font.family         : sans-serif  # 取消注释
font.sans-serif     : DejaVu Sans, Bitstream Vera Sans, Computer Modern Sans Serif, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif # 取消注释, 如没有字体则相应添加
```

### 参考

- [matplotlib 画图字体缺少问题解决方法](https://mp.weixin.qq.com/s/y5UJec9-LAXXt8oHes1m3g)
