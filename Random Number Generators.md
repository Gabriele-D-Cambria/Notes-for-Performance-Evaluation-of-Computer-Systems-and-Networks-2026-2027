---
title: Random Number Generators
---

# 1. Index

<!--toc:start-->

- [1. Index](#1-index)
- [2. Random Number Generators](#2-random-number-generators)
  - [2.1. Midsquare Method](#21-midsquare-method)
- [3. Pseudo-Random Number Generators](#3-pseudo-random-number-generators)
  - [3.1. Linear Congruential Generators (LCGs)](#31-linear-congruential-generators-lcgs)
  - [3.2. Independent Streams of Random Numbers](#32-independent-streams-of-random-numbers)
  - [3.3. Properties and Problems of LCGs](#33-properties-and-problems-of-lcgs)
  - [3.4. C++ functions](#34-c-functions)
  - [3.5. Other RNGs](#35-other-rngs)
- [4. Testing RNGs](#4-testing-rngs)
  - [4.1. QQplot](#41-qqplot)
  - [4.2. Chi-Square Test](#42-chi-square-test)
  - [4.3. Kolmogorov-Smirnov Test](#43-kolmogorov-smirnov-test)
  - [4.4. Independence Test](#44-independence-test)
- [5. Random Variate Generation](#5-random-variate-generation)
  - [5.1. Inverse Transform Method](#51-inverse-transform-method)
  <!--toc:end-->

# 2. Random Number Generators

In the example of the _queue_ and the _server_ the input was **artificially
generated** by a _random variable_ $X\sim Exp(\lambda)$.

To do so, we need to use a **Random Number Generator**.

They are not only used for generating random inputs, but also for _randomized
algorithms_, _Monte Carlo simulations_, _cryptography_, etc.

To simulate a **Random Variable** we use a two-step approach:

1. **Random Number Generation**: we generate a "random" variable uniformly distributed
   within $[0, 1)$.
2. **Random Variate Generation**: we transform the above variable in order to meet
   the desired distribution properties.

## 2.1. Midsquare Method

Its a method to generate random number sequentially, with each new number
being determined by one of its predecessors according to a _fixed mathematical
formula_.

The steps are the following:

1. We start from a **4 digit** positive integer number $Z_0$
2. We square it, obtaining a number with 8 digits (if necessary we add leading zeros)
3. We take the _middle 4 digits_ of this number obtaining a new number $Z_1$
4. We place a decimal point to the left of the number, obtaining a number $U_1$
   in $[0, 1)$

We can start again with $Z_1$ to obtain $Z_2$ and so on, getting a **stream of numbers**
that appears to be random.

Even though it looks promising, it has two main problems:

1. It **quickly degenerates** to $0$
1. It is not **actually random**, since it is _deterministic_ and _periodic_.

As for the second point, there are two truth:

1. **We _cannot_ generate random numbers** on a PC
2. **We _do not need_ random numbers**: its enough to have sequence of numbers
   that are both _distributed uniformly_ (according to the RV we are simulating)
   and _highly uncorrelated_, so that the IID assumption is reasonable.

# 3. Pseudo-Random Number Generators

To build a good-enough RNG we need something that:

1. Generates numbers in $[0, 1)$ that are both **uniformly distributed** and **uncorrelated**
2. Is fast and parsimonious with memory resources.
3. Yields **reproducible results**. This is fundamental for _debugging_,
   _validation_ and _repeatability_ of experiments. We can achieve this by
   initializing the RNG with a _seed value_, a number that determines the
   _starting point_ of the sequence.
4. Provides for "separate streams" of random numbers, so that we can run
   different simulations in parallel without having to worry about
   _correlation_ between the streams.

In the most general case what we obtain is a **Pseudo-Random Number
Generator** (`PRNG`), which is a _deterministic function_ that takes a _seed
value_ and produces a sequence of numbers that appears to be random:

$$
x_n = f(x_{n-1}, x_{n-2}, ..., x_0)
$$

Given the function $f$ and the $n$ **initial seeds** $x_{n-1}, x_{n-2}, ..., x_0$
we **always** obtain the same $x_n$.

Quite often $n = 1$, so we end up needing only **one initial seed** $x_0$.

Every pseudo-random number generator has a **period** $P$, which is the number of
numbers generated before the sequence starts to repeat itself.

At the very least, since it outputs results on $n$-bits, it will produce at
most $2^n$ different numbers, and therefore the period is at most $2^n$.

From the moment we loop back to one value we had already generated, the values
**will not be independent anymore**, hence they should not be used.

Then, to have a "good" function $f$, the period $P$ should be at least large.

Most programming languages provide built-in PRNGs. Before using them we should either
**read the code** (or at least the documentation) or **test them** using the conformity
tests that RNGs are expected to pass. The fact that they are in a library does
not mean
they are suitable for any scientific purpose.

## 3.1. Linear Congruential Generators (LCGs)

If we want generate number distributed in $[0, m)$, we can use a **Linear
Congruential Generator** (`LCG`), which is:

$$
  x_n = (a \cdot x_{n-1} + c) \operatorname*{mod} m
$$

If we want numbers distributed in $[0, 1)$ we can simply divide the result by $m$.

The random numbers appearing to be IID $U(0,1)$ are $\frac{x_0}{m},
\frac{x_1}{m}, ..., \frac{x_n}{m}$.

Taking $m = 16$, the full period will be at most $16$ numbers, but we can see quite
easily that starting with $x_0 = 0$, with $a = 5$ and $b = 3$ we will obtain
the following sequence:

$$
\begin{align*}
  x_0 &= 0 \\
  x_1 &= (5 \cdot 0 + 3) \operatorname*{mod} 16 = 3 \\
  x_2 &= (5 \cdot 3 + 3) \operatorname*{mod} 16 = 2 \\
  x_3 &= (5 \cdot 2 + 3) \operatorname*{mod} 16 = 13 \\
  x_4 &= (5 \cdot 13 + 3) \operatorname*{mod} 16 = 0 \\
\end{align*}
$$

If $P = m$ the LGC is said to have **full period**.

We can classify differently LGC based on values of $b$:

- **Mixed LCG** $(b > 0)$: if $m$ is a _large power of $2$_, $a = 4c + 1$ and
  $b$ is _odd_ we are sure to achieve _full period_.
- **Multiplicative LCG** $(b = 0)$: if $m$ is a _power of $2$_, the period
  cannot be larger
  than $\frac{m}{4}$. We can achieve _max period_ if $a = 8c \pm 3$ and $x_0$
  is _odd_.

The selection of the initial seed $x_0$ is important, since it can affect the
quality of the random numbers generated. In general, it is recommended to avoid:

- Even Numbers
- Zero
- Unpredictable numbers (like the current time, process id, `/dev/urandom`, etc.)

If our RNG comes with too many conditions, we should consider using another one,
since it is likely that it is not a good one.

## 3.2. Independent Streams of Random Numbers

Within a simulation, we might generally need _several streams of random numbers_.

For example, in a network simulation we might need one stream for the _packets arrival_
and another for the _user transmissions_.

The golden rule is to **never use the same RNG for more than one thing at the same
time**, as even sub-sampling might loose the RNG properties we need.

Then, we may use replicas of the same RNG, each initialized with a different seed,
preferably _far apart_ in the sequence.

This is preferable for two reasons:

- **Preserving Statistical Coherence**
- **Avoiding hidden side effects**: if two sources use the same stream, even
  debugging might be difficult, since the two sources will be _correlated_ and
  _dependent_. Changing a parameter in one will inevitably affect the other, and
  it will be difficult to understand why.

For example, if we need $n$ independent streams with less than $k$ random
number per stream, we must use a RNG with period $P \ge nk$. Once we found our
RNG, we instantiate $n$ replicas, each with a different seed:

$$
  s_i = x_{(i-1)\cdot k} \quad 1 \le i \le n
$$

## 3.3. Properties and Problems of LCGs

For both _multiplicative_ and _mixed_ LCGs, every $x_i$ can be **computed
deterministically**:

$$
  x_i = \left(a^ix_0 + \frac{b\left(a^i - 1\right)}{a-1}\right)
  \operatorname*{mod} m
$$

Moreover, $x_i$ can only take _rational values_ in the support
$S = \Set{x = \frac{i}{m} \vert 0 \le 1 \le P}$.

If we analyze tuples of subsequent numbers from the same LCG, e.g., by
plotting vector $[X(i), X(i+1), X(i+2)]$ in a 3D space, we observe that points
will tend to align on a **finite number of hyperplanes**.

## 3.4. C++ functions

`C` and `C++` provides two functions in `stdlib.h`

```cpp
// Generates a number between 0 and RAND_MAX
int rand()

// Sets the RNG
void srand(unsigned seed)
```

The RNG algorithm used by the above functions is **compiler-specific**, meaning
that the same code will produce different streams if compiled on two machines,
even with the same seed.

There is no guarantee that the stream of numbers produced by these generators
will pass **statistical test**.

Bottom line is **_never use these functions in our simulation_**.

## 3.5. Other RNGs

Other possible RNGs are:

- Quadratic Congruential Generators: $x_n = (ax_{n-1}^2 + bx_{n-1} + c)
\operatorname*{mod} m$
- Multiplicative Recursive Generators: $x_n = (a_1x_{n-1} + a_2x_{n-2} + ... +
a_kx_{n-k}) \operatorname*{mod} m$
- Tausworthe Generators: operate on bits in very large periods. It is often
  related to cryptographic methods.
- **Mersenne-Twister Generator**: it's the one provided by `OmNet++`, spacing
  huge periods
  $(2^{19937} - 1)$. One downside is that it is _memory-hungry_, since it
  needs to store a large state vector, and _slow_. Still, it satisfies our needs.

# 4. Testing RNGs

If we are proposing an RNG algorithm (which is unlikely, but still possible) or
we are _testing_ and RNG we found somewhere, we should test that it:

1. Produces a stream of uniform numbers in the range $[0, 1)$
1. Produces a stream of uncorrelated numbers

There are several techniques to do so, we will explore the most common ones.

## 4.1. QQplot

To check first one is, we can use a **QQplot** (Quantile-Quantile plot), which is
a graphical technique to compare two probability distributions by plotting their
_quantiles_ against each other.

A _quantile_ is a value below which a certain percentage of data falls. For example,
if a value $x^\ast$ has a probability of $0.25$, we say that the _25th
quantile_ is $x^\ast$.

By plotting this graph, in which the points $p : (q_{th}, q_{emp})$ are plotted,
we can see if the two distributions are similar (the points will be close to
the diagonal) or not (the points will be far from the diagonal). Thus, we can
select for one of the quantiles the one for $U(0,1)$.

## 4.2. Chi-Square Test

There are also numerical tests we can perform, such as the **Chi-Square Test**,
which is a statistical test to determine whether there is a significant difference
between the expected and observed frequencies.

We operate at a level $1 - \alpha = 0.99$.

It works by drawing a large sample $n$ and dividing it into $k$ buckets of equal
width (we must have $n/k > 10$. Each bucket $i$ contains $o_i$ elements, and
will have an expected frequency of $e_i = \frac{n}{k}$.

After finishing the sampling, we compute the Chi-Square statistic:

$$
  D = \sum_{i=1}^{k} \frac{(o_i - e_i)^2}{e_i}
$$

If $D > \chi^2_{\alpha, k-1}$ we **reject** the $U(0,1)$ hypothesis.

This test works with **large samples** $(\sim O(1000))$, otherwise the statistic
is not reliable.

## 4.3. Kolmogorov-Smirnov Test

This test amounts to finding the maximum vertical distance between a
theoretical distribution (in this case, a uniform) and the ECDF of your data
(in this case, the O.S.s of your RNG).

We draw a (not too large) sample $n$ and compute _ordered stats_ $X(i)$.

We then compute two numbers:

$$
\begin{align*}
  D^- = \sqrt{n} \cdot \max_{1 \le i \le n} \left(\frac{i}{n} - X(i)\right) \\
  D^+ = \sqrt{n} \cdot \max_{1 \le i \le n} \left(X(i) - \frac{i-1}{n}\right)
\end{align*}
$$

The two values $D^-$ and $D^+$ are in fact the **maximum distances** (times a coefficient).

If these distances are small enough, then the assumption of matching cannot be rejected.
The test should be done against a particular distribution, called the _K.S. distribution_,
which we can find on tables.

This test depends on the number of samples and the required confidence, but
can be used with pretty much any distribution. It works better with small
samples (when the chi-square test would instead be less significant).

## 4.4. Independence Test

To see if values are independent there are several tests:

- **Visual inspections**: we can either use _lag plots_, in which each point
  is plotted as $p_i = (x_i, x_{i+k})$ for $k = 1, 2, ..., n$. On these plots we
  can see if any trend is present.
- **Numerically**: we can plot a _correlogram_ or _auto-correlation function_.

In general many other tests are available, some more complex than others, but
they all have the same goal: to check if the RNG is good enough for our purposes.

In general, the thing we have to understand is to **_never use an untested RNG_**.

# 5. Random Variate Generation

Given a reliable stream of uniform (pseudo-)random numbers, we can compute
values for arbitrary random distributions.

Unfortunately, there is **not** a general approach that we can use with any distribution,
but there are still some common methods:

- Inverse Transform Method
- Convolution Method

## 5.1. Inverse Transform Method

Suppose we need a sample $x$ from a RV with a _given CDF_ $F(x)$ (contiguous and
increasing).

Denoting its inverse function $F^{-1}$, the method follows two steps:

1. Generate a uniform random number $u \sim U(0, 1)$
2. Return $x = F^{-1}(u)$

For example, supposing we want to sample from an exponentially distributed RV with
mean $1 \over \lambda$:

- PDF: &emsp; $f(x) = \lambda e^{-\lambda x}$
- CDF: &emsp; $F(x) = 1 - e^{-\lambda x}$

The inverse function of $F(x)$ is calculable, allowing us to use the inverse transform
method:

$$
  x = -\frac{1}{\lambda} \cdot \ln(1-u) \quad\wedge\quad u \sim U(0, 1)
$$

This process becomes even more intuitive with _discrete_ RVs, as we just to store
the values in an array, and use the CDF to determine which on which step we fall,
and consequently which value to return.
