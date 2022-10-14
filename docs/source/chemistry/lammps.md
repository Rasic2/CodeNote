# LAMMPS 教程

[LAMMPS](https://www.lammps.org/) 作为一个经典的分子动力学程序，可以模拟液体中的粒子，固体和气体的系综，也可以采用不同的力场和边界条件来模拟全原子，聚合物，生物，金属，粒状和粗料化体系；其可以计算的体系小至几个粒子，大到上百万甚至是上亿个粒子。

## 输入参数文件

LAMMPS 的输入参数文件其实是通过一系列命令来定义的，下面将对一些常用的命令进行介绍。

### units 命令

`units` 命令用来定义模拟过程中使用的**单位**类型，它决定了所有输入脚本、数据文件和所有输出到屏幕、日志文件以及 `dump` 文件中物理量的单位。

#### 语法

```bash
units style
```

其中 `style` 可选值为： `lj` `real` `metal` `si` `cgs` `electron` `micro` `nano`

#### 示例

```bash
units lj
units metal
```

#### 参数介绍

- lj 类型

  对于 lj 类型，所有的物理量都是没有单位的。不失一般性，LAMMPS 将基本量 mass、sigma、epsilon 和波尔兹曼常数设为 1。你所指定的质量、距离、能量就是这些基本量的倍数。下面给出了无单位量（用\*标出）与有单位量之间的换算关系。基于此，你可以将 lj 模拟无单位量转换为正常的物理量。

  - mass = mass 或 $m$，其中 $M^* = \frac{M}{m}$
  - distance = $\sigma$, 其中 $x^* = \frac{x}{\sigma}$
  - time = $\tau$, where $\tau^* = \tau \sqrt{\frac{\epsilon}{m \sigma^2}}$
  - energy = $\epsilon$, 其中 $E^* = \frac{E}{\epsilon}$
  - velocity = $\frac{\sigma}{\tau}$, 其中 $v^* = v \frac{\tau}{\sigma}$
  - force = $\frac{\epsilon}{\sigma}$, 其中 $f^* = f \frac{\sigma}{\epsilon}$
  - torque = $\epsilon$, 其中 $t^* = \frac{t}{\epsilon}$
  - temperature = reduced LJ temperature, 其中 $T^* = \frac{T k_B}{\epsilon}$
  - pressure = reduced LJ pressure, 其中 $P^* = P \frac{\sigma^3}{\epsilon}$
  - dynamic viscosity = reduced LJ viscosity, 其中 $\eta^* = \eta \frac{\sigma^3}{\epsilon \tau}$
  - charge = reduced LJ charge, 其中 $q^* = q \frac{1}{\sqrt{4 \pi \varepsilon_0 \sigma \epsilon}}$
  - dipole = reduced LJ dipole, moment 其中 $\mu^* = \mu \frac{1}{\sqrt{4 \pi \varepsilon_0 \sigma^3 \epsilon}}$
  - electric field = force/charge, 其中 $E^* = E \frac{\sqrt{4 \pi \varepsilon_0 \sigma \epsilon} \sigma}{\epsilon}$
  - density = mass/volume, 其中 $\rho^* = \rho \frac{\sigma^{dim}}{m}$

:::{note}
在 lj 单位类型下，通过命令 thermo_style 设置的热力学信息的输出是将能量对原子数量进行了归一化，即能量/原子，这可以通过命令 thermo_modify norm 进行修改。
:::

### dimension 命令

`dimension` 命令用来定义模拟的维度。

#### 语法

```bash
dimension N
```

其中，`N` 可选值 `2` `3`

#### 示例

```bash
dimension 2
```

:::{important}
该命令必须在模拟盒子建立（使用命令 `create_box` 或 `read_data`）之前使用。
:::

### atom_style 命令

atom_style body nparticle 2 6

read_data data.body

velocity all create 1.44 87287 loop geom

pair_style body/nparticle 5.0
pair_coeff \* \* 1.0 1.0

neighbor 0.5 bin
neigh_modify every 1 delay 0 check yes

fix 1 all nve/body
#fix 1 all nvt/body temp 1.44 1.44 1.0
fix 2 all enforce2d

compute 1 all body/local type 1 2 3
dump 1 all local 100 dump.body index c_1[1] c_1[2] c_1[3] c_1[4]

#dump 2 all image 1000 image.\*.jpg type type &

# zoom 1.6 adiam 1.5 body type 1.0 0

#dump_modify 2 pad 5

thermo 100
run 10000

## 坐标文件

针对输入文件中指定的不同原子类型（atom_style），坐标文件的书写有不同的格式要求，下面将对常用的原子类型 `atomic`、`charge` 和 `full` 如何书写坐标文件进行说明。

### atomic 类型

```{image} lammps.png
:width: 300
:align: center
```

atomic 类型书写时仅需要给出 `atom-ID` `atom-type` `x` `y` `z` 这 5 列信息即可。

### charge 类型

```{image} lammps2.png
:width: 500
:align: center
```

charge 类型书写时除了上述的 5 项之外，还要在 `atom-type` 后面加一列 `charge` 相关的信息。

### full 类型

```{image} lammps3.png
:width: 300
:align: center
```

full 类型除了上述的 6 项之外，还要在 `atom-ID` 后面加一列 `molecule-ID`，以及 `x` `y` `z` 后面加 `nx` `ny` `nz` 三列用于周期性原子的定位（可选）。

## 参考

- [LAMMPS—units 命令解析](https://zhuanlan.zhihu.com/p/410687074)
- [LAMMPS 翻译系列-dimension 命令](http://www.52souji.net/lammps-command-dimension.html)
- [lammps 不同类型 data 文件格式对比，以及不同类型 data 文件相互转换方法](https://zhuanlan.zhihu.com/p/420847294)
