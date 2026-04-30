---
title: 计算方法
published: 2026-04-01
draft: false
description: '计算方法课程笔记'
tags: ['计算方法', '课程笔记']
---

---

# 计算方法

## 绪论
### 计算方法的研究对象与特点
1. 秦九韶算法
### 误差与误差分析
1. 误差：观测误差、模型误差、方法误差（截断误差）、舍入误差
2. 绝对误差：$e^* = x^* - x$ （近似值 - 精确值），$|e^*| = |x^* - x| \leq \epsilon^*$，$\epsilon^*$ 为绝对误差限/误差限
3. 相对误差：$e_r^* = \frac{x^*-x}{x^*}$，或者 $e_r^* = \frac{x^*-x}{x^*}$，$|e^*_r| \leq \epsilon^*_r$，$\epsilon^*$ 为相对误差限
4. 有效数字：如果近似值 $x^*$ 的绝对误差不超过某一位的半个单位，从该位向左数到 $x^*$ 的第一个非零的数字共有 $n$ 位。则称这 $n$ 位数字全是有效数字，并称 $x^*$ 具有n位有效数字。由精确值经过四舍五入得到的近似值，其绝对误差不超过近似值末位数的半个单位。数字末尾的零不可随意省去。
### 算法稳定性

---

## 插值与拟合
1. 插值法
	1. 拉格朗日插值
		$L(x) = l_{1}(x)y_{1} + l_{2}(x)y_{2} \dots l_{n}(x)y_{n}$ 
	2. 牛顿插值
		列差商表
	3. 埃尔米特插值
		1. 三点三次埃尔米特插值  
		   对 $x_0,x_1,x_2,p(x_i)=f(x_i)(i=0,1,2),p'(x_1)=f'(x_1)$  
		2. 两点三次埃尔米特插值
			对 $x_0,x_1,p(x_i)=f(x_i),p'(x_i)=f'(x_i),(i=0,1)$ 
	4. 分段低次插值
		1. 分段线性插值
		2. 分段埃尔米特插值
		3. 三次样条插值 **（考定义）**：
			对 $x_0,x_1,...,x_n,f(x_i)=y_i$，满足：
			1. 分段三次多项式
			2. 二阶连续导数 **（考过）**
			3. $S(x_i)=f(x_i)=y_i$   
			$$S(x) = \left\{ \begin{array}{cc} S_0(x) & x \in [x_0,x_1] \\ \vdots & \vdots \\ S_n(x) & x \in [x_{n-1},x_n] \end{array} \right.$$
2. 拟合
	1. 最小二乘法
	2. 非线性拟合
		1. $y = a e^{bx}$ ：线性化，$\ln y = \ln a + bx$
		2. $y = a x^b$ ：线性化，$\ln y = \ln a + b \ln x$

---

## 数值积分与数值微分
$$
I = \int_a^b f(x) dx \approx \sum_{k=0}^n A_k f(x_k) + R[f]
$$
### 插值型求积公式
$$
\int_a^b f(x) dx \approx \sum_{k=0}^n A_k f(x_k) \\
A_k = \int_a^b l_k(x) dx 
$$
1. 梯形公式：
$$
I \approx \frac{b-a}{2} [f(a) + f(b)]
$$
2. 矩形公式：
$$
I \approx (b-a) f\left(\frac{a+b}{2}\right)
$$
### 代数精度
1. 在求积公式 $\int_a^b f(x) dx \approx \sum_{k=0}^n A_k f(x_k)$ 中，若对于任意的不高于$m$次的代数多项式函数（如 $1, x, x^2, \ldots, x^m$ ）精确成立，但是对于 $m+1$ 次的代数多项式函数不准确成立，则称该求积公式具有 $m$ 次代数精度。
### 收敛性与稳定性
1. 在求积公式 $I \approx \sum_{k=0}^n A_k f(x_k)$ 中，若$$\lim_{n \to \infty} \lim_{h \to 0} \sum_{k=0}^n A_k f(x_k) = \int_a^b f(x) dx$$其中，$h = \max_{1 \leq i \leq n} (x_i-x_{i-1})$，则称该公式是收敛的。
2. 对任给的 $\epsilon > 0$，若存在 $\delta > 0$，只要 $|f(x_k) - \overline{f}_k| < \delta (k=1,2,\ldots,n)$，就有$$\left| I_n(f) - I_n(\overline{f}) \right| = \left|\sum_{k=0}^n A_k[f(x_k) - \overline{f}_k] \right| < \epsilon$$则称该公式是稳定的。
3. 在求积公式 $I \approx \sum_{k=0}^n A_k f(x_k)$ 中，若系数 $A_k$ 满足$$\sum_{k=0}^n A_k > 0$$其中，$M$ 为常数，则称该公式是稳定的。**（考判断）** 
### 牛顿-柯特斯公式
$$
\int_a^b f(x) dx \approx (b - a) \sum_{k=0}^n C_k^{(n)} f(x_k) \\
C_k^{(n)} = \frac{(-1)^{n-k}}{nk!(n-k)!} \int_0^n \prod_{j=0, j \neq k}^n (t-j) dt
$$
对于 $Cotes$ 系数 $C_k^{(n)}$，
$$
\begin{cases}
\sum_{k=0}^n C_k^{(n)} = 1 \\
C_k^{(n)} = C_{n-k}^{(n)}   
\end{cases}
$$
当 $n=1$ 时，为梯形公式，具有 $1$ 次代数精度：
$$
\int_a^b f(x) dx \approx \frac{b-a}{2} [f(a) + f(b)] = T \\
R[f] = -\frac{(b-a)^3}{12} f''(\xi), \xi \in (a, b)
$$
当 $n=2$ 时，为辛普森公式，具有 $3$ 次代数精度：
$$
\int_a^b f(x) dx \approx \frac{b-a}{6} [f(a) + 4f\left(\frac{a+b}{2}\right) + f(b)] = S \\
R[f] = -\frac{(b-a)^5}{2880} f^{(4)}(\xi), \xi \in (a, b)
$$
$n$ 为偶数时，牛顿-柯特斯公式至少有 $n+1$ 次代数精度。
### 复化（复合）积分公式
1. 复化梯形公式
$$
\int_a^b f(x) dx \approx \frac{h}{2} [f(a) + 2\sum_{k=1}^{n-1} f(x_k) + f(b)] = T_n \\
R[f] = -\frac{(b-a)h^2}{12} f''(\eta), \eta \in (a, b)
$$
2. 复化辛普森公式
$$
\int_a^b f(x) dx \approx \frac{h}{6} [f(a) + 4\sum_{i=0}^{n-1} f(x_{i+\frac{1}{2}}) + 2\sum_{i=1}^{n-1} f(x_{i}) + f(b)] = S_n \\
R[f] = -\frac{b-a}{2880}h^4 f^{(4)}(\eta), \eta \in (a, b)
$$

性质：复合梯形公式与复合辛普森公式都是收敛的，且都是稳定的。

---

## 线性方程组的数值解法
对于 $Ax = b$，
1. 高斯消元法
2. 列主元素消去法
3. 矩阵分解法
	1. 直接三角分解法：$A=LU$
	2. 平方根法（对称正定矩阵）：$A=LL^T$
	3. 改进的平方根法：$A=LDL^T$
4. 误差估计与扰动分析
	1. 常见向量范数：
		1. 1-范数：$\Vert x \Vert _1 = \sum^n_{i=1}|x_i|$ **绝对值之和**
		2. 2-范数：$\Vert x \Vert _2 = \big( \sum^n_{i=1}x^2_i \big) ^{\frac{1}{2}}$ **平方和开根号**
		3. 无穷范数（最大范数）：$\Vert x \Vert _\infty = \underset{1 \leq i \leq n}{\max}|x_i|$ **绝对值最大值**
	2.  常见矩阵范数
		1. F-范数：$\Vert A \Vert _F = \big( \sum^n_{i=1} \sum^n_{j=1} a^2_{ij} \big) ^{\frac{1}{2}}$ **平方和开根号**
		2. 常见算子范数：
			1. 1-范数（列范数）：$\Vert A \Vert _1 = \underset{1 \geq j \leq n}{\max} \sum^n_{i=1} |a_{ij}|$ **列绝对值和最大值**
			2. 2-范数（谱范数）：$\sqrt{\rho(A^T A)}$ **特征值的绝对值的最大值（$A^TA$）开根号**
			3. 无穷范数（行范数）：$\Vert A \Vert _{\infty} = \underset{1 \leq i \leq n}{\max} \sum^n_{j=1} |a_{ij}|$ **行绝对值和最大值**
	3. 矩阵的条件数：设 $A$ 非奇异，则称 $Cond(A) = \Vert A^{-1} \Vert \Vert A \Vert$ 为 $A$ 的条件数
	4. 迭代法
		1. 矩阵分裂迭代法：$A = D-L-U = M-N$ 
			1. Jacobi 迭代：$M=D,N=L+U$ 
			2. Gauss-Seidel 迭代：$M=D-L,N=U$ 
			3. SOR 迭代：$M=\frac{1}{\omega}(D - \omega L),N=\frac{1}{\omega}[(1-\omega)D + \omega U]$  
		2. $x^{(k+1)} = Bx^{(k)} + f$ 收敛 $\Leftrightarrow$ $\rho(B) < 1$ ，特征值的绝对值的最大值（$B$）

例如：对于
$$
\begin{pmatrix}
2 & -1 & 0 \\
-1 & 3 & -1 \\
0 & -1 & 2 \\
\end{pmatrix}
\begin{pmatrix}
x_{1} \\
x_{2} \\
x_{3} \\
\end{pmatrix}
=
\begin{pmatrix}
1 \\
8 \\
-5 \\
\end{pmatrix}
$$
1. Jacobi 迭代
$$
\begin{cases}
x^{(k+1)}_{1} = \frac{1}{2}(1 + x^{(k)}_{2}) \\
x^{(k+1)}_{2} = \frac{1}{3}(8 + x^{(k)}_{1} + x^{(k)}_{3}) \\
x^{(k+1)}_{3} = \frac{1}{2}(-5 + x^{(k)}_{2})
\end{cases}
$$
2. Gauss-Seidel 迭代
$$
\begin{cases}
x^{(k+1)}_{1} = \frac{1}{2}(1 + x^{(k)}_{2}) \\
x^{(k+1)}_{2} = \frac{1}{3}(8 + x^{(k+1)}_{1} + x^{(k)}_{3}) \\
x^{(k+1)}_{3} = \frac{1}{2}(-5 + x^{(k+1)}_{2})
\end{cases}
$$
3. SOR 迭代
$$
\begin{cases}
x^{(k+1)}_{1} = x^{(k)}_{1} + \omega \frac{1}{2}(1 - 2x^{(k)}_{1} + x^{(k)}_{2}) \\
x^{(k+1)}_{2} = x^{(k)}_{2} + \omega \frac{1}{3}(8 + x^{(k+1)}_{1} - 3x^{(k)}_{2} + x^{(k)}_{3}) \\
x^{(k+1)}_{3} = x^{(k)}_{3} + \omega \frac{1}{2}(-5 + x^{(k+1)}_{2} - 2x^{(k)}_{3})
\end{cases}
$$
