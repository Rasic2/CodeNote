# 标记语言教程

## $\TeX$

LaTeX（$\LaTeX$，音译“拉泰赫”）是一种基于 $\TeX$ 的排版系统，它非常适用于生成高印刷质量的科技和数学类文档。确切的来讲，他并不是一种标记语言，但因为日常工作中经常会和 [Markdown](https://markdown.com.cn/) 一起使用，故而列在此处。

### $\LaTeX$ 常用符号

#### 特殊符号

| Grammar   |   Symbol    |
| --------- | :---------: |
| \alpha    |  $\alpha$   |
| \beta     |   $\beta$   |
| \gamma    |  $\gamma$   |
| \tilde{w} | $\tilde{w}$ |

#### 基本运算符号

| Grammar |  Symbol  |
| ------- | :------: |
| \times  | $\times$ |
| \cdot   | $\cdot$  |

#### 高级运算符号

| Grammar     |    Symbol     |
| ----------- | :-----------: |
| \frac{a}{b} | $\frac{a}{b}$ |

#### 矩阵表示

- 小括号向量

  ```latex
  \begin{pmatrix}a
  \\ b
  \\ c
  \end{pmatrix}
  ```

  表示为：

  $$
  \begin{pmatrix}a
  \\ b
  \\ c
  \end{pmatrix}
  $$

- 中括号矩阵

  ```latex
  \begin{bmatrix}a
  \\ b
  \\ c
  \end{bmatrix}
  ```

  表示为：

  $$
  \begin{bmatrix}a
  \\ b
  \\ c
  \end{bmatrix}
  $$

#### 格式控制

- 控制等号对齐

  ```latex
  \begin{aligned}
  M &= e^{\tilde{w}θ} \\
  &= I + \tilde{w}sinθ+\tilde{w}^2(1-cosθ) \\
  \end{aligned}
  ```

  表示为：

  $$
  \begin{aligned}
  M &= e^{\tilde{w}θ} \\
  &= I + \tilde{w}sinθ+\tilde{w}^2(1-cosθ) \\
  \end{aligned}
  $$

### $\LaTeX$ 常用网站

- [在线 $\TeX$ 公式编辑器](https://www.latexlive.com/home)
