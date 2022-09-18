# PowerShell 教程

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
