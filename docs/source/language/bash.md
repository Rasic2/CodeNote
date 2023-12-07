# Bash 脚本教程

## Bash 基础语法

- 定义变量（变量名和等号之间不能有空格）

```bash
a=10
```

## Bash 数组

- 定义数组

```bash
array=(1 2 3)
```

- 以命令返回结果定义数组

```bash
files=( $(ls) )
```

- 获得数组中的某个元素

```bash
a=${array[0]}
```

- 获得数组的长度

```bash
length=${#array[@]}
```

## Bash 运算

- 自增

```bash
c=$((++c))
```

- 加法

```bash
b=$[$a+1]
```

## Bash 判断语句

### 单中括号

- 判断大小

```bash
[ $COMP_CWORD -ge 7 ]
```

### 双中括号 (注意两侧空格)

- 正则匹配（变量）

```bash
[[ "$pre" =~ "-h" ]]
```

- 取反（不是`!~`）

```bash
[[ ! "$pre" =~ "-h" ]]
```

- 正则匹配（数组）

```bash
[[ "${COMP_WORDS[@]}" =~ "-s" ]]
```

- 逻辑判断

```bash
[[ "${COMP_WORDS[@]}" =~ "-s" || "${COMP_WORDS[@]}" =~ "--sequential" ]]
```

## Bash 循环语句

- for 循环

```bash
for (( i=0; i<${#array[@]}; i++ ))
do
    echo ${array[i]}
done
```

- while 循环

```bash
while [ $c -lt $COMP_CWORD ]
do
    c=$((++c))
done
```

## Bash 变量替换

使用下述命令可以实现字符串的变量替换：

```bash
${string/substring/replacement} # 替换第一个匹配项
${string//substring/replacement} # 替换全部匹配项
```

具体效果如下所示：

```bash
EXEC=/opt/vasp/bin/vasp_std
${EXEC/vasp_std/vasp_gam} => /opt/vasp/bin/vasp_gam
```

:::{note}
当省略第二个 `/` 时，将会把匹配到的字符串从变量中删除
:::

例如，我们有一个变量 `user`

```bash
user=root:x:0:0:root:/root:/bin/bash
```

使用下述命令会发生字符串的删除；

```bash
> ${user/r*t}
> :/bin/bash
```

```bash
> ${user//r\*t}
> :/bin/bash
```

```bash
> ${user//r??t}
> :x:0:0::/:/bin/bash
```

```bash
> ${user/r??t}
> :x:0:0:root:/root:/bin/bash
```

## Bash 字符串删除

假设我们定义了一个变量 `file`

```bash
file=/dir1/dir2/dir3/my.file.txt
```

我们可用 `${var#}/${var%}` 来进行字符串删除获得不同的值：

```bash
> ${file#_/}
> dir1/dir2/dir3/my.file.txt

> ${file##_/}
> my.file.txt

> ${file#\*.}        
> file.txt

> ${file##_.}        
> txt

> ${file%/_}        
> /dir1/dir2/dir3

> ${file%%/\*}        
> (空值)

> ${file%.\_}
> /dir1/dir2/dir3/my.file

> ${file%%.\_}        
> /dir1/dir2/dir3/my
```

## Bash 函数

- 将 ls 的结果放到 files 数组中

```bash
_files () {
    local c=0
    for file in `ls`
    do
        files[$c]=$file
        c=$((++c))
    done
    echo ${files[@]}
}
```

- 获取函数的返回值

```bash
files=( $(_files) )
```

## mkdir 命令

- 构建多级目录 (-p 参数)

```bash
mkdir -p /opt/intel/oneapi/**
```

## find 命令

- 通过管道传送多个命令 (显示指明 `bash -c`)

```bash
find DIR -iname '*.gro' -exec bash -c 'echo -n "{} "; grep UNL {} | wc -l' \;
```

- 查找重复文件

```bash
find -not -empty -type f -printf "%s\n" | sort -rn | uniq -d | xargs -I{} -n1 find -type f -size {}c -print0 | xargs -0 md5sum | sort | uniq -w32 --all-repeated=separate
```

## xargs 命令

- 参数替换（-I 选项）

```bash
ls | shuf -n 50 | xargs -I {} cp {} DIR
```

其中，`shuf` 命令随机选择 N 个文件

## awk 命令

- 条件语句

```bash
awk -F, '{if($5>2000){print $0}}' file
```

## Linux 自定义命令补全

实现 Linux 系统下自定义命令的补全需要借助 `bash-completion`，它是对 bash 补全功能的增强，可以实现对参数和包名的补全。

默认安装位置在 `/etc/bash_completion` (`/usr/share/bash-completion/completions`则是一些工具的补全实现，可作为参考)，如果没有安装，可以借助 apt 工具进行安装。

```bash
apt install bash-completion
```

### 自动补全脚本编写

实现自定义命令的补全主要借助两个命令（`complete` 和 `compgen`）以及几个环境变量（`COMPREPLY`，`COMP_WORDS` 和 `COMP_CWORD`），下面对他们的一些用法进行说明。

#### complete 命令

- 用自定义函数（\_gvasp）对自定义命令（gvasp）进行补全

```bash
complete -F _gvasp gvasp
```

- 取消自动添加空格

```bash
complete -o -F _gvasp gvasp
```

#### compgen 命令

- 对 cur 字符串从 opts 中进行补全

```bash
compgen -W "$opts" -- $cur
```

- 当未找到匹配项时，以 bash 自带的补全进行替换

```bash
compgen -o default -W "$opts" -- $cur
```

#### 重要环境变量

- `COMP_CWORD` 表示当前光标所在的索引
- `COMP_WORDS` 当前命令行所有单词组成的数组
- `COMPREPLY` 从该数组变量中产生后续的命令补全

#### 简单实现

```bash
_gvasp_submit() {
    local cur opts

    COMPREPLY=()
    pre=${COMP_WORDS[COMP_CWORD-1]}
    cur=${COMP_WORDS[COMP_CWORD]}
    if [[ "$pre" =~ "-" ]]; then
        opts=""
    else
        opts="opt con-TS chg wf dos freq md stm neb dimer -h --help"
    fi
    COMPREPLY=( $( compgen -W "$opts" -- $cur ) )
}
```

定义好的 `bash-completion` 脚本在 `~/.bashrc` 中定义并通过 `source` 命令执行即可。

## 参考资料

- [Manipulating Variables](https://tldp.org/LDP/abs/html/string-manipulation.html)
