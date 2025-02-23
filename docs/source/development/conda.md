# Conda 教程

## 基本用法

- 删除某个虚拟环境

```bash
conda remove -n env_name --all
```

- 重命名环境

```bash
conda rename -n old_env new_env
```

## 更新 conda

使用下述命令更新 conda：

```bash
conda update -n base -c conda-forge conda
```

如果未能更新到最新版本的 conda，可能存在包冲突，删除 base 环境的部分包来安装最新版的 conda：

```bash
# 获取 base 环境中的所有包
packages=$(conda list --name base --export | grep -v "^#" | cut -d "=" -f 1)

# 定义需要保留的核心包
core_packages=("conda" "python" "pip" "setuptools" "wheel")

# 移除非核心包
for package in $packages; do
    if [[ ! " ${core_packages[@]} " =~ " ${package} " ]]; then
        conda remove --name base $package --yes
    fi
done
```

## Conda 包制作相关

- 预览 package 元信息

```bash
conda-render meta.yaml
version=0.1.6.alpha conda-render meta.yaml  # 传递环境变量
```

## Bug 一览

- 执行命令时显示 "DEBUG:urllib3.connectionpool:Starting new HTTPS connection (1): repo.anaconda.com:443"

问题原因：conda-build 包存在问题

解决办法：

```bash
conda install "conda-build!=3.26.0"
```

## 参考

- [遇到 EBUG:urllib3.connectionpool:Starting new HTTPS connection (1): repo.anaconda.com:443 的解决方法](https://blog.csdn.net/weixin_73141818/article/details/133794445)

- [DEBUG:urllib3.connectionpool:Starting new HTTPS connection (1): repo.anaconda.com:443](https://www.cnblogs.com/liujiaxin2018/p/17725178.html)
