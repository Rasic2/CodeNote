# LAMMPS 教程

[LAMMPS](https://www.lammps.org/) 作为一个经典的分子动力学程序，可以模拟液体中的粒子，固体和气体的系综，也可以采用不同的力场和边界条件来模拟全原子，聚合物，生物，金属，粒状和粗料化体系；其可以计算的体系小至几个粒子，大到上百万甚至是上亿个粒子。

## 输入参数文件

## 坐标文件

针对输入文件中指定的不同原子类型（atom_style），坐标文件的书写有不同的格式要求，下面将对常用的原子类型 `atomic`、`charge` 和 `full` 如何书写坐标文件进行说明。

### atomic 类型

```{image} lammps.png
:width: 500
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
:width: 500
:align: center
```

full 类型除了上述的 6 项之外，还要在 `atom-ID` 后面加一列 `molecule-ID`，以及 `x` `y` `z` 后面加 `nx` `ny` `nz` 三列用于周期性原子的定位（可选）。

### 参考

- [lammps 不同类型 data 文件格式对比，以及不同类型 data 文件相互转换方法](https://zhuanlan.zhihu.com/p/420847294)
