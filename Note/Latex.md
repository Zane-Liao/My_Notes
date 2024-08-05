以下是一些常用的 LaTeX 公式，涵盖了基本的数学符号和表达式。

### 基本数学符号
```latex
% 加法和减法
a + b - c

% 乘法和除法
a \times b \div c

% 分数
\frac{a}{b}

% 根号
\sqrt{a}
\sqrt[n]{a} % n次根

% 幂
a^b
a_b % 下标
```

### 三角函数
```latex
\sin x
\cos x
\tan x
\cot x
\sec x
\csc x
```

### 对数和指数
```latex
e^x
\ln x
\log x
\log_{a} x % 底数为a的对数
```

### 微积分
```latex
% 导数
\frac{d}{dx} f(x)
f'(x)
\frac{\partial}{\partial x} f(x) % 偏导数

% 积分
\int f(x)\,dx
\int_{a}^{b} f(x)\,dx % 定积分

% 二重积分
\iint f(x, y)\,dx\,dy

% 三重积分
\iiint f(x, y, z)\,dx\,dy\,dz
```

### 极限
```latex
\lim_{x \to \infty} f(x)
\lim_{x \to 0} \frac{\sin x}{x}
```

### 矩阵
```latex
\begin{matrix}
  a & b \\
  c & d
\end{matrix}

\begin{pmatrix}
  a & b \\
  c & d
\end{pmatrix}

\begin{bmatrix}
  a & b \\
  c & d
\end{bmatrix}

\begin{vmatrix}
  a & b \\
  c & d
\end{vmatrix}

\begin{Vmatrix}
  a & b \\
  c & d
\end{Vmatrix}
```

### 方程组
```latex
\begin{cases}
  ax + by = c \\
  dx + ey = f
\end{cases}
```

### 组合数学
```latex
% 排列
P(n, k) = \frac{n!}{(n-k)!}

% 组合
C(n, k) = \binom{n}{k} = \frac{n!}{k!(n-k)!}
```

### 希腊字母
```latex
\alpha, \beta, \gamma, \delta, \epsilon, \zeta, \eta, \theta, \iota, \kappa, \lambda, \mu, \nu, \xi, \omicron, \pi, \rho, \sigma, \tau, \upsilon, \phi, \chi, \psi, \omega

\Gamma, \Delta, \Theta, \Lambda, \Xi, \Pi, \Sigma, \Upsilon, \Phi, \Psi, \Omega
```

### 其他常用符号
```latex
% 无穷大
\infty

% 部分
\partial

% 集合
\cup, \cap
\subset, \subseteq
\in, \notin

% 向量
\vec{v}
\overrightarrow{AB}

% 箭头
\to, \leftarrow, \Rightarrow, \Leftarrow

% 等号
=
\neq
\approx
\equiv
```

你可以将这些公式嵌入到 LaTeX 文档或其他支持 LaTeX 数学语法的环境中（例如 Markdown 中的数学模式 `$...$` 或 `$$...$$`，Jupyter Notebook，Overleaf 等）来显示数学表达式。

- 概率公式格式
在概率论中，有许多常用的公式和符号来表示各种概率概念和计算。以下是一些常见的概率论公式和符号：

### 基本概率公式
```latex
% 概率
P(A) % 事件 A 发生的概率

% 条件概率
P(A \mid B) = \frac{P(A \cap B)}{P(B)} % 在事件 B 已经发生的情况下事件 A 发生的概率

% 全概率公式
P(A) = \sum_{i} P(A \mid B_i) P(B_i) % 事件 A 的概率等于其在所有分割事件 B_i 上的条件概率之和

% 贝叶斯公式
P(A \mid B) = \frac{P(B \mid A) P(A)}{P(B)} % 贝叶斯公式，用于更新概率
```

### 随机变量和分布
```latex
% 随机变量
X, Y, Z % 通常用大写字母表示随机变量

% 离散随机变量的概率质量函数 (PMF)
P(X = x) % 随机变量 X 取值为 x 的概率

% 连续随机变量的概率密度函数 (PDF)
f_X(x) % 随机变量 X 在 x 处的概率密度

% 分布函数 (CDF)
F_X(x) = P(X \leq x) % 随机变量 X 小于或等于 x 的概率

% 期望值
E(X) = \sum_{i} x_i P(X = x_i) % 离散情况
E(X) = \int_{-\infty}^{\infty} x f_X(x) \, dx % 连续情况

% 方差
\mathrm{Var}(X) = E[(X - E(X))^2]

% 标准差
\sigma_X = \sqrt{\mathrm{Var}(X)}
```

### 常见分布
```latex
% 二项分布
X \sim \mathrm{Binomial}(n, p)
P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}

% 泊松分布
X \sim \mathrm{Poisson}(\lambda)
P(X = k) = \frac{\lambda^k e^{-\lambda}}{k!}

% 正态分布
X \sim \mathcal{N}(\mu, \sigma^2)
f_X(x) = \frac{1}{\sigma \sqrt{2 \pi}} e^{-\frac{(x - \mu)^2}{2 \sigma^2}}

% 指数分布
X \sim \mathrm{Exponential}(\lambda)
f_X(x) = \lambda e^{-\lambda x} \quad \text{for } x \geq 0

% 均匀分布
X \sim \mathrm{Uniform}(a, b)
f_X(x) = \frac{1}{b - a} \quad \text{for } a \leq x \leq b
```

### 常见定理
```latex
% 大数定律
\overline{X}_n = \frac{1}{n} \sum_{i=1}^n X_i \to \mu \quad \text{as } n \to \infty

% 中心极限定理
\frac{\overline{X}_n - \mu}{\sigma / \sqrt{n}} \xrightarrow{d} \mathcal{N}(0, 1) \quad \text{as } n \to \infty

% 切比雪夫不等式
P(|X - \mu| \geq k\sigma) \leq \frac{1}{k^2}
```

这些公式和符号是概率论中的基础工具，广泛应用于各种概率问题和统计分析中。你可以在 LaTeX 文档中使用这些公式来清晰地表示和计算概率相关的内容。