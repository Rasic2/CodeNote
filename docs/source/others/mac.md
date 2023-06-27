# MAC 系统相关

## On My ZSH 相关

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

## 参考资料

- [Mac 下复制 dmg 文件内的内容](https://blog.csdn.net/iteye_11434/article/details/82208921)

- [避免 Parallels Desktop Windows 虚拟机的快捷方式与 Mac 桌面共享](https://blog.csdn.net/qq_43444349/article/details/107307472)
