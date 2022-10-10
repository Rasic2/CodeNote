# VMD 教程

- 记录命令

```tcl
log xx.txt
```

- 取消记录

```tcl
log off
```

- 显示边框

```tcl
pbc box
```

- 导入 gro 结构

```tcl
mol new {xx.gro} type {gro} first 0 last -1 step 1 waitfor 1
```

- 更改表示为 CPK

```tcl
mol modstyle 0 0 CPK 1.000000 0.300000 12.000000 12.000000
```
