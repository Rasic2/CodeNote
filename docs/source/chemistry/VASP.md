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

其中，$b_i$ 为 OUTCAR 中的倒格子常量（单位为$2\pi/Å$），KSPACING 的单位为$1/Å$，因此需要乘以$2\pi$。

### NELECT

该参数用于设置体系的电子数。

类型：实数

默认值：价电子数之和（来自于 POTCAR）

:::{note}
该参数设置为 0 无效，可用 0.0001 代替。
:::
