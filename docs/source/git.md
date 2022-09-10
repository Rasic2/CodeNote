# Git Tutorial

[Git](https://git-scm.com/) is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

## Common commands

- initial git repository

```
git init
```

- list the configuration of the repository

```
git config -l
```

- add files in stage

```
git add .
```

- add stage in local repository

```
git commit -m "comment"
```

where `comment` represent the comment for this commit.

- list log

```
git log
```

- alias the remote repository

```bash
git remote add origin git@github.com:Rasic2/gvasp.git
```

where `origin` represent the alias of the remote repository, `git@github*` represent the url of the remote repository.

- sync the latest local repository to remote

```
git push origin master
```

where `origin` represent the alias of the remote repository, `master` represent the name of branch.

- sync the latest remote repository to local

```
git pull origin master
```

- store the code in workspace to stash buffer

```
git stash
```

- list stash buffer records

```
git stash list
```

- restore the first record of the stash buffer to workspace

```
git stash pop
```

Since the git-related knowledge is relatively rich, so this tutorial only lists the most common used commands. For more information, you can refer to the following links.

## Reference

[1. 常用 Git 命令总结](https://zhuanlan.zhihu.com/p/384819351)

[2. Git 教程](https://www.runoob.com/git/git-tutorial.html)

[3. Git reset 命令的使用](https://www.jianshu.com/p/cbd5cd504f14)

[4. 细读 Git | 让你弄懂 origin、HEAD、FETCH_HEAD 相关内容](https://developer.aliyun.com/article/919354)

[5. GitHub fork 与 pull request](https://blog.csdn.net/benben0729/article/details/83031135)
