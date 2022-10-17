# Windows 子系统（WSL）教程

WSL 是 Windows 下的 Linux 子系统，可以代替虚拟机来运行 Linux 系统，占用资源少，使用方便。

## WSL 迁移

对于 WSL 来说，默认安装位置是 C 盘，如果你想将其迁移到其他磁盘中，可以按照如下步骤执行：

1. 终止正在运行的分发或虚拟机

```bash
wsl --shutdown
```

2. 将需要迁移的分发或虚拟机导出（如 Ubuntu-22.04）

```
wsl --export Ubuntu-22.04 D:\wsl-Ubuntu-22.04
```

此命令执行后 `Ubuntu-22.04` 会导出为 D 盘中的 `wsl-Ubuntu-22.04`

3. 卸载分发版或虚拟机

```bash
wsl --unregister Ubuntu-22.04
```

4. 导入新的分发版或虚拟机

```
wsl --import Ubuntu-22.04 D:\wsl\Ubuntu2004 D:\wsl-Ubuntu-22.04 --version 2
```

其中第二个参数代表 `Ubuntu-22.04` 的迁入位置

:::{note}
当 WSL 中除了 Ubuntu 还有 docker 时，卸载 Ubuntu 后，docker 会成为 WSL 的默认分发（即执行 `wsl -l -v` 时左侧有星号），重新导入迁移后的 Ubuntu 并不会自动成为 WSL 的默认分发，这时候需要设置 WSL 为默认分发，即执行命令：`wsl --setdefault Ubuntu-22.04`，参考[这里](https://zhuanlan.zhihu.com/p/337361570)。
:::

## XShell 连接 WSL2

1. 重新安装 ssh

```bash
sudo apt-get remove --purge openssh-server ## 先删 ssh
sudo apt-get install openssh-server ## 在安装 ssh
sudo rm /etc/ssh/ssh_config ## 删配置文件，让 ssh 服务自己想办法链接
sudo service ssh --full-restart
```

2. 修改配置文件

```
sudo vim /etc/ssh/sshd_config
```

修改如下（取消注释）：

```bash
Port 22
ListenAddress 0.0.0.0
PasswordAuthentication yes
```

3. 重启 ssh（每次重启 wsl 都要执行该语句）

```bash
sudo service ssh --full-restart
```

4. 重新生成 host key

```bash
sudo dpkg-reconfigure openssh-serve
```

5. 设置 XSell 中主机地址（127.0.0.1）、用户名和密码等

### 设置开机启动 WSL 的 SSH 服务

1. 新建一个 powershell 脚本（AutoStartSSH.ps1），内容为：

```powershell
wsl -u root service ssh start
```

2. 选择此电脑-管理-任务计划程序-创建基本任务，将该脚本添加到计划任务中，步骤如下：

```{image} wsl1.png
:width: 500
:align: center
```

```{image} wsl2.png
:width: 500
:align: center
```

```{image} wsl3.png
:width: 500
:align: center
```

```{image} wsl4.png
:width: 500
:align: center
```

```{image} wsl5.png
:width: 500
:align: center
```

3. 使用最高权限运行脚本，之后就大功告成了。

```{image} wsl6.png
:width: 500
:align: center
```

### 参考

- [XShell 初次连接 WSL2 教程](https://blog.csdn.net/qq_42437577/article/details/110664557)
- [开机启动 WSL2 的 SSHD 服务](http://lyallchan.github.io/2021/01/24/%E5%BC%80%E6%9C%BA%E5%90%AF%E5%8A%A8WSL2%E7%9A%84SSHD%E6%9C%8D%E5%8A%A1/)
