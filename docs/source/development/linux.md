# Linux 系统相关

## 系统相关命令

- 查看系统版本

```bash
>>> uname -a
>>> cat /etc/redhat-release  # RedHat 系统专属
```

- 查看系统架构

```bash
>>> arch
```

## CentOS6 系统相关

对于 CentOS6 系统来说，官方已经停止维护，但部分程序还会在部署该系统的服务器上运行，但升级成本（难度）过大，且时常会遇到一些系统底层相关的问题，故在此记录。

### yum 更新

因为官方已经停止了 yum 的维护，但部署该系统的服务器安装软件时 yum 工具仍是第一选择，所以为了让 yum 继续在该系统上运行，需要执行以下操作：

1. 禁用镜像加速插件

```bash
sed -i "s|enabled=1|enabled=0|g" /etc/yum/pluginconf.d/fastestmirror.conf
```

2. 备份基础源

```bash
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

3. 更换为阿里源 （如报错，增加 `-k` 选项）

```bash
curl -o /etc/yum.repos.d/CentOS-Base.repo https://www.xmpan.com/Centos-6-Vault-Aliyun.repo
```

4. 清除 yum 缓冲

```bash
yum clean all
```

5. 生成缓存

```bash
yum makecache
```

通过这几步操作后，yum 应该就可以正常使用了。

### 修复动态库版本

对于 `CentOS6` 系统来说，它默认的 `libc.so.6` 最高只支持 `GLIBC_2.12`，对于某些在较高系统下编译的软件，经常会报 `version 'GLIBC_2.14' not found` 这个错误，因此就导致软件无法正常使用，似乎除了更新系统库就别无他法了（但这稍有不慎可能会导致整个系统崩溃，因此不推荐使用）。

幸运的是通过 [patchelf](https://github.com/NixOS/patchelf) 我们可以使用自己安装的 glibc 对软件的动态库查找路径进行替换而对系统不产生任何影响，具体用法如下：

- 替换动态库链接器

```bash
patchelf --set-interpreter /path/ld-2.14.so /application
```

- 替换动态库（如 `libc`）

```
patchelf --replace-needed libc.so.6 /path/libc-2.14.so ./application
```

其中，`application` 为要进行修改的程序或 `so` 文件，`/path/xx` 为用户自己安装的系统库文件

## GLIBC 安装

1. 下载对应版本的 `GLIBC`，下载地址[在这](http://ftp.gnu.org/gnu/glibc/)

2. 解压 `glibc-xx.tar.gz` 文件，同时在解压目录下新建一个 `build` 目录

```bash
>>> tar -zxvf glibc-xx.tar.gz
>>> cd glibc-xx
```

3. 执行 configure 命令

```bash
../configure --prefix=/your-install-path
```

4. 编译及安装

```bash
make -j 32 && make install
```

5. 删除源代码

```bash
rm -rf glibc-xx
```
