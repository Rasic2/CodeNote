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

### NBANDS

NBANDS 决定计算中的 KS 或 QP 轨道数目。

类型：整数

默认值：max(NELECT/2+NIONS/2,NELECT\*0.6)

:::{note}
并行计算时，该参数需设置为 CPU 的整数倍，否则设置无效。
:::

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

## Periodic NBO 分析

1. Makefile 修改 `MKLROOT` 为含有 `*.mod` 的 `include` 目录，如 `/home/apps/intel/composer_xe_2013.1.117/mkl/include/intel64/lp64`

2. [基组网站](https://www.basissetexchange.org/)

3. [NBO 项目](https://github.com/jrschmidt2/periodic-NBO)

## 报错解决方案

- forrtl: severe (174): SIGSEGV, segmentation fault occurred

解决办法：执行 `ulimit -s unlimited` 命令可解决

- projection.exe 报 `INFO on exit of ZHEEV 1303` 错误

解决办法：最后面的数是第 n 条能带开始无法计算本征值，设置 `NBANDS` 重新计算 wavefunctions.dat 可解决
