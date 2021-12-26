+++
title = "人工智能数学基础（四）线性代数基础"
date = 2021-11-26
lastmod = 2021-11-28T11:26:48+08:00
draft = true
katex = true
+++

> 学习笔记《人工智能必备数学基础全套课程》线性代数基础


## 行列式 {#行列式}

说起行列式，就要从求解线性方程组开始，考虑求解二元线性方程组：
\\[
  \\{\begin{array}{l}a\_{11} x\_{1}+a\_{12} x\_{2}=b\_{1} \\ a\_{21} x\_{1}+a\_{22} x\_{2}=b\_{2}\end{array}
  \\]

通过消元法可以得到：

$$

\begin{array}{l}\left(a\_{11} a\_{22}-a\_{12} a\_{21}\right) x\_{1}=b\_{1} a\_{22}-a\_{12} b\_{2} \\ \left(a\_{11} a\_{22}-a\_{12} a\_{21}\right) x\_{2}=a\_{11} b\_{2}-b\_{1} a\_{21}\end{array}

$$

进而得到：

\\[
  x\_{1}=\frac{b\_{1} a\_{22}-a\_{12} b\_{2}}{a\_{11} a\_{22}-a\_{12} a\_{21}} \quad x\_{2}=\frac{a\_{11} b\_{2}-b\_{1} a\_{21}}{a\_{11} a\_{22}-a\_{12} a\_{21}}
  \\]

我们发现， \\(x\_1\\) 和 \\(x\_2\\) 的分母都是 \\({a\_{11} a\_{22}-a\_{12} a\_{21}}\\) ，因此当 \\(a\_{11} a\_{22}-a\_{12} a\_{21} \neq 0\\) 时，方程组有唯一解。根据二元线性方程组的规律，可以得到二阶行列式，即：
![](/ox-hugo/pngpaste_clipboard_file_20211126103302.png)

表达式 \\({a\_{11} a\_{22}-a\_{12} a\_{21}}\\) 便是行列式的值，即：
\\[
  D=\left|\begin{array}{ll}a\_{11} & a\_{12} \\ a\_{21} & a\_{22}\end{array}\right|=a\_{11} a\_{22}-a\_{12} a\_{21}
  \\]
其中 \\(a\_{i j}(i=1,2 ; j=1,2)\\) 称为元素。\\(i\\) 代表行标，\\(j\\) 代表列标。

我们可以将二阶行列式推广到三阶行列式，行列式的值即为主对角线元素的乘积减去副对角线的乘积，如下图：

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20211126103810.png" >}}


## 矩阵 {#矩阵}

矩阵经常被用来在计算机中表示各种各样的数据，如表格、图像等，通常我们对数据进行操作同样也是对矩阵的操作。

这里以二维矩阵为例，包括行和列:

\\[
  A=\left(\begin{array}{cccc}a\_{11} & a\_{12} & \cdots & a\_{1 n} \\ a\_{21} & a\_{22} & \cdots & a\_{2 n} \\ \vdots & \vdots & & \vdots \\ a\_{m 1} & a\_{m 2} & \cdots & a\_{m n}\end{array}\right)
  \\]

我们将其中的一行称之为行向量，一列称之为列向量。它和行列式的主要区别在于，行列式其实是一个值 \\(\sum\_{p\_{1} p\_{2} \cdots p\_{n}}(-\mathbf{1})^{t\left(p\_{1} p\_{2} \cdots p\_{n}\right)} a\_{1 p\_{1}} a\_{2 p\_{2}} \cdots a\_{n p\_{n}}\\) ，并且要求行数等于列数，其中一共有 \\(n^2\\) 个元素。而矩阵是数据的表示，行数可以不等于列数，一共有 \\(m \times n\\) 个元素。

一些特殊的矩阵如下：

-   方阵：行和列一样，一般叫做n阶方阵。

\\[
  A=A\_{n \times n}=A\_{n}=\left(\begin{array}{cccc}a\_{11} & a\_{12} & \cdots & a\_{1 n} \\ a\_{21} & a\_{22} & \cdots & a\_{2 n} \\ \cdots & & & \\ a\_{n 1} & a\_{n 2} & \cdots & a\_{n n}\end{array}\right)=\left(a\_{i j}\right)\_{n \times n}
  \\]

-   上三角矩阵：左下角元素全为 0

    $$

\left(\begin{array}{cccc}a<sub>11</sub> & a<sub>12</sub> & &ctdot; & a<sub>1 n</sub> \\\\ 0 & a<sub>22</sub> & &ctdot; & a<sub>2 n</sub> \\\\ \vdots & \vdots & \vdots & \vdots \\\\ 0 & 0 & &ctdot; & a<sub>n n</sub>\end{array}\right)
$$

-   下三角矩阵：右上角元素全为 0

    $$

\left(\begin{array}{cccc}a<sub>11</sub> & 0 & &ctdot; & 0 \\\\ a<sub>21</sub> & a<sub>22</sub> & &ctdot; & 0 \\\\ \vdots & \vdots & \vdots & \vdots \\\\ a<sub>n 1</sub> & a<sub>n 2</sub> & &ctdot; & a<sub>n n</sub>\end{array}\right)
$$

-   对角阵： 除对角线外，元素全为 0

    $$

\left(\begin{array}{cccc}&lambda;<sub>1</sub> & 0 & &ctdot; & 0 \\\\ 0 & &lambda;<sub>2</sub> & &ctdot; & 0 \\\\ \vdots & \vdots & & \vdots \\\\ 0 & 0 & &ctdot; & &lambda;<sub>n</sub>\end{array}\right)
$$

-   单位矩阵：对角阵 + 对角线元素全为 1

    $$

\left(\begin{array}{cccc}1 & 0 & &ctdot; & 0 \\\\ 0 & 1 & &ctdot; & 0 \\\\ \vdots & \vdots & \vdots & \vdots \\\\ 0 & 0 & &ctdot; & 1\end{array}\right)
$$

-   同型矩阵：两个矩阵的行列数一致
-   相等矩阵：同型矩阵 + 对应元素相等

矩阵的一些基本运算：

-   加减法：对应元素相加减，要求两个矩阵是同型矩阵，即行列数一致
-   数乘运算：一个数 \\(\lambda\\) 与矩阵 A 的乘积

    $$

&lambda; A=A &lambda;=\left(\begin{array}{cccc}&lambda; a<sub>11</sub> & &lambda; a<sub>12</sub> & &ctdot; & &lambda; a<sub>1 n</sub> \\\\ &lambda; a<sub>21</sub> & &lambda; a<sub>22</sub> & &ctdot; & &lambda; a<sub>2 n</sub> \\\\ \vdots & \vdots & & \vdots \\\\ &lambda; a<sub>m 1</sub> & &lambda; a<sub>m 2</sub> & &ctdot; & &lambda; a<sub>m n</sub>\end{array}\right)
$$

-   矩阵乘法: 假设两个矩阵 \\(A\_{mn}\\) 和矩阵 \\(B\_{ij}\\) ，矩阵乘法要求对应的行和列必须一致，即如果是 \\(AB\\) 则 \\(n == i\\) , 对应行和对应列相乘，最终得到一个 \\(mj\\) 大小的矩阵。
    矩阵乘法拥有一些性质，如下：
    -   结合律： \\((AB)C = A(BC)\\) ， \\(\lambda (AB) = (\lambda A)B = A(\lambda B)\\)
    -   分配律： \\(A(B+C) = AB + AC\\) ， \\((B + C)A = BA + CA\\)
    -   **不存在交换律** ： \\(AB  \neq BA\\)
-   矩阵转置： 行和列进行交换
    $$

A=\left(\begin{array}{ccc}1 & 2 & 0 \\\\ 3 & -1 & 1\end{array}\right) \quad A<sup>T</sup>=\left(\begin{array}{cc}1 & 3 \\\\ 2 & -1 \\\\ 0 & 1\end{array}\right)
$$

-   矩阵转置的性质
    1.  \\(\left(A^{T}\right)^{T}=A\\)
    2.  \\((A+B)^{T}=A^{T}+B^{T}\\)
    3.  \\((\lambda A)^{T}=\lambda A^{T}\\)
    4.  \\((A B)^{T}=B^{T} A^{T} \Rightarrow\left(A\_{1} A\_{2} \cdots A\_{n}\right)^{T}=A\_{n}^{T} \cdots A\_{2}^{T} A\_{1}^{T}\\)

-   对称矩阵： \\(a\_{ij}=a\_{ji}\\)
    $$

\left(\begin{array}{cc}10 & 1 \\\\ 1 & -1\end{array}\right)\left(\begin{array}{cc}0 & 0 \\\\ 0 & 0\end{array}\right)\left(\begin{array}{ccc}-3 & 2 & -4 \\\\ 2 & 0 & 7 \\\\ -4 & 7 & 5\end{array}\right)
$$

-   逆矩阵： A 为 n 阶方阵，如果存在 n 阶方阵 B，使得： AB=BA=I（单位阵），则 \\(B=A^{-1}\\) 为 A 的逆矩阵
    -   性质
        $$

\begin{array}{l}\left(A^{T}\right)^{-1}=\left(A^{-1}\right)^{T} \\ \left(A^{-1}\right)^{-1}=A \\ (\lambda A)^{-1}=\frac{1}{\lambda} A^{-1} \\ (A B)^{-1}=B^{-1} A^{-1}\end{array}

$$

矩阵的秩：核心就是列向量或者行向量中极大线性无关组的个数，矩阵中最大不相关向量的个数就是秩了。举一个简单的例子，比如家中有很多张照片（N），但是一家只有三口（R），我们就把R当做矩阵的秩。

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20211128111041.png" >}}

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20211128111055.png" >}}

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20211128111141.png" >}}

向量的内积: element-wise 的乘法

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20211128111510.png" >}}

向量长度：n 维向量的 2 范数
![](/ox-hugo/pngpaste_clipboard_file_20211128111605.png)

向量正交：向量 a 和向量 b 的内积为 0

规范正交基
![](/ox-hugo/pngpaste_clipboard_file_20211128111811.png)
