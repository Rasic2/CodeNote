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

#zoom 1.6 adiam 1.5 body type 1.0 0

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
- [LAMMPS 翻译系列-atom-style 命令](http://www.52souji.net/lammps-command-atom-style.html)
- [lammps 不同类型 data 文件格式对比，以及不同类型 data 文件相互转换方法](https://zhuanlan.zhihu.com/p/420847294)
