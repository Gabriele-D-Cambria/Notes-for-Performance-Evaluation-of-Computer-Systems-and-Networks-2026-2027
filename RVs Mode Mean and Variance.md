---
title: RVs Mode Mean and Variance
---

# 1. Index

- [1. Index](#1-index)
- [2. RVs Mode Mean and Variance](#2-rvs-mode-mean-and-variance)
  - [2.1. Mode](#21-mode)
  - [2.2. Mean Values](#22-mean-values)
    - [2.2.1. Indicator Variable](#221-indicator-variable)
      - [2.2.1.1. Example: Mean and Indicator Variable](#2211-example-mean-and-indicator-variable)
    - [2.2.2. Functions of Random Variables](#222-functions-of-random-variables)
    - [2.2.3. Example: Mean of a Function of a Random Variable](#223-example-mean-of-a-function-of-a-random-variable)
    - [2.2.4. Linear Transformations of Random Variables](#224-linear-transformations-of-random-variables)
    - [2.2.5. Expectation of the sum of RVs](#225-expectation-of-the-sum-of-rvs)
      - [2.2.5.1. Exercise: Secretary and Letters](#2251-exercise-secretary-and-letters)
      - [2.2.5.2. Exercise: Coupons](#2252-exercise-coupons)
    - [2.2.6. Mean of Independent RVs](#226-mean-of-independent-rvs)
  - [2.3. Variance](#23-variance)
    - [2.3.1. Indicator RV for Variance](#231-indicator-rv-for-variance)
    - [2.3.2. Variance of Linear Transformations](#232-variance-of-linear-transformations)
  - [2.4. Covariance](#24-covariance)
    - [2.4.1. Covariance and Correlation](#241-covariance-and-correlation)

# 2. RVs Mode Mean and Variance

## 2.1. Mode

The **mode** of a RV $X$ is the value $x$ that maximizes the PMF (or PDF) of $X$:

In the previous example, the _mode_ and the _mean_ coincide, but it is only casual.

In general they are different. We can see this for example with _even_ PMF (or
_even_ PDF), where the mean is $x = 0$, and the mode is whatever value
maximizes one of the branches the PMF (or PDF) of $X$.

## 2.2. Mean Values

The **mean value** (or _expectation_, _expected value_) of a RV $X$ denoted with
$E[X]$ and defined as:

$$
E[X] = \begin{cases}
  \sum_{x}{x\cdot p(x)} & \text{if $X$ is discrete} \\
  \int_{-\infty}^{+\infty}{x\cdot f(x)\;dx} & \text{if $X$ is continuous}
\end{cases}
$$

In the discrete case, it is equivalent to the _weighted sum_ of the values $X$
can take on, where the weights are the probabilities of each value.
The expression is akin to the **center of mass**.

In general, it is not true that $E[x]$ is in the _support_ of $X$.

The mean value of a RV is also called **first-order moment** of the variable.
The second order is called **mean square value**.

In general the $n$-th order moment of a RV $X$ is defined as $E[X^n]$.

One last thing to say is that the mean value is **dimensionally coherent**
with the values of RV. (if $X$ is kg, then $E[X]$ is kg as well).

### 2.2.1. Indicator Variable

An interesting and useful discrete RV is the **indicator variable** $I_A$ of an
event $A$, which is defined as:

$$
I_A = \begin{cases}
  1 & \text{if $A$ occurs} \\
  0 & \text{otherwise}
\end{cases}
$$

Therefore:

$$
\begin{CD}
\begin{aligned}
  p(1) &= P(A) \\
  p(0) &= 1 - P(A)
\end{aligned}
@>>>
{
  E[I_A] = 1\cdot P(A) + 0\cdot (1 - P(A)) = P(A)
}
\end{CD}
$$

#### 2.2.1.1. Example: Mean and Indicator Variable

> Compute the value of the RV having the following PDF:
>
> $$
>   f(x) = \begin{cases}
>     \frac{3}{8} \cdot (4x - 2x^2) & 0\le x\le 2 \\
>     0 & \text{otherwise}
>   \end{cases}
> $$

The graph we draw for a previous exercise tells us that the mean value should
be $1$, the computation confirms this:

$$
\begin{align*}
  E[X] &= \int_{-\infty}^{+\infty}{x\cdot f(x)\;dx} \\
  &= \int_{0}^{2}{x\cdot \frac{3}{8} \cdot (4x - 2x^2)\;dx} \\
  &= \frac{3}{8}\int_{0}^{2}{(4x^2 - 2x^3)\;dx} \\
  &= \frac{3}{8}\left[\frac{4}{3}x^3 - \frac{1}{2}x^4\right]_{0}^{2} \\
  &= \frac{3}{8}\left(\frac{32}{3} - 8\right) = 1
\end{align*}
$$

### 2.2.2. Functions of Random Variables

Suppose we have a discrete random variable $X$, and assume a random variable
$Y$ defined as $Y = g(X)$.

To make things easier to visualize, let's suppose $g$ is **injective**.

The mean value expression will then be:

$$
  E[Y] = \sum_{i}{y_i\cdot p_Y(y_i)} = \sum_{j}{g(x_j)\cdot p_X(x_j)} = E[g(X)]
$$

Removing the hypothesis of $g$ being injective, we obtain that the probability
of $p_Y(y_i)$ is given by the **sum of the probabilities of all the values
$x_j$ that are mapped to $y_i$**:

$$
E[Y] = \sum_{i}{y_i\cdot p_Y(y_i)} =
  \sum_{j:g(x_j) = y_i}{p_X(x_j)} =
\sum_{i}{g(x_j)\cdot \sum_{j}{p_X(x_j)}}
= E[g(X)]
$$

The same holds for **continuous RVs**, where the mean value is given by:

$$
E[g(X)] = \int_{-\infty}^{+\infty}{g(x)\cdot f_X(x)\;dx} = E[Y]
$$

### 2.2.3. Example: Mean of a Function of a Random Variable

> Consider a discrete RV with:
>
> $$
> \begin{align*}
>   p(0) &= 0.2 \\
>   p(1) &= 0.5 \\
>   p(2) &= 0.3
> \end{align*}
> $$
>
> Compute $E[X^2]$.

The function $g(X) = X^2$ is injective, so we can compute the mean value as:

$$
\begin{align*}
  E[X^2] &= \sum_{i}{x_i^2\cdot p_X(x_i)} \\
  &= 0^2\cdot 0.2 + 1^2\cdot 0.5 + 2^2\cdot 0.3 \\
  &= 0 + 0.5 + 1.2 = 1.7
\end{align*}
$$

### 2.2.4. Linear Transformations of Random Variables

Given that computing the mean value of a function of a RV is true for **whatever
function**, we can easily see that for **linear Transformations** $g(X) = aX + b$:

$$
\begin{align*}
  E[aX + b] &= \sum_{i}{(ax_i + b)\cdot p_X(x_i)} \\
  &= a\sum_{i}{x_i\cdot p_X(x_i)} + b\sum_{i}{p_X(x_i)} \\
  &= aE[X] + b
\end{align*}
$$

This proves that if we take a RV:

- **Scaling** its values scales the mean value _by the same factor_
- **Adding an offset** to its values _adds the same offset_ to the mean value

### 2.2.5. Expectation of the sum of RVs

The property we described above, can be extended to the sum of **any number of random
variables**, provided that we know their **joint** PMF/PDF.

In general:

$$
E[g(X, Y)] = \begin{cases}
  \sum{\sum{g(x, y) \cdot p(x,y)}} & \text{if $X$ and $Y$ are discrete} \\
  \iint{g(x, y) \cdot f(x,y)\;dxdy} & \text{if $X$ and $Y$ are continuous}
\end{cases}
$$

In the particular case of $g(X, Y) = X + Y$, we have:

$$
\begin{align*}
  E[X + Y] &= \sum{\sum{(x + y) \cdot p(x,y)}} \\
  &= \sum_x{\sum_y{x \cdot p(x,y)}} + \sum_x{\sum_y{y \cdot p(x,y)}} \\
  &= \sum_x{x\cdot\left[\sum_y{p(x,y)}\right]} +
\sum_y{y\cdot\left[\sum_x{p(x,y)}\right]} \\
  &= \sum_x{x\cdot p_X(x)} + \sum_y{y\cdot p_Y(y)} \\
  &= E[X] + E[Y]
\end{align*}
$$

This can be generalized, having the following property:

> The mean value of the sum of $n$ random variables is the **sum** of the mean
> values of the single variable

This is true **whether the RVs are independent or not**.

#### 2.2.5.1. Exercise: Secretary and Letters

> A secretary prints $N$ letters, that have to be inserted in $N$ envelopes.
> A gust of wind blows them on the floor in random order, so she does not know anymore
> which letter goes into which envelope.
>
> Suppose she works at the city hall, the only possible course of action for her
> is to insert the letters randomly into envelopes.
>
> What is the mean value of the number of letters paired with their envelopes?

Define event $A_i$ as _"the $i$-th letter goes into the $i$-th envelope"_.
Since things are done at random, the probability of this event is $P(A_i) = \frac{1}{N}$.

Defining $X_i$ as the _indicator variable_ of the event $A_i$, we have that
$E[X_i] = P(A_i) = \frac{1}{N}$.

Since we need to count how many letters are in the correct envelope, we need
to sum up all the indicator RVs, meaning we need to evaluate the mean value of
the RV $Y = \sum_{i=1}^N{X_i}$:

$$
E\left[\sum_{i=1}^N{X_i}\right] = \sum_{i=1}^N{E[X_i]} =
  \sum_{i=1}^N{\frac{1}{N}} = 1
$$

Meaning that, **on average** only one letter will find the right envelope, no matter
how many letters are printed.

#### 2.2.5.2. Exercise: Coupons

> Assume that coupons can be of $n$ different types with identical probability.
> We pick up $k$ coupons at random. What is the mean value of RV $X$, defined as
> _"the number of different types of coupons picked up"_?

We need once again to _count_ the types of coupons, meaning it will be useful
to have indicator variables.

Defining:

$$
X_i = \begin{cases}
  1 & \text{at least one $i$-th type of coupon is picked up} \\
  0 & \text{otherwise}
\end{cases}
$$

The mean value of different coupons types will be defined as $Y =
\sum_{i=1}^n{X_i}$, and we can compute the mean value of $X$ as:

$$
E[Y] = E\left[\sum_{i=1}^n{X_i}\right] = \sum_{i=1}^n{E[X_i]} = n \cdot E[X_i]
$$

Where the mean value $E[X_i]$:

$$
\begin{align*}
  E[X_i]  &= 1 \cdot P\Set{X_i = 1} + 0 \cdot P\Set{X_i = 0} \\
          &= P\Set{X_i = 1} \\
          &= 1 - P\Set{X_i = 0} \\
          &= 1 - \left(1-\frac{1}{n}\right)^k \\
          &= 1 - \left(\frac{n-1}{n}\right)^k
\end{align*}
$$

The overall mean value of different types of coupons picked up is then:

$$
  E[Y] = n\left[1 - \left(\frac{n-1}{n}\right)^k\right]
$$

### 2.2.6. Mean of Independent RVs

If two RVs $X$ and $Y$ are **independent**, we can compute the mean value of
their product as:

$$
  E[XY] = E[X] \cdot E[Y]
$$

This is easy to prove.

For the discrete case, we have:

$$
\begin{align*}
  E[XY] &= \sum_{x}\sum_{y}{x\cdot y\cdot p(x,y)} \\
  &= \sum_{x}\sum_{y}{x\cdot y\cdot p_X(x)p_Y(y)} \\
  &= \sum_{x}{x\cdot p_X(x)}\sum_{y}{y\cdot p_Y(y)} \\
  &= E[X] \cdot E[Y]
\end{align*}
$$

While for the continuous case:

$$
\begin{align*}
  E[XY] &= \iint{x\cdot y\cdot f(x,y)\;dxdy} \\
  &= \iint{x\cdot y\cdot f_X(x)f_Y(y)\;dxdy} \\
  &= \int{x\cdot f_X(x)\;dx}\int{y\cdot f_Y(y)\;dy} \\
  &= E[X] \cdot E[Y]
\end{align*}
$$

## 2.3. Variance

While **mean values** are a great way to _summarize economically_ as much
information as possible on a given distribution, when alone is often insufficient.

In fact, we might be interested in determining **how dispersed** the values of
a RV are around their mean value.

The measure of **how much a RV is dispersed around its mean value** is called **_Variance_**:

$$
  \operatorname*{Var}(X) = E\left[(X - E[X])^2\right]
$$

There are two reason for the square to be there. The fist one is that we need
a way to cancel out signs, otherwise defects and surpluses would compensate,
as its obvious that $E[X - \mu] = E[X] - \mu = 0$.

In order to dispense with the sign we need either a **modulus** or a **even power**.

The second one is preferable since its both analytically more tractable and
_dimensionally coherent_ with the values of the RV.

We can compute the variance as:

$$
\begin{align*}
  \operatorname*{Var}(X) &= E\left[(X - \mu)^2\right] \\
  &= E\left[X^2 - 2\mu X + \mu^2\right] \\
  &= E[X^2] - 2\mu E[X] + \mu^2 \\
  &= E[X^2] - 2\mu^2 + \mu^2 \\
  &= E[X^2] - \mu^2 \\
  &= E[X^2] - E[X]^2
\end{align*}
$$

Note that variance (unlike the mean value) **is not dimensionally coherent**
with the values of the RV, since it squares is (time $\to$ time², kg $\to$
kg², etc.). For this reason, we often use the **standard deviation** of a RV
$X$, defined as:

$$
\operatorname*{StDev}(X) = \sqrt{\operatorname*{Var}(X)}
$$

### 2.3.1. Indicator RV for Variance

Remembering that the indicator variable $I_A$ of an event $A$ is defined as:

$$
I_A = \begin{cases}
  1 & \text{if $A$ occurs} \\
  0 & \text{otherwise}
\end{cases}
$$

We can easily compute its variance:

$$
\begin{align*}
  \operatorname*{Var}(I_A) &= E\left[(I_A - E[I_A])^2\right] \\
    &= E[I_A^2] - E[I_A]^2 \\
    &= E[I_A] - E[I_A]^2 \\
    &= E[I_A]( 1 - E[I_A])
    &= P(A)(1 - P(A))
\end{align*}
$$

This tells us that the **variance is maximum** when event $A$ has a probability
of $0.5$, and it is **zero** when the event is either _certain_ or _impossible_.

### 2.3.2. Variance of Linear Transformations

We have seen that for the mean value $E[aX + b] = aE[X] + b$, because the expectation
is a **linear operation**.

Since variance is a _second-order moment_, it is not linear, but we can still compute
how it behaves under linear transformations:

$$
\begin{align*}
  \operatorname*{Var}(aX + b) &= E\left[(aX + b - E[aX + b])^2\right] \\
  &= E\left[(aX + b - aE[X] - b))^2\right] \\
  &= E\left[(aX - aE[X])^2\right] \\
  &= E\left[a^2(X - E[X])^2\right] \\
  &= a^2E\left[(X - E[X])^2\right] \\
  &= a^2\operatorname*{Var}(X)
\end{align*}
$$

This tells us two things:

- **Offsets do not matter**
- **Rescaling does matter**: rescaling by a constant factor $a$ means:
  - **Compressing** the distribution if $a < 1$
  - **Spreading** the distribution if $a > 1$

In both cases, the multiplicative factor for the variance is $a^2$, meaning
that the _spread_ of the distribution is **squared** when rescaling.

## 2.4. Covariance

When we were analyzing the mean value, we also noticed that the mean of the
sum of RVs is the sum of the mean values of the single RVs. For the variance
this is not true.

A simple counterexample is the following:

$$
\operatorname*{Var}(X+X) = \operatorname*{Var}(2X)
= 4 \cdot \operatorname*{Var}(X) \ne 2 \cdot \operatorname*{Var}(X)
$$

Therefore, we need to figure out a way the formula for the variance of the sum
of RVs:

$$
\begin{align*}
  \operatorname*{Var}(X+Y) &= E\left[(X + Y - E[X + Y])^2\right] \\
  &= E\left[(X + Y - \mu_X - \mu_Y)^2\right] \\
  &= E\left[(X - \mu_X + Y - \mu_Y)^2\right] \\
  &= E\left[(X - \mu_X)^2 + (Y - \mu_Y)^2 + 2(X - \mu_X)(Y - \mu_Y)\right] \\
  &= E\left[(X - \mu_X)^2\right] + E\left[(Y - \mu_Y)^2\right] +
    2E\left[(X - \mu_X)(Y - \mu_Y)\right] \\
  &= \operatorname*{Var}(X) + \operatorname*{Var}(Y) +
    2E\left[(X - \mu_X)(Y - \mu_Y)\right]
\end{align*}
$$

The last term is called **covariance** of $X$ and $Y$, and it is denoted as:

$$
  \operatorname*{Cov}(X, Y) = E\left[(X - \mu_X)(Y - \mu_Y)\right]
$$

Developing the covariance, we can also write it as:

$$
\begin{align*}
  \operatorname*{Cov}(X, Y) &= E\left[(X - \mu_X)(Y - \mu_Y)\right] \\
  &= E[XY - X\mu_Y - Y\mu_X + \mu_X\mu_Y] \\
  &= E[XY] - \mu_YE[X] - \mu_XE[Y] + \mu_X\mu_Y \\
  &= E[XY] - \mu_Y\mu_X - \mu_X\mu_Y + \mu_X\mu_Y \\
  &= E[XY] - \mu_X\mu_Y
\end{align*}
$$

Therefore the covariance is the **expectation of the product** _minus_ the
**product of the expectations**.

Some properties of the covariance are:

- **Commutative**: $\operatorname*{Cov}(X, Y) = \operatorname*{Cov}(Y, X)$
- $\operatorname*{Cov}(X, X) = \operatorname*{Var}(X)$
- $\Cov(X, Y) = 0$ if $X$ and $Y$ are independent.

From the last property, we can see that **if $X$ and $Y$ are independent _the
variance of their sum is the sum of the variances_**.

For the case of the _sum of $n$ variables_, we have:

$$
\begin{align*}
  \operatorname*{Var}\left(\sum_{i}{X_i}\right) &=
    \sum_{i}{\operatorname*{Var}(X_i)} +
\sum_{i}^n{\sum_{j\ne i}^n{\operatorname*{Cov}(X_i, X_j)}} \\
  &= \sum_{i}^n{\sum_{j}{\operatorname*{Cov}(X_i, X_j)}}
\end{align*}
$$

### 2.4.1. Covariance and Correlation

We saw that the covariance of two variables can be either **positive,**
**negative** or **null**. Its sign has an important physical explanation.

To show this, let's define two events $A$ and $B$, and the related indicator
RVs $I_A$ and $I_B$:

$$
\begin{matrix}
I_A = \begin{cases}
  1 & \text{if $A$ occurs} \\
  0 & \text{otherwise}
\end{cases} &
\qquad &
I_B = \begin{cases}
  1 & \text{if $B$ occurs} \\
  0 & \text{otherwise}
\end{cases}
\end{matrix}
$$

The product of the two indicator RVs $I_A \cdot I_B$ will be null _if and only if_
the event $AB$ occurs.

Computing the covariance:

$$
\begin{align*}
  \operatorname*{Cov}(I_A, I_B) &= E[I_AI_B] - E[I_A]E[I_B] \\
  &= P(AB) - P(A)P(B)
\end{align*}
$$

From this we can discriminate three cases.

The first one is when $\operatorname*{Cov}(I_A, I_B) > 0$, meaning $P(AB) > P(A)P(B)$.
Hence:

$$
  P(A\vert B) = \frac{P(AB)}{P(B)} > P(A)
$$

Conversely, if $\operatorname*{Cov}(I_A, I_B) < 0$, meaning $P(AB) <
P(A)P(B)$, we get:

$$
P(A\vert B) = \frac{P(AB)}{P(B)} < P(A)
$$

The same occurs for $P(B\vert A)$, since both covariance and intersection are
symmetric, meaning that:

- **Positive Covariance** indicates that $A$ is _more likely_ to occur if $B$ occurs.
- **Negative Covariance** indicates that $A$ is _less likely_ to occur if $B$ occurs.

This respects the result we found before that if two events are independent,
their covariance is null.

In general, if we take 2 RVs $X$ and $Y$, a **positive covariance** means that _large
values of $X$ come together with large values of $Y$_, while a **negative
covariance** means that _large values of $X$ come together with small values
of $Y$_.

We can _normalize_ this measurement defining the **Correlation**:

$$
\operatorname*{Corr}(X, Y) = \frac{\operatorname*{Cov}(X, Y)}
  {\sqrt{\operatorname*{Var}(X)\operatorname*{Var}(Y)}} \in [-1, 1]
$$

When the correlation is positive we have a _high calls for high_, when it's
negative we have a _high calls for low_.

The absolute magnitude of the correlation reveals _how much one RV can be predicted
from the other_.

For example, if the correlation is equal to $1$, it means that one is a
linear scaling of the other $(X = aY + b)$.

In general, the correlation is a **measure of linear dependence** between two RVs,
and its invariant under linear scaling:

$$
\operatorname*{Corr}(aX + b, cY + d) = \operatorname*{Corr}(X, Y) \quad a,c > 0
$$
