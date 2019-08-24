## 笔记
- ### 1.熟悉 Eigen 矩阵运算
本次习题,你需要使用 Eigen 库,编写程序,求解一个线性方程组。为此,你需要先了解一些有关线性方程组数值解法的原理。
设线性方程$\boldsymbol{A} \boldsymbol{x}=\boldsymbol{b}$,在 $\boldsymbol{A} $ 为方阵的前提下,请回答以下问题:
- 1. 在什么条件下,$\boldsymbol{x}$ 有解且唯一?
- 2. 高斯消元法的原理是什么?
- 3. QR 分解的原理是什么?
- 4. Cholesky 分解的原理是什么?
- 5. 编程实现 $\boldsymbol{A} $  为 100 × 100 随机矩阵时,用 QR 和 Cholesky 分解求 $\boldsymbol{x}$的程序。

####  1. 在什么条件下,$\boldsymbol{x}$ 有解且唯一?
n元非齐次线性方程组$A_{m \times n} x=b$
- $R(A) \neq R(B)  \Leftrightarrow A x=b$无解
- $R(A)=R(B)=n \Leftrightarrow A x=b$有唯一解
- $ R(A)=R(B)< n \Leftrightarrow A x=b $有无穷多解

#### 2. 高斯消元法的原理是什么?
[参考](https://en.wikipedia.org/wiki/Gaussian_elimination)
数学上，高斯消元法（英语：Gaussian Elimination），是线性代数中的一个算法，可用来为线性方程组求解，求出矩阵的秩，以及求出可逆方阵的逆矩阵。当用于一个矩阵时，高斯消元法会产生出一个行梯阵式。
$$
\left[\begin{array}{rrrr}{1} & {3} & {1} & {9} \\ {1} & {1} & {-1} & {1} \\ {3} & {11} & {5} & {35}\end{array}\right] \rightarrow\left[\begin{array}{rrr|r}{1} & {3} & {1} & {9} \\ {0} & {-2} & {-2} & {-8} \\ {0} & {2} & {2} & {8}\end{array}\right] \rightarrow\left[\begin{array}{rrr|r}{1} & {3} & {1} & {9} \\ {0} & {-2} & {-2} & {-8} \\ {0} & {0} & {0} & {0}\end{array}\right] \rightarrow\left[\begin{array}{rrr|r}{1} & {0} & {-2} & {-3} \\ {0} & {1} & {1} & {4} \\ {0} & {0} & {0} & {0}\end{array}\right]
$$

#### 3. QR 分解的原理是什么?
[参考](https://zh.wikipedia.org/wiki/QR%E5%88%86%E8%A7%A3)
QR分解法是三种将矩阵分解的方式之一。这种方式，把矩阵分解成一个正交矩阵与一个上三角矩阵的积。QR分解经常用来解线性最小二乘法问题。QR分解也是特定特征值算法即QR算法的基础。
