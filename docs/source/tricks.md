# 奇怪的技巧

- `Pycharm` 的快捷键 `Ctrl+Alt+L` 与网易云音乐不兼容
- 使用 `conda-build` 制作包时注意 conda 的版本，发现 `conda 4.14.0` 和 `conda-build 3.22.0` 不兼容, 需降级为 `conda 4.12.0`，详情看[这里](https://github.com/conda/conda-build/issues/4484)
- 使用环境中的 `conda` 命令需要指明路径，例如, `~/anaconda3/envs/gvasp-build/bin/conda`，否则使用的还是`base`下的`conda`命令。
