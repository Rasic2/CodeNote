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
git commit --amend --author "qwer <qwer@qq.com>" --no-edit
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

- 基于分支新建分支

```bash
git checkout -b new_branch  # 基于当下分支
git checkout -b new_branch old_branch  # 基于已有分支
```

- 删除本地分支

```bash
git branch -d branch_name
```

- 删除远程分支

```bash
git push origin --delete branch_name
```

- 重命名分支

```bash
git branch -m new_branch_name
```

- 合并分支

```bash
git merge branch_name
git rebase branch_name  # 变基
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

### 历史相关

- 合并分支（变基）

```bash
git rebase branch  # 区别于 git merge，该命令会删除原有 commit，新建 commit
```

- 梳理提交历史

```bash
git rebase -i HEAD~num  # 进入交互模式

- pick

重新安排 pick 命令的顺序会更改提交的顺序。如果选择不包括提交，则应删除整行。

- reword

使用后会提供更改提交信息的机会，提交所做的任何更改均不受影响。

- edit

添加或修改提交，可以将大型提交拆分为较小的提交，或者删除在提交中所做的错误更改。

- squash

该命令可以将两个或多个提交合并为一个提交。提交被压缩到其上方的提交中，有机会重新编写描述这两个更改的新提交消息。

- fixup

与 squash 类似，但提交仅合并到其上方的提交中，并且较早提交的消息用于描述所有更改。

- exec

可以对提交运行任意的 Shell 命令。
```

:::{note}
重新推送到远程仓库时，需要加上 `--force-with-lease`，不建议直接用 `--force`，详情可看[这里](https://blog.walterlv.com/post/safe-push-using-force-with-lease.html)。
:::

- 将一个 commit 拆分成多个

依次执行如下步骤：

```bash
git checkout -b new_branch_name commit_id  # 想拆分 commit 的版本号
git reset HEAD^  # 撤销上一个 commit 到工作区
git add some_files && git commit -m ""  # 新建多个 commit
git checkout branch  # 切换到想更改历史的分支
git rebase new_branch_name  # 变基到拆分 commit 的分支即可自动将某个 commit 拆分成多个
```

- 版本回退

```bash
git reset --hard commit_id  # 移动HEAD指针，并清空工作区和暂存区（git add）
git reset --soft commit_id  # 移动HEAD指针，并把版本差异放进暂存区
git reset --mixed commit_id # 移动HEAD指针，并把版本差异放进工作区
git reset  # 默认 mixed 模式
```

## 规范 Git 提交说明

使用 `git cz` 命令可以自动生成规范的 Git 提交，要使用该命令，需要以下几个步骤：

1. 安装 `nodejs` 和 `npm`（Ubuntu 系统）

```bash
sudo apt-get update
sudo apt-get install nodejs npm
```

2. 安装适配器

```bash
npm install -g cz-conventional-changelog
```

3. 在项目目录下初始化 `cz-conventional-changelog` 适配器

```bash
commitizen init cz-conventional-changelog --save --save-exact
```

4. 在 Git 提交时使用 `git cz` 命令

```bash
git add .
git cz
```

## 使用 Git hook 强制检查提交

1. 首先，进入你的 Git 仓库并创建或编辑 `.git/hooks/commit-msg` 文件：

```bash
#!/bin/bash

# 提交消息文件
COMMIT_MSG_FILE=$1

# 允许的前缀列表
PREFIXES="feat|fix|docs|style|refactor|chore|perf|test|ci|revert"

# 读取提交消息的第一行
COMMIT_MSG=$(head -n 1 "$COMMIT_MSG_FILE")

# 检查提交消息是否以允许的前缀开头
if ! echo "$COMMIT_MSG" | grep -qE "^($PREFIXES)"; then
    echo "错误：提交消息必须以以下前缀之一开头：$PREFIXES" >&2
    exit 1
fi
```

2. 将这个钩子脚本设置为可执行：

```bash
chmod +x .git/hooks/commit-msg
```

## 使用 Git 模版来初始化 hook 钩子

1. **创建一个模板目录：**

   ```bash
   mkdir -p ~/.git-templates/hooks
   ```

2. **添加 `commit-msg` 钩子：**

   ```bash
   vim ~/.git-templates/hooks/commit-msg
   ```

3. **在钩子文件中添加代码：**

   ```bash
   #!/bin/bash

   # 提交消息文件
   COMMIT_MSG_FILE=$1

   # 允许的前缀列表
   PREFIXES="feat|fix|docs|style|refactor|chore|perf|test|ci|revert"

   # 读取提交消息的第一行
   COMMIT_MSG=$(head -n 1 "$COMMIT_MSG_FILE")

   # 检查提交消息是否以允许的前缀开头
   if ! echo "$COMMIT_MSG" | grep -qE "^($PREFIXES)"; then
       echo "错误：提交消息必须以以下前缀之一开头：$PREFIXES" >&2
       exit 1
   fi
   ```

4. **确保钩子文件是可执行的：**

   ```bash
   chmod +x ~/.git-templates/hooks/commit-msg
   ```

5. **配置 Git 使用模板目录：**

   ```bash
   git config --global init.templateDir '~/.git-templates'
   ```

6. **初始化新的 Git 仓库：**

   ```bash
   git init my-new-repo
   ```

## 自动生成 CHANGELOG

使用 `conventional-changelog` 命令可以自动生成 `CHANGELOG`，要使用该命令，需要以下几个步骤：

1. 安装或者更新 `nodejs` 和 `npm`（Ubuntu 系统默认 `apt` 安装的版本较低，需要使用 `n` 升级到较高的版本）

```bash
sudo apt-get update
sudo apt-get install nodejs npm
sudo npm install -g n
sudo n stable  # 安装 nodejs stable
sudo n latest  # 或安装 nodejs latest
sudo n ls      # 查看已下载的安装版本
sudo n 18.21.1 # 切换版本
```

2. 安装 `conventional-changelog-cli`

```bash
npm install -g conventional-changelog-cli
```

3. 更新 `CHANGELOG.md`

```bash
touch CHANGELOG.md  # 首次需手动创建
npx conventional-changelog -p angular -i CHANGELOG.md -s -r 0
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

[6. git rebase，看这一篇就够了](https://juejin.cn/post/6969101234338791432)

[7. 【git 整理提交】git rebase -i 命令详解](https://blog.csdn.net/the_power/article/details/104651772)

[8. Git Reset 三种模式](https://www.jianshu.com/p/c2ec5f06cf1a)

[9. Cz 工具集使用介绍 - 规范 Git 提交说明](https://juejin.cn/post/6844903831893966856)

[10. git commit 、CHANGELOG 和版本发布的标准自动化](https://zhuanlan.zhihu.com/p/51894196)

[11. Ubuntu 安装最新版本 NodeJs 和 Npm 的方法](https://blog.csdn.net/weixin_55719805/article/details/128094550)
