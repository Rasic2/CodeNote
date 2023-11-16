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

其中，`N` 可选值为： `2` `3`

#### 示例

```bash
dimension 2
```

:::{important}
该命令必须在模拟盒子建立（使用命令 `create_box` 或 `read_data`）之前使用。
:::

### atom_style 命令

`atom_style` 命令用来定义模拟过程中原子的类型，决定原子应该具有哪些属性。

#### 语法

```bash
atom_style style args
```

其中，`style` 可选值为： `amoeba` `angle` `atomic` **`body`** `bond` `charge` `dielectric` `dipole` `dpd` `edpd` `electron` `ellipsoid` `full` `line` `mdpd` `mesont` `molecular` `oxdna` `peri` `smd` `sph` `sphere` `bpm/sphere` `spin` `tdpd` `tri` `template` `wavepacket` `hybrid`

下述 `atom_style` 需要设置 `args` 参数，其余为空：

- **body**

  `args` 可选值为：`bstyle` `bstyle-args`

- **sphere**

  `args` 可选值为：`0` `1`

- **bpm/sphere**

  `args` 可选值为：`0` `1`

- **tdpd**

  `args` 可选值为：`Nspecies`

- **template**

  `args` 可选值为：`template-ID`

- **hybrid**

  `args` 可选值为：子类型的参数组合

`accelerated styles` 可选值为：`angle/kk` `atomic/kk` `bond/kk` `charge/kk` `full/kk` `molecular/kk` `spin/kk`

:::{important}
该命令必须在建立模拟盒子（使用命令 `read_data` 或 `read_restart` 或 `create_box`）之前使用。
:::

#### 示例

```bash
atom_style atomic
atom_style bond
atom_style full
atom_style body nparticle 2 10
atom_style hybrid charge bond
atom_style hybrid charge body nparticle 2 5
atom_style spin
atom_style template myMols
atom_style hybrid template twomols charge
atom_style tdpd 2
```

#### 参数介绍

下表列出了每种类型所包括的属性以及会用到这种类型的典型物理体系。所有类型都包括坐标、速度、原子 ID 和原子类型。

|  原子类型  |                         属性                         |               适用体系                |
| :--------: | :--------------------------------------------------: | :-----------------------------------: |
|   amoeba   |          molecular + charge + 1/5 neighbors          |  AMOEBA/HIPPO polarized force fields  |
|   angle    |                   bonds and angles                   |  bead-spring polymers with stiffness  |
|   atomic   |               only the default values                | coarse-grain liquids, solids, metals  |
|    body    | mass, inertia moments, quaternion, angular momentum  |           arbitrary bodies            |
|    bond    |                        bonds                         |         bead-spring polymers          |
|   charge   |                        charge                        |      atomic system with charges       |
| dielectric |               dipole, area, curvature                |   system with surface polarization    |
|   dipole   |               charge and dipole moment               |     system with dipolar particles     |
|    dpd     |      internal temperature and internal energies      |             DPD particles             |
|    edpd    |            temperature and heat capacity             |            eDPD particles             |
|  electron  |             charge and spin and eradius              |        electronic force field         |
| ellipsoid  |         shape, quaternion, angular momentum          |         aspherical particles          |
|    full    |                  molecular + charge                  |             bio-molecules             |
|    line    |             end points, angular velocity             |             rigid bodies              |
|    mdpd    |                       density                        |            mDPD particles             |
|   mesont   | mass, radius, length, buckling, connections, tube id |         mesoscopic nanotubes          |
| molecular  |         bonds, angles, dihedrals, impropers          |          uncharged molecules          |
|   oxdna    |                 nucleotide polarity                  |   coarse-grained DNA and RNA models   |
|    peri    |                     mass, volume                     |     mesoscopic Peridynamic models     |
|    smd     |    volume, kernel diameter, contact radius, mass     |     solid and fluid SPH particles     |
|    sph     |                    rho, esph, cv                     |             SPH particles             |
|   sphere   |           diameter, mass, angular velocity           |            granular models            |
| bpm/sphere |     diameter, mass, angular velocity, quaternion     | granular bonded particle models (BPM) |
|    spin    |                   magnetic moment                    |    system with magnetic particles     |
|    tdpd    |                chemical concentration                |            tDPD particles             |
|  template  |            template index, template atom             |  small molecules with fixed topology  |
|    tri     |           corner points, angular momentum            |             rigid bodies              |
| wavepacket |      charge, spin, eradius, etag, cs_re, cs_im       |                 AWPMD                 |

上面的类型中，除了 `sphere`, `ellipsoid`, `electron`, `peri`, `wavepacket`, `line`, `tri`, and `body` 定义的是**有限尺寸粒子**，其他类型定义的都是**点粒子**。

对于定义为**有限尺寸粒子**的原子类型，在设置质量时是对每一个粒子进行的；而对于**点粒子**，质量是对类型进行设置的。

- **body**

  在 LAMMPS 中，`body` 是广义的**有限尺寸粒子**。单个 `body` 可以表示复杂的实体，例如离散点的表面网格、子粒子的集合、可变形对象等。

  `body` 接受一个 `bstyle` 参数，可选值为：`nparticle` `rounded/polygon` `rounded/polyhedron`

  - **nparticle**

    `nparticle` 将 `body` 表示为具有可变数量 N 个子粒子的刚体。

    指定该样式时需要两个额外的参数：

    ```bash
    atom_style body nparticle Nmin Nmax
    ```

    其中，`Nmin` 表示最小子粒子数，`Nmax` 表示最大子粒子数，`Nmin` 和 `Nmax` 参数用于限制每个粒子使用的数据结构的大小。

    :::{note}
    对于该种样式的坐标文件编写，参见[手册](https://docs.lammps.org/Howto_body.html)。
    :::

### read_data 命令

导入 LAMMPS 模拟所需要的数据文件，该文件可以是 ASCII 文本格式或 gzip 压缩文件格式。

#### 语法

```bash
read_data file keyword args ...
```

其中，`file` 表示数据文件名；

`keyword` 可选参数为：`add` `offset` `shift` `extra/atom/types` `extra/bond/types` `extra/angle/types` `extra/dihedral/types` `extra/improper/types` `extra/bond/per/atom` `extra/angle/per/atom` `extra/dihedral/per/atom` `extra/improper/per/atom` `group` `nocoeff` `fix`

各关键词对应的参数为：

- **add**

  `args` 可选值为：`append` `IDoffset` `IDoffset` `MOLoffset` `merge`

- **offset**

  `args` 可选值为：`toff` `boff` `aoff` `doff` `ioff`

- **shift**

  `args` 可选值为：Sx Sy Sz

- **extra/atom/types**

  `args` 可选值为：#

- **extra/angle/types**

  `args` 可选值为：#

- **extra/dihedral/types**

  `args` 可选值为：#

- **extra/improper/types**

  `args` 可选值为：#

- **extra/bond/per/atom**

  `args` 可选值为：

- **extra/angle/per/atom**

  `args` 可选值为：

- **extra/dihedral/per/atom**

  `args` 可选值为：

- **extra/improper/per/atom**

  `args` 可选值为：

- **extra/special/per/atom**

  `args` 可选值为：

- **group**

  `args` 可选值为：`groupID`

- **nocoeff**

- **fix**

  `args` 可选值为：`fix-ID` `header-string` `section-string`

#### 示例

```bash
read_data data.lj
read_data ../run7/data.polymer.gz
read_data data.protein fix mycmap crossterm CMAP
read_data data.water add append offset 3 1 1 1 1 shift 0.0 0.0 50.0
read_data data.water add merge group solvent
```

### velocity 命令

该命令用于设置或改变原子组的速度。

#### 语法

```bash
velocity group-ID style args keyword value ...
```

其中，`group-ID` 代表想要设置速度的原子组序号；

`style` 可选参数值为：`create` `set` `scale` `ramp` `zero`

各种 style 对应的额外参数为：

- **create**

  `args` 可选值为：`temp` `seed`

- **set**

  `args` 可选值为：`vx` `vy` `vz`

- **scale**

  `args` 可选值为：`temp`

- **ramp**

  `args` 可选值为：`vdim` `vlo` `vhi` `dim` `clo` `chi`

- **zero**

  `args` 可选值为：`linear` `angular`

`keyword` 可选值为：`dist` `sum` `mom` `rot` `temp` `bias` `loop` `units`

各个 `keyword` 对应的可选值为：

- **dist**

  `value` 可选值为：`uniform` `gaussian`

- **sum**、**mom**、**rot**、**bias**

  `value` 可选值为：`no` `yes`

- **temp**

  `value` 可选值为：温度计算 ID

- **loop**

  `value` 可选值为：`all` `local` `geom`

- **rigid**

  `value` 可选值为：固定 ID

- **units**

  `value` 可选值为：`box` `lattice`

#### 示例

```bash
velocity all create 300.0 4928459 rot yes dist gaussian
velocity border set NULL 4.0 v_vz sum yes units box
velocity flow scale 300.0
velocity flow ramp vx 0.0 5.0 y 5 25 temp mytemp
velocity all zero linear
```

#### 参数介绍

- **create**

  对特定温度使用随机数生成一个速度系综，可使用关键词 `loop`。

### pair_style 命令

设置相互作用力场形式。

#### 语法

```
pair_style style args
```

其中，可使用的 `style` 列表可参考[在线手册](https://docs.lammps.org/pair_style.html)。

#### 示例

```
pair_style lj/cut 2.5
pair_style eam/alloy
pair_style hybrid lj/charmm/coul/long 10.0 eam
pair_style table linear 1000
pair_style none
```

#### 参数介绍

- **body/nparticle**

  该 style 与 `body`（atom_style 中定义）一起使用，用于计算粒子之间的相互作用。

  - 语法

  ```bash
  pair_style body/nparticle cutoff
  ```

### pair_coeff 命令

对一对或多对原子类型指定成对相互作用力场系数，取决于 `pair_style` 命令中 `style` 的选取。

#### 语法

```bash
pair_coeff I J args
```

#### 示例

```bash
pair*coeff 1 2 1.0 1.0 2.5
pair_coeff 2 * 1.0 1.0
pair*coeff 3* 1*2 1.0 1.0 2.5
pair_coeff * * 1.0 1.0
pair_coeff * * nialhjea 1 1 2
pair_coeff * 3 morse.table ENTRY1
pair_coeff 1 2 lj/cut 1.0 1.0 2.5 # (for pair_style hybrid)

labelmap atom 1 C
labelmap atom 2 H
pair_coeff C H 1.0 1.0 2.5
```

### neighbor 命令

该命令设置影响成对邻居列表构建的参数。对于 `cutoff` + `skin` 截止距离内的所有原子对都存储在列表中。通常，`skin` 越大，需要建立的邻居列表就越少，但每个时间步必须检查更多对以了解可能的力相互作用。

#### 语法

```bash
neighbor skin style
```

其中，`skin` 表示超过 `cutoff` 之后额外的距离；

`style` 表示选择何种算法构建邻接表，其的可选值为：`bin` `nsq` `multi` `multi/old`

#### 示例

```bash
neighbor 0.3 bin
neighbor 2.0 nsq
```

### neigh_modify 命令

此命令设置影响成对邻居列表的构建和使用的参数。

#### 语法

```bash
neigh_modify keyword values ...
```

其中，`keyword` 可选值为：`delay` or `every` or `check` or `once` or `cluster` or `include` or `exclude` or `page` or `one` or `binsize` or `collection/type` or `collection/interval`

对于每一个关键词，其取值如下：

- **delay N**

  距离上次 N 步后构建邻居表。

- **every M**

  表示每 M 步构建一次邻居表。

- **check yes**

  当部分原子移动超过 `skin` 定义距离的一半时构建邻居表。

- **once**、**cluster**、

  `value` 可选值：`yes` `no`

- **include**

  `value` 可选值：`group-ID`

- **exclude**

  `value` 可选值：`type M N` `group group1-ID group2-ID` `molecule/intra group-ID` `molecule/inter group-ID` `none`

- **page**、**one**

  `value` 可选值：`N`

- **binsize**

  `value` 可选值：`size`

- **collection/type**、**collection/interval**

  `value` 可选值：`N arg1 ... argN`

#### 示例

```
neigh_modify every 2 delay 10 check yes page 100000
neigh_modify exclude type 2 3
neigh_modify exclude group frozen frozen check no
neigh_modify exclude group residue1 chain3
neigh_modify exclude molecule/intra rigid
neigh_modify collection/type 2 1*2,5 3*4
neigh_modify collection/interval 2 1.0 10.0
```

### fix 命令

该命令表示在时间步长或最小化时对原子组进行操作。

#### 语法

```bash
fix ID group-ID style args
```

#### 示例

```
fix 1 all nve
fix 3 all nvt temp 300.0 300.0 0.01
fix mine top setforce 0.0 NULL 0.0
```

#### 子命令介绍

- **fix ID group-ID nve/body**

  执行 NVE 积分，更新原子组中各 `body` 粒子的位置、速度、方向和角速度。

- **fix ID group-ID nvt/body**

  使用 Nose/Hoover 恒温器执行 NVT 积分，更新原子组中各 `body` 粒子的的位置、速度、方向和角速度。

  - 语法

  ```
  fix ID group-ID nvt/body keyword value ...
  ```

  其中，`keyword` 可选值为：`temp` `iso` `aniso` `tri` `x` `y` `z` `xy` `yz` `xz` `couple` `tchain` `pchain` `mtk` `tloop` `ploop` `nreset` `drag` `ptemp` `dilate` `scalexy` `scaleyz` `scalexz` `flip` `fixedpoint` `update`

  - 参数介绍

    **temp Tstart Tstop Tdamp**
    其中，`Tstart` 和 `Tstop` 表示开始/结束运行的外部温度，`Tdamp` 表述温度阻尼系数（时间单位）

- **fix ID group-ID enforce2d**

  将原子组中每个原子的 z 维速度和力归零，这在运行 2d 模拟以确保原子不会从其初始 z 坐标移动时很有用。

### compute 命令

定义将在一组原子上执行的计算。计算得到的是瞬时值，这意味着它们是由当前时间步长或迭代的原子信息得出的。**定义计算不会执行计算**，相反，计算由其他 LAMMPS 命令根据需要时调用。

#### 语法

```bash
compute ID group-ID style args
```

#### 示例

```bash
compute 1 all temp
compute newtemp flow temp/partial 1 1 0
compute 3 all ke/atom
```

#### 子命令介绍

- **compute ID group-ID body/local**

  定义有关 `body` 子粒子属性相关的计算。

  - 语法

  ```bash
  compute ID group-ID body/local input1 input2 ...
  ```

  其中，`input` 的可选值为：`id` `type` `integer`

  `type` 表示 body 粒子的原子类型；

  `integer` 表示 body 类型定义的字段索引；

### dump 命令

按照指定的样式，将原子的物理量快照转储到一个或多个文件中。

#### 语法

```bash
dump ID group-ID style N file args
```

#### 示例

```bash
dump myDump all atom 100 dump.lammpstrj
dump myDump all atom/mpiio 100 dump.atom.mpiio
dump myDump all atom/gz 100 dump.atom.gz
dump myDump all atom/zstd 100 dump.atom.zst
dump 2 subgroup atom 50 dump.run.bin
dump 2 subgroup atom/mpiio 50 dump.run.mpiio.bin
dump 4a all custom 100 dump.myforce.* id type x y vx fx
dump 4a all custom 100 dump.myvel.lammpsbin id type x y z vx vy vz
dump 4b flow custom 100 dump.%.myforce id type c*myF[3] v_ke
dump 4b flow custom 100 dump.%.myforce id type c_myF[*] v*ke
dump 2 inner cfg 10 dump.snap.*.cfg mass type xs ys zs vx vy vz
dump snap all cfg 100 dump.config.\_.cfg mass type xs ys zs id type c_Stress[2]
dump 1 all xtc 1000 file.xtc
```

#### 子命令介绍

- **dump ID group-ID local N file**

  将局部属性输出。

  - 语法

  ```bash
  dump ID group-ID local N file args
  ```

  其中，`args` 的可能取值为：`index` `c_ID` `c_ID[I]` `f_ID` `f_ID[I]`

  - index 表示局部值的枚举；

  - c_ID 表示由 compute 命令定义的计算输出的向量；

  - c_ID[I] 表示第 I 列值；

  - f_ID 表示由 fix 命令定义的计算输出的向量；

  - f_ID[I] 表示第 I 列值；

- **dump ID group-ID image N file**

  将原子属性以图片形式输出。

  - 语法

  ```bash
  dump ID group-ID image N file color diameter keyword value ...
  ```

  其中，`color` 的值为 `type` 时，根据原子类型的索引使用默认颜色；

  `diameter` 的值为 `type` 时，使用默认原子直径 1.0；

  keyword 取值详细介绍如下：

  - **adiam size**

    根据 `size` 设置原子的直径。

  - **body = color bflag1 bflag2**

    `color` 为 `type` 时，使用默认颜色；根据 `bflag1` 和 `bflag2` 设定的尺寸绘制粒子。

  - **zoom zfactor**

    根据 `zfactor` 的值放大或缩小输出图片的尺寸。

- **dump ID group-ID custom N file**

  - 语法

  ```bash
  dump ID group-ID custom N file args
  ```

  其中，`args` 的可能取值为：`id`, `mol`, `proc`, `procp1`, `type`, `element`, `mass`,
  `x`, `y`, `z`, `xs`, `ys`, `zs`, `xu`, `yu`, `zu`,
  `xsu`, `ysu`, `zsu`, `ix`, `iy`, `iz`,
  `vx`, `vy`, `vz`, `fx`, `fy`, `fz`,
  `q`, `mux`, `muy`, `muz`, `mu`,
  `radius`, `diameter`, `omegax`, `omegay`, `omegaz`,
  `angmomx`, `angmomy`, `angmomz`, `tqx`, `tqy`, `tqz`,
  `heatflow`, `temperature`,
  `c_ID`, `c_ID[I]`, `f_ID`, `f_ID[I]`, `v_name`,
  `i_name`, `d_name`, `i2_name[I]`, `d2_name[I]`

  - id 表示原子 ID

  - type 表示原子类型

  - x,y,z 表示未缩放的原子坐标

  - fx,fy,fz 表示原子受力

  - vx,vy,vz 表示原子速度

### dump_modify 命令

修改 dump 命令的参数设置。

#### 语法

```bash
dump_modify dump-ID keyword values ...
```

#### 示例

```bash
dump_modify 1 format line "%d %d %20.15g %g %g" scale yes
dump_modify 1 format float %20.15g scale yes
dump_modify myDump image yes scale no flush yes
dump_modify 1 region mySphere thresh x < 0.0 thresh fx >= 3.2
dump_modify xtcdump precision 10000 sfactor 0.1
dump_modify 1 every 1000 nfile 20
dump_modify 1 every v_myVar
```

#### 子命令介绍

- **dump_modify dump-ID pad Nchar**

  使用通配符指定转储文件名时生效，如果 pad 为 0（默认），时间步将转换为未填充长度的字符串（例如，100 或 12000 或 2000000）。当 pad 指定为 Nchar 时，字符串用前导零填充，因此它们的长度都相同 = Nchar。

### thermo 命令

在 N 的倍数时间步长以及模拟开始和结束时计算和打印热力学信息（例如温度、能量、压力）。 值 0 表示仅在开始和结束时打印热力学信息。

#### 语法

```bash
thermo N
```

#### 示例

```bash
thermo 100
```

### run 命令

按照指定的时间步长进行动力学模拟。

#### 语法

```bash
run N keyword values ...
```

#### 示例

```bash
run 10000
run 1000000 upto
run 100 start 0 stop 1000
run 1000 pre no post yes
run 100000 start 0 stop 1000000 every 1000 "print 'Protein Rg = $r'"
run 100000 every 1000 NULL
```

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

## LAMMPS 三斜晶胞的定义与转换

### 定义

LAMMPS 支持在三斜晶胞中进行分子动力学模拟，晶胞原点位于 (xlo, ylo, zlo)，由从原点开始的 3 个边缘向量定义：

```
a = (xhi-xlo, 0, 0)
b = (xy, yhi-ylo, 0)
c = (xz, yz, zhi-zlo)
```

其中，`xy`，`xz`，`yz` 可以是 0.0 或正数或负数，称为“倾斜因子”，代表将最初的正交盒变换为平行六面体的面位移量。

:::{note}
在 LAMMPS 中，三斜晶胞的边缘向量 **a** 必须位于 _x_ 轴正方向；**b** 必须位于 _xy_ 平面上，且 _y_ 分量严格为正；**c** 可以具有严格正的 _z_ 分量的任何方向。总之，**a**、**b** 和 **c** 必须形成一个完整的右手基。
:::

### dump 文件中的晶胞输出

对于三斜晶胞，dump 文件输出的是包围三斜晶胞的正交边界盒子，以及三斜晶胞的 3 个倾斜因子`xy`，`xz` 和 `yz`，格式如下：

```
ITEM: BOX BOUNDS xy xz yz
xlo_bound xhi_bound xy
ylo_bound yhi_bound xz
zlo_bound zhi_bound yz
```

其中，`_bound` 后缀是为了区别正交晶胞而特意加的，表示 dump 文件输出的并不是“真正的”三斜晶胞，具体定义如下：

```
xlo_bound = xlo + MIN(0.0,xy,xz,xy+xz)
xhi_bound = xhi + MAX(0.0,xy,xz,xy+xz)
ylo_bound = ylo + MIN(0.0,yz)
yhi_bound = yhi + MAX(0.0,yz)
zlo_bound = zlo
zhi_bound = zhi
```

### 晶胞矩阵转化为 LAMMPS 的三斜晶胞

对于任意的晶胞矩阵（3×3），如果 **A**，**B**，**C** 三个晶胞矢量满足右手基，则可以通过下式将它们转为 LAMMPS 的边缘矢量 **a**，**b**，**c**：

$$
\begin{pmatrix}
 a & b & c
\end{pmatrix} = \begin{pmatrix}
 a_x & b_x & c_x \\
 0   & b_y & c_y \\
 0   & 0   & c_z
\end{pmatrix}
$$

$$
\begin{align}
& a_x = A \\
& b_x = \mathrm {B} \cdot \hat{\mathrm{A}} = B\cos \gamma \\
& b_y = |\hat{\mathrm{A}}\times \mathrm{B}| = B\sin \gamma = \sqrt{B^2-b_x^2} \\
& c_x = \mathrm{C} \cdot \hat{\mathrm{A}} = C \cos \beta \\
& c_y = \mathrm{C} \cdot (\widehat{\mathrm{A} \times \mathrm{B}}) \times \hat{\mathrm{A}} = \frac{\mathrm{B}\cdot\mathrm{C}-b_xc_x}{b_y} \\
& c_z = |\mathrm{C} \cdot (\widehat{\mathrm{A} \times \mathrm{B}})| = \sqrt{\mathrm{C}^2-c_x^2-c_y^2} \\
\end{align}
$$

Python 代码实现为：

```python
# coord是从VASP提取的晶格矢量3*3
coord = np.array(copy.deepcopy(system['box_coord']))

A_hat = coord[0]/math.sqrt(np.sum(np.square(coord[0])))
AcrossB = np.cross(coord[0],coord[1])
AcrossB_hat = AcrossB/math.sqrt(np.sum(np.square(AcrossB)))

x = float(math.sqrt(np.sum(np.square(coord[0]))))
xy = float(np.dot(coord[1],A_hat))
y = float(math.sqrt(np.sum(np.square(np.cross(A_hat,coord[1])))))
xz = float(np.dot(coord[2],A_hat))
yz = float(np.dot(coord[2],np.cross(AcrossB_hat,A_hat)))
z = float(np.dot(coord[2],AcrossB_hat))

coord = [[x,0,0],[xy,y,0],[xz,yz,z]]
```

### LAMMPS 晶胞转换为 ASE 晶胞

通过下述代码，ASE 将 LAMMPS 边缘矢量转化为了晶胞矩阵（3×3）：

```python
def construct_cell(diagdisp, offdiag):
    """Help function to create an ASE-cell with displacement vector from
    the lammps coordination system parameters.

    :param diagdisp: cell dimension convoluted with the displacement vector
    :param offdiag: off-diagonal cell elements
    :returns: cell and cell displacement vector
    :rtype: tuple
    """
    xlo, xhi, ylo, yhi, zlo, zhi = diagdisp
    xy, xz, yz = offdiag

    # create ase-cell from lammps-box
    xhilo = (xhi - xlo) - abs(xy) - abs(xz)
    yhilo = (yhi - ylo) - abs(yz)
    zhilo = zhi - zlo
    celldispx = xlo - min(0, xy) - min(0, xz)
    celldispy = ylo - min(0, yz)
    celldispz = zlo
    cell = np.array([[xhilo, 0, 0], [xy, yhilo, 0], [xz, yz, zhilo]])
    celldisp = np.array([celldispx, celldispy, celldispz])

    return cell, celldisp
```

## 参考

- [LAMMPS—units 命令解析](https://zhuanlan.zhihu.com/p/410687074)
- [LAMMPS 翻译系列-dimension 命令](http://www.52souji.net/lammps-command-dimension.html)
- [LAMMPS 翻译系列-atom-style 命令](http://www.52souji.net/lammps-command-atom-style.html)
- [lammps 不同类型 data 文件格式对比，以及不同类型 data 文件相互转换方法](https://zhuanlan.zhihu.com/p/420847294)
- [LAMMPS 学习总结 1](https://blog.csdn.net/weixin_45896923/article/details/116032495)
- [8.2.3. Triclinic (non-orthogonal) simulation boxes](https://docs.lammps.org/Howto_triclinic.html)
- [任意晶格矢量与 Lammps 盒子定义转换](https://zhuanlan.zhihu.com/p/359736837)
