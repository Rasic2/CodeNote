# VASP 软件教程

## INCAR 参数

### KSPACING

当 KPOINTS 文件不存在时，该参数可决定 K 点的数目。

类型：实数

默认值：0.5

自动生成的 K 点数目可根据下述公式进行推导：

$$
N_i = max(1, \frac{2\pi \times b_i}{KSPACING})
$$

其中，$b_i$ 为 OUTCAR 中的倒格子常量（单位为$2\pi/$Å），KSPACING 的单位为$1/$Å，因此需要乘以$2\pi$。

### NELECT

该参数用于设置体系的电子数。

类型：实数

默认值：价电子数之和（来自于 POTCAR）

:::{note}
该参数设置为 0 无效，可用 0.0001 代替。
:::

### LORBIT

该参数联合一个合适的 RWIGS 值, 来决定 PROCAR 和 PROOUT 文件的输出。

可选值： 0 | 1 | 2 | 5 | 10 | 11 | 12

默认值：None

| LORBIT | RWIGS tag | files written                                                                                   |
| ------ | --------- | ----------------------------------------------------------------------------------------------- |
| 0      | required  | DOSCAR and PROCAR                                                                               |
| 1      | required  | DOSCAR and lm-decomposed PROCAR                                                                 |
| 2      | required  | DOSCAR and lm-decomposed PROCAR + phase factors                                                 |
| 5      | required  | DOSCAR and PROOUT                                                                               |
| 10     | ignored   | DOSCAR and PROCAR                                                                               |
| 11     | ignored   | DOSCAR and lm-decomposed PROCAR                                                                 |
| 12     | ignored   | DOSCAR and lm-decomposed PROCAR+ phase factors                                                  |
| 13     | ignored   | DOSCAR and lm-decomposed PROCAR+ phase factors, choose best projector for each band             |
| 14     | ignored   | DOSCAR and lm-decomposed PROCAR+ phase factors, choose single projector for interval EMIN, EMAX |

对于 `LORBIT=10`，DOSCAR 只输出 spdf-轨道；对于 `LORBIT=11，12`， DOSCAR 会对 spdf-轨道进行拆分输出。

## 报错解决方案

- forrtl: severe (174): SIGSEGV, segmentation fault occurred

执行 `ulimit -s unlimited` 命令可解决
