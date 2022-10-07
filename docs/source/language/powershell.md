# PowerShell 教程

## PowerShell 基础语法

- 定义变量

```powershell
$CondaDir = "conda"
```

- 获取当前目录的路径

```powershell
Get-Location
```

- 切换目录

```powershell
Set-Location directory
```

- 拷贝文件

```powershell
Copy-Item source destination
Copy-Item source1,source2,source3 destination  # 逗号分隔，没有空格！！
```

- 删除文件

```powershell
Remove-Item file
```

- 定义数组

```powershell
$Excluded = @("bld.bat", "build.sh", "meta.yaml")
```

- 获得文件集合

```powershell
Get-ChildItem -Path "."  # 当前路径下的所有文件
Get-ChildItem -Path "." -Exclude $Excluded  # 排除某些文件
Get-ChildItem -Path "." -Exclude $Excluded  | Remove-Item -Force -Recurse  # 对符合模式的文件（夹）递归删除
```

## PowerShell 设置环境变量

- 查看所有环境变量

```bash
ls env:
```

- 搜索环境变量

```bash
ls env:NODE*
```

- 查看单个环境变量

```bash
$env:NODE_ENV
```

- 添加/更新环境变量

```bash
$env:NODE_ENV=development
```

- 删除环境变量

```bash
del evn:NODE_ENV
```
