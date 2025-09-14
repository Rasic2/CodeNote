# 数学相关笔记

## 旋转矩阵的计算

核心思路：根据旋转前后的两个向量值，先求出旋转角和旋转轴，然后用[罗德里格旋转公式](https://baike.baidu.com/item/%E7%BD%97%E5%BE%B7%E9%87%8C%E6%A0%BC%E6%97%8B%E8%BD%AC%E5%85%AC%E5%BC%8F/18878562)计算相应的旋转矩阵。

### 旋转角计算

旋转前后的向量 $P$、$Q$ 之间的夹角 $θ$ 计算公式为：

$$
θ = arccos(\frac{P \cdot Q}{|P||Q|})
$$

### 旋转轴计算

旋转轴 $c$ 为 $a$ 与 $b$ 向量的叉乘，即

$$
c = a \times b = \begin{pmatrix}a_2b_3 - a_3b_2
\\ a_3b_1 - a_1b_3
\\ a_1b_2 - a_2b_1
\end{pmatrix}
$$

### Rodrigues 旋转公式

旋转矩阵 $M$ 可由旋转轴和旋转角推导得出，具体为：

$$
\begin{aligned}
M &= e^{\tilde{w}θ} \\
  &= I + \tilde{w}sinθ+\tilde{w}^2(1-cosθ) \\
  &= \begin{bmatrix}
      cosθ+w_x^2(1-cosθ) & w_xw_y(1-cosθ)-w_zsinθ & w_ysinθ+w_xw_z(1-cosθ) \\
      w_zsinθ+w_xw_y(1-cosθ) & cosθ+w_y^2(1-cosθ) & w_yw_z(1-cosθ)-w_xsinθ \\
      w_xw_z(1-cosθ)-w_ysinθ & w_xsinθ+w_yw_z(1-cosθ) & cosθ+w_z^2(1-cosθ)
     \end{bmatrix}
\end{aligned}
$$

其中 $\tilde{w} = \begin{bmatrix}
0    & -w_z & w_y  \\
w_z  & 0    & -w_x \\
-w_y & w_x  & 0
\end{bmatrix}$
### 代码实现

### 参考

- [根据旋转前后的向量值求旋转矩阵](https://www.cnblogs.com/xpvincent/archive/2013/02/15/2912836.html#:~:text=%E6%B1%82%E6%97%8B%E8%BD%AC%E7%9F%A9%E9%98%B5,%E6%A0%B9%E6%8D%AE%E6%97%8B%E8%BD%AC%E5%89%8D%E5%90%8E%E7%9A%84%E4%B8%A4%E4%B8%AA%E5%90%91%E9%87%8F%E5%80%BC%EF%BC%8C%E4%BD%BF%E7%94%A8%E4%B8%8A%E9%9D%A2%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%8C%E5%85%88%E6%B1%82%E5%87%BA%E6%97%8B%E8%BD%AC%E8%A7%92%E5%BA%A6%E5%92%8C%E6%97%8B%E8%BD%AC%E8%BD%B4%EF%BC%8C%E7%84%B6%E5%90%8E%E7%94%A8%E7%BD%97%E5%BE%B7%E9%87%8C%E6%A0%BC%E6%97%8B%E8%BD%AC%E5%85%AC%E5%BC%8F%E5%8D%B3%E5%8F%AF%E6%B1%82%E5%87%BA%E5%AF%B9%E5%BA%94%E7%9A%84%E6%97%8B%E8%BD%AC%E7%9F%A9%E9%98%B5%E3%80%82)
