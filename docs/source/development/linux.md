# Linux 运维相关

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

## yum 包管理器

yum 是一个在 Fedora 和 RedHat 以及 SUSE 中的 Shell 前端软件包管理器。

- ssh 安装

```bash
yum -y install openssh-clients
```

- service 安装

```bash
yum -y install initscripts
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

## ldd 命令

`ldd` 命令用于打印程序或者库文件所依赖的共享库列表。

### 参考

- [ldd 命令详解](https://blog.csdn.net/f_carey/article/details/109686310)

## strace 命令

`strace` 是一个可用于诊断、调试和教学的 Linux 用户空间跟踪器。我们用它来监控用户空间进程和内核的交互，比如系统调用、信号传递、进程状态变更等。

我用它查看过 `whoami` 执行未成功的问题，最后定位到的原因是缺少 `libnss_files.so.2` 这个动态库。

### 参考

- [strace 命令详解](https://www.cnblogs.com/machangwei-8/p/10388883.html)

## LOGO 制作

### figlet 命令

- 安装 figlet

```bash
sudo apt install figlet
```

- 效果展示

```bash
> figlet GVasp
  ______     __
 / ___\ \   / /_ _ ___ _ __
| |  _ \ \ / / _` / __| '_ \
| |_| | \ V / (_| \__ \ |_) |
 \____|  \_/ \__,_|___/ .__/
                      |_|

> figlet -c GVasp  # 居中
                           ______     __
                          / ___\ \   / /_ _ ___ _ __
                         | |  _ \ \ / / _` / __| '_ \
                         | |_| | \ V / (_| \__ \ |_) |
                          \____|  \_/ \__,_|___/ .__/
                                               |_|
> showfigfonts # 查看可用字体
```

### toilet 命令

- 安装 toilet

```bash
sudo apt install toilet
```

- 效果展示

```bash
> figlet GVasp | toilet -f term --gay  # 彩虹色（终端可见）
  ______     __
 / ___\ \   / /_ _ ___ _ __
| |  _ \ \ / / _` / __| '_ \
| |_| | \ V / (_| \__ \ |_) |
 \____|  \_/ \__,_|___/ .__/
                      |_|
```

## 问题记录

1. 使用 `sh 文件`安装 anaconda 时出现 **Error -3 from inflate: incorrect header check** 错误

解决办法：先将 sh 文件压缩之后再上传到服务器解压安装。

2. ~/.ssh/config 文件出现 **Bad owner or permissions** 错误

解决办法：文件权限位问题，设置 config 文件权限为 `600` （chmod 命令）。
