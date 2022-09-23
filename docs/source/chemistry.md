# 计算化学相关

## VASP 软件

### 技巧

- VASP 的 `NELECT` 参数不能等于 0，可设为 0.0001 替代。

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
