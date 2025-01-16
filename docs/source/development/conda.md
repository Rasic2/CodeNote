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

## Bug 一览

- 执行命令时显示 "DEBUG:urllib3.connectionpool:Starting new HTTPS connection (1): repo.anaconda.com:443"

问题原因：conda-build 包存在问题

解决办法：

```bash
conda install "conda-build!=3.26.0"
```

## 参考

- [遇到EBUG:urllib3.connectionpool:Starting new HTTPS connection (1): repo.anaconda.com:443的解决方法](https://blog.csdn.net/weixin_73141818/article/details/133794445)

- [DEBUG:urllib3.connectionpool:Starting new HTTPS connection (1): repo.anaconda.com:443](https://www.cnblogs.com/liujiaxin2018/p/17725178.html)