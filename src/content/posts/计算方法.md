---
title: 计算方法
published: 2026-04-01
draft: false
description: '计算方法课程笔记'
tags: ['计算方法', '课程笔记']
---

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
1. 在求积公式$\int_a^b f(x) dx \approx \sum_{k=0}^n A_k f(x_k)$中，若对于任意的不高于$m$次的代数多项式函数（如$1, x, x^2, \ldots, x^m$）精确成立，但是对于$m+1$次的代数多项式函数不准确成立，则称该求积公式具有$m$次代数精度。
### 收敛性与稳定性
1. 在求积公式$I \approx \sum_{k=0}^n A_k f(x_k)$中，若$$\lim_{n \to \infty} \lim_{h \to 0} \sum_{k=0}^n A_k f(x_k) = \int_a^b f(x) dx$$其中，$h = \max_{1 \leq i \leq n} (x_i-x_{i-1})$，则称该公式是收敛的。
2. 对任给的$\epsilon > 0$，若存在$\delta > 0$，只要$|f(x_k) - \overline{f}_k| < \delta (k=1,2,\ldots,n)$，就有$$\left| I_n(f) - I_n(\overline{f}) \right| = \left|\sum_{k=0}^n A_k[f(x_k) - \overline{f}_k] \right| < \epsilon$$则称该公式是稳定的。
3. 在求积公式$I \approx \sum_{k=0}^n A_k f(x_k)$中，若系数$A_k$满足$$\sum_{k=0}^n A_k > 0$$其中，$M$为常数，则称该公式是稳定的。
### 牛顿-柯特斯公式
$$
\int_a^b f(x) dx \approx (b - a) \sum_{k=0}^n C_k^{(n)} f(x_k) \\
C_k^{(n)} = \frac{(-1)^{n-k}}{nk!(n-k)!} \int_0^n \prod_{j=0, j \neq k}^n (t-j) dt
$$
对于Cotes系数$C_k^{(n)}$，
$$
\begin{cases}
\sum_{k=0}^n C_k^{(n)} = 1 \\
C_k^{(n)} = C_{n-k}^{(n)}   
\end{cases}
$$
当$n=1$时，为梯形公式，具有1次代数精度：
$$
\int_a^b f(x) dx \approx \frac{b-a}{2} [f(a) + f(b)] = T \\
R[f] = -\frac{(b-a)^3}{12} f''(\xi), \xi \in (a, b)
$$
当$n=2$时，为辛普森公式，具有3次代数精度：
$$
\int_a^b f(x) dx \approx \frac{b-a}{6} [f(a) + 4f\left(\frac{a+b}{2}\right) + f(b)] = S \\
R[f] = -\frac{(b-a)^5}{2880} f^{(4)}(\xi), \xi \in (a, b)
$$
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

## 线性方程组的求解

