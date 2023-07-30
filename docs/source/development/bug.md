# bug 一览

- `Pycharm` 的快捷键 `Ctrl+Alt+L` 与网易云音乐不兼容
- 使用 `conda-build` 制作包时注意 conda 的版本，发现 `conda 4.14.0` 和 `conda-build 3.22.0` 不兼容, 需降级为 `conda 4.12.0`，详情看[这里](https://github.com/conda/conda-build/issues/4484)
- 使用环境中的 `conda` 命令需要指明路径，例如, `~/anaconda3/envs/gvasp-build/bin/conda`，否则使用的还是`base`下的`conda`命令。
- 当使用 `conda env list` 之后显示 `[y/N]`，后续不论输入 y 还是 N，都没有任何反馈时，执行`conda init`可解决。
- `Pycharm` 中 `运行/调试设置-执行` 不要勾选任何选项，否则 `\033[1;31m` 等颜色代码乱码

```{image} pycharm.png
:width: 500
:align: center
```

- `VSCode` 中的 `Markdown Preview Enhanced` 插件出现无法激活的现象时，卸载插件-关闭 VSCode-打开 VSCode-安装插件，不要直接在 VSCode 中重新加载！！！

- `VSCode` 终端使用 `Oh-my-zsh` 出现乱码问题时，修改 `Setting-Terminal-Font Family` 的字体即可

```{image} vscode-terminal.png
:width: 500
:align: center
```
