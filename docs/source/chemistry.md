# 计算化学相关

## Material Studio 软件 Perl 接口

### Perl 基础语法

- 遍历目录下的所有文件

  ```perl
  my $dir="C:\\Users\\hui_zhou\\Documents\\Materials Studio Projects\\Default\\Documents\\catalysts";

  opendir (DIR, $dir) or die "can't open the directory!";
  my @dir = readdir DIR;
  ```

- 正则匹配文件后缀

  ```perl
  $file =~ /\.xsd/
  ```

- 变量替换
  ```perl
  $prefix =~ s/\.xsd//;
  ```

### MS 接口相关

- 引用 MS 接口

  ```perl
  use MaterialsScript qw(:all);
  ```

- 打开结构文件并绑定到 doc 变量

  ```perl
  my $doc = $Documents{"heterogenous\\$file"};
  ```

- 取消结构的对称性

  ```perl
  $doc->MakeP1;
  ```

- 设置边框的颜色

  ```perl
  my $lattice=$doc->Lattice3D;
  $lattice->Color=16724923;
  ```

- 计算成键

  ```perl
  $doc->CalculateBonds;
  ```

- 获得所有原子

  ```perl
  my $atoms=$doc->UnitCell->Atoms;
  ```

- 更改原子的显示类型

  ```perl
  $atom->Style = "Ball and stick";
  ```

- 保存结构

  ```perl
  $doc->Save;
  ```

- 将结构输出为 msi 格式

  ```perl
  $doc->Export("$export_dir\\${prefix}.msi");
  ```

- 关闭结构文件

  ```perl
  $doc->Close;
  ```

## Gaussian 软件

`Gaussian` 是做半经验计算和从头计算使用最广泛的量子化学软件，可以研究：分子能量和结构，过渡态的能量和结构，化学键以及反应能量，分子轨道，偶极矩和多极矩，原子电荷和电势，振动频率，红外和拉曼光谱，NMR，极化率和超极化率，热力学性质，反应路径等。

支持的计算方法如下：

| 理论方法                                  |     举例      |
| ----------------------------------------- | :-----------: |
| 半经验（Semi-empirical）                  | AM1，PM3，PM6 |
| 密度泛函（Density functional theory）     |  B3LYP， M06  |
| Hartree-Fock                              |      HF       |
| 微扰（Perturbation theory）               |   MP2， MP4   |
| 耦合簇（Coupled cluster）                 |     CCSD      |
| 组态相互作用（Configuration interaction） |     CISD      |
| 多组态自洽场（MCSCF）                     |    CASSCF     |

### 基组

基组其实就是一套模板化的函数。

进行量子化学计算，最关键的部分就是求解出体系各个能级的波函数。根据分子轨道理论的思想，分子轨道可以用各个原子轨道的线性组合来拟合。因此，我们就可以拿一套原子的波函数（基函数）作为基底，混合它们来求解体系波函数。**描述一个原子所用到的全部基函数就叫做一套基组。**

#### 劈裂价键基组（Split-valence basis set）

利用高斯开展 DFT 计算时，常用`劈裂价键基组`对原子轨道进行描述，得益于其对计算精度和效率的有效平衡。

常见的劈裂价键基组有 3-21G、6-31G、6-311G 等，在这些表示中，“-”之前的数字表示构成内层原子轨道的高斯型函数数目，“-”之后的数字分别表示构成价层轨道的劈裂基函数的高斯型函数数目。

:::{note}
以 6-31G 所代表的基组为例，每个内层电子轨道由 6 个高斯型函数线性组合而成，而每个价层电子轨道则被劈裂成两个基函数，分别由 3 个和 1 个高斯型函数线性组合而成。
:::

#### 极化函数（polarization function）

在劈裂价键基组上添加极化函数的方式：以添加了极化函数的基组 6-31G(d)和 6-31G(d,p)为例，6-31G(d)相当于在劈裂价键基组 6-31G 的基础上为重原子（除氢，氦原子）添加一个 d 极化函数（在表示方法上，6-31G(d)等同于 6-31G\*）；而 6-31G(d,p)则相当于除了在劈裂价键基组 6-31G 的基础上为重原子添加一个 d 极化函数，也为轻原子（氢和氦原子）添加一个 p 极化函数（6-31G(d,p)等同于 6-31G\*\*）。

#### 弥散函数（diffuse function）

添加弥散基组的方式：以添加了极化函数和弥散函数的基组 6-31+G(d)和 6-31++G(d,p)为例，6-31+G(d)相当于在基组 6-31G 的基础上为重原子（除氢，氦原子）添加一个极化函数和一个弥散函数；而 6-31++G(d,p)则相当于除了在基组 6-31G 的基础上为重原子添加一个极化函数和一个弥散函数，也为轻原子（氢和氦原子）添加一个极化函数和一个弥散函数。

#### Gaussian 关键词指明基组

- 混合基组

混合基组就是指不同的原子用不同的基组，写 `gen` 关键词代表从坐标部分后面读入基组定义。

以 CH$_4$ 分子为例，如果我们想让 C 是 6-311G\*，H 是 6-31G\*\*，则输入文件为

```
# m062x/gen

CH4

0 1
C -0.00000000  0.00000000  0.00000000
H -0.00000000  0.00000000  1.09000000
H -0.00000000 -1.02766186 -0.36333333
H -0.88998127  0.51383093 -0.36333333
H  0.88998127  0.51383093 -0.36333333

C 0
6-311G*
****
H 0
6-31G**
****
```

:::{note}
可以在元素符号前写上`-`，表明分子中有这个元素则基组定义生效，如果没有这个元素也不报错，例如可以将上面 `C 0` 改成 `C -F 0`。
:::

- 赝势基组

赝势基组需要和对应的赝势一起使用，使用赝势基组需要用 `genecp` 关键词，代表先从分子坐标后面读取赝势基组定义，再读取赝势定义。

以 Cu(CO)+为例：

```
# B3LYP/genecp

Cu(CO)+

1 1
Cu
C 1 B1
O 2 B2 1 180.

B1 1.94000000
B2 1.11540000

Cu 0
SDD
****
C O 0
6-31G*
****

Cu 0
SDD
```

#### 参考

- [基组的简要介绍及选择建议](https://zhuanlan.zhihu.com/p/363177076)
- [详解 Gaussian 中混合基组、自定义基组和赝势基组的输入](http://sobereva.com/60)
- [Gaussian 教程 | 使用基组和赝势](http://blog.molcalx.com.cn/2017/11/30/gaussian-tutorial-basis-set.html)
- [BSE 基组数据库](https://www.basissetexchange.org/)
- [Stuttgart 系列赝势](http://www.tc.uni-koeln.de/PP/clickpse.en.html)

## VASP 软件

### 技巧

- VASP 的 `NELECT` 参数不能等于 0，可设为 0.0001 替代。
