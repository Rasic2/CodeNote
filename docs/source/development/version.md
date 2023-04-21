# 版本管理

## 介绍

[bumpversion](https://github.com/c4urself/bump2version/#installation) 是一个小型的命令行工具，用于简化应用发布时的版本控制，它具有以下几个功能：

- 根据命令升级版本号；
- 自动替换项目相关文件中的版本信息；
- 自动创建一次 Git 提交；
- 为这一次提交打上版本升级的标签；

## 包安装

使用 pip 可以很容易的进行 `bumpversion` 的安装：

```bash
pip install bumpversion
```

## 使用

当安装好 `bumpversion` 后，通过在项目目录下新建一个`.bumpversion.cfg` 文件来进行配置管理，具体可见下述文件：

```ini
[bumpversion]
current_version = 0.1.4.beta
parse = (?P<major>\d+)\.(?P<minor>\d+)\.(?P<patch>\d+)\.(?P<release>.*)
serialize =
	{major}.{minor}.{patch}.{release}

[bumpversion:part:release]
values =
	alpha
	beta
	gamma

[bumpversion:file:setup.py]
search = version='{current_version}'
replace = version='{new_version}'

[bumpversion:file:conda/meta.yaml]
search = version: {current_version}
replace = version: {new_version}

[bumpversion:file:gvasp/common/constant.py]
search = Version = "{current_version}"
replace = Version = "{new_version}"

[bumpversion:file:docs/source/conf.py]
search =
	version = '{current_version}'
	release = '{current_version}'
replace =
	version = '{new_version}'
	release = '{new_version}'
```

当配置好这一文件之后，便可以通过在命令行中执行下列命令来进行版本更换：

```bash
bumpversion release  # release 为 cfg 中新加的命令，默认为 major、minor 和 patch
bumpversion release --allow-dirty  # 允许工作目录未 commit
```

## 参考

- [使用 bumpversion 管理版本](https://zhuanlan.zhihu.com/p/99505381)
