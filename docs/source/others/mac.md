# MAC 系统相关

## 命令行相关

### sudo 命令

执行 `sudo` 命令后，仍然报 `Operation not permitted` 错误，与 MAC 系统的 **SIP 机制（System Integrity Protection）**有关

> 解决方案
>
> - 关闭 SIP（不推荐）
> - 使用 brew 重新安装软件，修改自定义软件的目录

### locate 命令

执行 `locate` 命令，出现下述提示后：

```
WARNING: The locate database (/var/db/locate.database) does not exist.
To create the database, run the following command:

  sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.locate.plist

Please be aware that the database can take some time to generate; once
the database has been created, this message will no longer appear.
```

依次执行下述命令：

```bash
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.locate.plist
sudo /usr/libexec/locate.updatedb
```

## on-my-zsh 相关

### command-not-found 配置

1. 使用 `brew` 安装 `command-not-found` 工具

```bash
brew tap homebrew/command-not-found
```

2. 在 `.zshrc` 文件中添加 `command-not-found` 插件

```text
# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git command-not-found)
```

### conda 命令自动补全

1、下载 `conda-zsh-completion` 插件

```bash
git clone https://github.com/esc/conda-zsh-completion $ZSH_CUSTOM/plugins/conda-zsh-completion
```

2、修改 `.zshrc` 文件,在文件中添加 `fpath` 路径

```bash
plugins=(git command-not-found)

fpath+=$ZSH_CUSTOM/plugins/conda-zsh-completion

source $ZSH/oh-my-zsh.sh
```

3、使修改生效

```bash
source ~/.zshrc
```

## dmg 格式文件查看

根据下述步骤可以查看 dmg 格式文件（压缩文件）里面包含的具体文件内容：

1. 双击打开 dmg 文件，mac 系统会自动加载 dmg，并在/Volume 下挂载一个磁盘

2. 打开终端，进入到/Volume 中新挂载的磁盘中

3. 执行 ls 命令，便可以看到 dmg 文件里的所有的文件或文件夹

## Parallels Desktop 相关

### 软件破解

Github 仓库地址：https://github.com/dreamncn/ParallelsDesktopCrack

Gitea 仓库地址：https://icrack.day/pdfm

### 避免虚拟机系统桌面文件映射到 MAC 系统

当使用 Parallels Desktop 进行虚拟机相关的操作时，部分虚拟机系统桌面上的文件会映射到 MAC 系统，看着很不方便，通过下述步骤即可进行清理：

1. 找到虚拟机系统中的 `C:\Users\公用\公用桌面` 文件夹（该文件夹默认隐藏，在 Win11 系统中可通过勾选 `查看-显示-隐藏的项目` 进行定位）

2. 将虚拟机系统中不想共享到 MAC 系统中的文件移动到上述文件夹中即可避免映射（需要管理员权限）

## 定时任务

MAC 系统可以使用 `launchctl` 命令创建定时任务，仅需创建一个执行脚本和 `plist 文件`（并放在合适的位置即可），具体的步骤和示例如下：

1. 创建执行脚本文件，示例如下：

```bash
#!/bin/bash

source ~/.bashrc
abspath=`realpath $0`
parent_dir=`dirname $abspath`
echo `date` >> $parent_dir/log

cd $parent_dir
conda activate fund_pro
python fund.py
conda activate base

echo 'finish' >> $parent_dir/log
```

2. 创建 `plist文件`，示例如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <!-- 名称，要全局唯一 -->
    <key>Label</key>
    <string>com.hzhou.fund.plist</string>
    <!-- 命令， 第一个为命令，其它为参数-->
    <key>ProgramArguments</key>
    <array>
      <string>/Users/hui_zhou/Project/FundPro/scheduled_mac.sh</string>
    </array>
    <key>RunAtLoad</key>
    <true />
    <!-- 运行时间 -->
    <key>StartCalendarInterval</key>
    <dict>
      <key>Minute</key>
      <integer>30</integer>
      <key>Hour</key>
      <integer>14</integer>
    </dict>
    <!-- 标准输出文件 -->
    <key>StandardOutPath</key>
    <string>/Users/hui_zhou/Project/FundPro/out.log</string>
    <!-- 标准错误输出文件 -->
    <key>StandardErrorPath</key>
    <string>/Users/hui_zhou/Project/FundPro/error.log</string>
  </dict>
</plist>
```

:::{important}
项目文件不要放在 ～/Desktop、～/Downloads、～/Documents 等 MAC 自带的目录下，最好在用户目录下新建目录（如～/Project），原因看[这里](https://cloud.tencent.com/developer/ask/sof/427570)
:::

3. 将 `plist 文件`复制/移动到 `～/Library/LaunchAgents 目录`下，并执行下述命令：

```bash
launchctl load -w *.plist  #加载定时任务
#launchctl unload *.plist  #卸载定时任务
#launchctl start *.plist  #立即开始定时任务
```

:::{note}
可以使用 [launchcontrol 工具](https://zhuanlan.zhihu.com/p/388287366)来可视化定时任务（收费）
:::

## 参考资料

- [ubuntu 下 conda 在 bash 和 zsh 终端下的自动补全设置](https://blog.csdn.net/zhanghm1995/article/details/120010254)

- [Mac 下复制 dmg 文件内的内容](https://blog.csdn.net/iteye_11434/article/details/82208921)

- [避免 Parallels Desktop Windows 虚拟机的快捷方式与 Mac 桌面共享](https://blog.csdn.net/qq_43444349/article/details/107307472)

- [Mac 使用 Launchctl 设置后台定时任务无效的解决方法](https://zhuanlan.zhihu.com/p/388287366)

- [MacOS:Launchd&LaunchDaemon&LaunchAgent&.plist 文件编写](https://blog.csdn.net/dddgggd/article/details/122599616)

- [launchctl ：MAC 下的定时任务](https://www.cnblogs.com/cangqinglang/p/17498858.html)

- [Mac 中的定时任务](https://cloud.tencent.com/developer/article/2208861)

- [Mac 创建定时任务](https://developer.aliyun.com/article/970774)

- [Mac 下 locate 命令使用问题](https://blog.csdn.net/aiyou3665/article/details/101434281)

- [Mac 权限问题，operation not permitted](https://zhuanlan.zhihu.com/p/393796032)
