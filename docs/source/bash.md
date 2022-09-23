# Bash 脚本教程

## Bash 变量替换

假设我们定义了一个变量 `file`

```bash
file=/dir1/dir2/dir3/my.file.txt
```

我们可用 `${}` 来进行变量替换获得不同的值：

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
