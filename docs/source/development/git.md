# Git 教程

[Git](https://git-scm.com/) 是一个免费开源的分布式版本控制系统，旨在快速高效地处理从小型项目到大型项目的所有事务。

## Git 常用命令

### 初始化及配置

- 初始化 git 本地仓库

```bash
git init
```

- 显示仓库的配置

```bash
git config -l
```

- 配置颜色

```bash
git config --global color.ui true
```

- 配置编辑器为 vim

```bash
git config --global core.editor vim
```

- 给远程仓库地址添加别名

```bash
git remote add origin git@github.com:Rasic2/gvasp.git
```

其中 `origin` 代表给远程仓库添加的别名, `git@github*` 代表远程仓库的地址。

### 提交相关

- 显示工作目录和暂存区的状态

```bash
git status
```

- 将文件添加到暂存区

```bash
git add .
```

- 将暂存区的内容添加到本地仓库

```bash
git commit -m "comment"
```

其中 `comment` 表示对本次版本更新的标注。

- 显示历史日志

```bash
git log
git log --stat    # 查看所有提交记录的修改文件信息
git log -p file   # 查看某个文件的修改历史
```

- 修改 commit 的最后一条注释

```bash
git commit --amend
```

- 将本地仓库的内容同步到远程

```bash
git push origin master
```

其中 `origin` 代表远程仓库的别名, `master` 代表一个分支。

- 将远程仓库的内容同步到本地

```bash
git pull origin master
```

- 将工作区的内容缓存

```bash
git stash
```

- 显示缓存区记录

```bash
git stash list
```

- 将缓存区的第一条记录恢复到工作区

```bash
git stash pop
```

### 分支相关

- 删除本地分支

```bash
git branch -d branch_name
```

### tag 相关

- 查看已有的 tag

```bash
git tag
```

- 为当前 commit 增加 tag（如 v0.0.3）

```bash
git tag v0.0.3
```

- 后期为某个版本号增加 tag（如 v0.1.1）

```bash
git tag -a v0.1.1 COMMIT_ID -m "message"
```

- 删除已有的 tag（如 v0.0.1）

```bash
git tag -d v0.0.1  # 删除本地 tag
git push origin :refs/tags/v0.0.1  # 删除远程 tag
```

- 将本地 tag 提交到远程仓库

```bash
git push origin --tags
```

- 从远程仓库获取 tag

```bash
git fetch origin --tags
```

## Git 删除大文件历史

当使用 Git 进行版本管理时，可能会不慎将一些库文件、配置文件或特大文件加入版本管理而使得 `.git` 文件夹占用空间过大。因此特别有必要对一些 git 历史进行重写，删除不想被 git 追踪的文件，官方推荐使用的工具是 `git-filter-repo`（可使用 pip 或 conda 进行安装），下面将对该工具的一些具体用法进行说明。

- 对 git 历史进行分析

```bash
git filter-repo --analyze
```

首次运行该命令会生成 `.git/filter-repo` 目录，可查看该目录获取文件的大小和历史名字，如 `.git/filter-repo/analysis/path-all-sizes.txt`：

```text
=== All paths by reverse accumulated size ===
Format: unpacked size, packed size, date deleted, path name
       35149      12121 <present>  LICENSE
         291        209 <present>  README.md
```

::: {note}
后续分析时覆盖该目录需加入参数 `--force`。
:::

- 删除文件

```bash
git filter-repo --path foo.zip --invert-paths
```

该命令表示将 `foo.zip` 从 git 历史中移除。

## 更多参考资源

[1. 常用 Git 命令总结](https://zhuanlan.zhihu.com/p/384819351)

[2. Git 教程](https://www.runoob.com/git/git-tutorial.html)

[3. Git reset 命令的使用](https://www.jianshu.com/p/cbd5cd504f14)

[4. 细读 Git | 让你弄懂 origin、HEAD、FETCH_HEAD 相关内容](https://developer.aliyun.com/article/919354)

[5. GitHub fork 与 pull request](https://blog.csdn.net/benben0729/article/details/83031135)
