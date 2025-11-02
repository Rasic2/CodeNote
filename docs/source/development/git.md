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

- 同步远程分支

```bash
git fetch --prune  # 当远程仓库上已经删除了某个分支，但本地的 `git branch -r` 仍然显示该分支
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

- 撤销操作

```bash
git revert -n OLDER_COMMIT^...NEWER_COMMIT  # 撤销从 OLDER_COMMIT 到 NEWER_COMMIT 的所有 COMMIT，并且保存为一次 COMMIT
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

### 冲突相关

- 对于冲突的文件，使用合并进来的分支（如 `main`）覆盖当前分支（如`test`）

  1.  确保你正处于合并冲突的状态中，并且当前分支是 test。
  2.  对于每一个冲突文件，使用 --theirs 选项来检出 main 的版本。
      :::{note}

      - 在合并操作中，--theirs 代表要合并进来的分支（即 main）的版本。
      - --ours 代表当前所在的分支（即 test）的版本。

      :::

  ```bash
  git checkout --theirs -- path/to/conflicted_file1.js
  git checkout --theirs -- path/to/conflicted_file2.css
  git checkout --theirs -- path/to/conflicted_file3.html
  ```

  3. 提交版本

  ```bash
  git add .
  git commit
  ```

### 历史相关

- 比较差异

```bash
git diff zyguo..main # 比较两个分支的差异
git diff abc123 def456 -- app.py # 比较某文件在两次提交中的差异
```

- 显示历史日志

```bash
git log
git log --stat    # 查看所有提交记录的修改文件信息
git log -p file   # 查看某个文件的修改历史
git log COMMIT1..COMMIT2 -- file # 查看某个文件从 COMMIT1 到COMMIT2 所有的修改情况（概略信息）
git log COMMIT1..COMMIT2 -p file # 查看某个文件从 COMMIT1 到COMMIT2 所有的修改情况（详细信息）
git log -n 4 --pretty=format:"%s" # 查看最近4条提交记录并只显示提交信息
git log --pretty=format:"%s" main..dev/zhouh # 查看 dev/zhouh 分支相比 main 分支的差异
```

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

- 将一个文件的多处变更拆分为多个 commit

依次执行如下步骤：

1. 确保你的工作区是干净的，并且已经将想要拆分的文件修改完毕。

2. 执行交互式暂存命令：

```bash
git add -p <文件名>
```

或者，如果你想对所有修改过的文件进行操作，可以省略文件名：

```bash
git add -p
```

3. Git 会逐块（hunk）地显示你的更改，并询问你如何处理它。你会看到类似下面的提示：

```text
Stage this hunk [y,n,q,a,d,s,e,?]?
```

- y：暂存此区块。

- n：不暂存此区块。

- q：退出；不暂存这个区块和后面所有的区块。

- a：暂存这个区块和这个文件后面所有的区块。

- d：不暂存这个区块和这个文件后面所有的区块。

- s：分割这个区块。如果这个区块比较大，且包含多个逻辑变更，可以用这个选项将它拆分成更小的区块，然后再决定。

- e：手动编辑这个区块。这是最强大的选项，可以精确地选择要暂存哪些行。

4. 循环操作：对于第一个你想提交的功能，对所有相关的区块选择 y（或者 s 分割后选择 y）。对于不属于这个功能的区块，选择 n。

5. 完成第一次提交：

```bash
git commit -m “提交信息：功能 A”
```

6. 重复步骤 2-5：再次运行 git add -p，这次选择与第二个功能相关的区块进行暂存，然后完成第二次提交。

```bash
git add -p <文件名>
git commit -m “提交信息：功能 B”
```

7. 检查结果：使用 git log --oneline 查看是否成功创建了多个 commit。

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

## Git hook 相关

### 强制检查提交

1. 首先，进入你的 Git 仓库并创建或编辑 `.git/hooks/commit-msg` 文件：

```bash
#!/bin/bash

# 提交消息文件
COMMIT_MSG_FILE=$1

# 允许的前缀列表
PREFIXES="feat|fix|docs|style|refactor|build|chore|perf|test|ci|revert"

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

### Commit 前操作

1. 创建 `.git/hooks/pre-commit` 文件：

```bash
#!/bin/bash

# 获取当前分支名称
current_branch=$(git rev-parse --abbrev-ref HEAD)

# 获取远程分支的最新更新
git fetch origin $current_branch

# 比较本地和远程分支
local_commit=$(git rev-parse $current_branch)
remote_commit=$(git rev-parse origin/$current_branch)

if [ "$local_commit" != "$remote_commit" ]; then
   echo "Your local branch is not up to date with origin/$current_branch."
   echo "Please pull the latest changes before committing."
   exit 1
fi

# 如果一致，则允许提交
exit 0
```

2. 将这个钩子脚本设置为可执行：

```bash
chmod +x .git/hooks/pre-commit
```

### 本地保护 main 或 master 分支

1. 创建 `.git/hooks/pre-push`

```bash
#!/bin/bash

protected_branch="main"
current_branch=$(git symbolic-ref HEAD 2>/dev/null | sed -e 's,.*/\(.*\),\1,')

# 如果没有当前分支（比如在 detached HEAD 状态），直接退出
if [ -z "$current_branch" ]; then
    exit 0
fi

# 检查是否是推送到受保护分支
if [ "$current_branch" = "$protected_branch" ]; then
    # 获取推送的引用（分支或标签）
    while read local_ref local_sha remote_ref remote_sha
    do
        # 如果是标签推送，允许
        if [[ "$remote_ref" == refs/tags/* ]]; then
            continue
        fi

        # 如果是删除分支操作，允许
        if [ "$remote_sha" = "0000000000000000000000000000000000000000" ]; then
            continue
        fi

        # 如果是推送到受保护分支的提交，拒绝
        if [ "$remote_ref" = "refs/heads/$protected_branch" ]; then
            echo "ERROR: You are not allowed to push commits directly to the $protected_branch branch."
            echo "Use a feature branch and create a Pull Request."
            exit 1
        fi
    done
fi

exit 0
```

2. 将这个钩子脚本设置为可执行：

```bash
chmod +x .git/hooks/pre-push
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

### conventional-changelog

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

### github_changelog_generator

1. 安装 `Ruby` 和 `github_changelog_generator`

```bash
brew install ruby
gem install github_changelog_generator
```

2. 生成 `CHANGELOG.md`

```bash
github_changelog_generator -u github_project_namespace -p github_project
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

## 使用 opencommit 自动生成 commit

1. 使用 npm 安装 [opencommit](https://github.com/di-sukharev/opencommit):

```bash
nvm use 18
npm install -g opencommit
```

2. 配置 OPENAI

```bash
oco config set OCO_API_URL=https://xxxx.openai.azure.com
oco config set OCO_AI_PROVIDER='azure' OCO_MODEL='gpt-4o'
oco config set OCO_API_KEY=xxx
```

3. 【可选配置】

```bash
oco config set OCO_GITPUSH=false # 关闭自动 push
oco config set OCO_ONE_LINE_COMMIT=true # 单行 commit
oco config set OCO_OMIT_SCOPE=true # 省略 scope
oco config set OCO_TOKENS_MAX_INPUT=40960 # 修改输入最大 Token 数
```

4. 使用 oco 命令生成 commit

```bash
git add .
oco
```

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
