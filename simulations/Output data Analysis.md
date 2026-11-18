---
title: Output data Analysis
---

# 1. Index

<!--toc:start-->

- [1. Index](#1-index)
- [2. Output data Analysis](#2-output-data-analysis)
  - [2.1. Steady State Analysis](#21-steady-state-analysis)
    - [2.1.1. OMNeT++ Simulation Configuration](#211-omnet-simulation-configuration)
  - [2.2. Representativeness of the simulation results](#22-representativeness-of-the-simulation-results)
  <!--toc:end-->

# 2. Output data Analysis

Most people think that a simulation project involves modeling the system
and programming a computer software. An that running a single simulation is enough
to obtain credible results.

This is obviously **false**.

An output measure from a simulation can be:

- A **Random Variable**
- A **Stochastic Process**

Randomness at the output follows the randomness at the input.
To produce credible results, the solution is to repeat the simulation $N$ times
with different initial seeds.

This will produce $N$ independent results, which can be used to estimate the
characteristics of the output measure.

## 2.1. Steady State Analysis

For our purpose, we are interested in what a system does **in the long run** under
**normal operating conditions**.

Thus, we are interested in the **_steady-state behavior_** of the system (i.e.,
the behavior once it ended the _transient warm-up period_).

This poses a new problem: **estimating the length of the warm-up period**.

> [!NOTE]
> Real-life systems rarely reach a steady state, since the conditions change
> over time even while the system is running, while in models, for which happens
> often, the conditions tend to stay the same for the whole simulation.  
> However, for our purpose this does not matter, since we work with models.

The common approach to estimate the length of the warm-up period is to
**cut the first 10%**.

However, this approach is not sound, since the length of the period depends on the
system we are analyzing. For instance, we might need to cut 15% or 20% of the data,
or even more.

The correct approach is to:

1. Make $n$ **independent replications** of the same scenario using **different
   RNG seeds**
1. Collect a (relatively large) sample of the quantities of interest
1. Make _statistical inferences_ on those values

For example, for _make statistical inferences_ we can:

1. Divide the outputs into $m$ different intervals
1. For each replica, **compute the mean** across the intervals $Y_{i,j} \to Z_i
= E[Y_{i,j}]$
1. For each interval, **compute the mean** across the replicas $Z_{i} \to E[Z_{i}]$
1. Plot $E[Z_i]$ for $i = 1, ..., m$. To smooth the curve we an use a _sliding window
   average mechanism_
1. Identify the $i^\ast$ for which $E[Z_i]$ converges to a constant value.

At the same time, we also have the problem to choose **how long
the simulation should run**.

Unfortunately, the only always correct answer is: _long enough to collect
a statistically meaningful number of events_.

Of course, this is a simplistic answer, but it highlights the fact that the
length of the simulation depends yet again on the system we are modeling.

For example, suppose we simulate a link with a given error model, and we want
to estimate the packet loss probability, knowing, from some a-priori knowledge,
that we ought to expect it to be `\sim O(10^{-4})`.

This knowledge, tells us that we need to simulate the transmission of **_more_
than $10^4$ packets** in order to obtain statistically meaningful results.
Knowing the number $f$ of packets per second what we can transmit, we can compute
a first estimate of the simulation time $T$ as $T = (10^4 / f )* C$, where
$C$ is a constant factor that we can use to widen the confidence interval of
our estimate (it is not rare to set it to $100$ or more)

### 2.1.1. OMNeT++ Simulation Configuration

In the `INI` file of our `OMNeT++` simulation, we can set the `warmup_period`
parameter in order for the average metrics to be computed only
after the warm-up period has passed.

```ini
warmup_period = 100s
```

In our `C++` logic, we cannot use anymore the `simTime()` function when
we want to make `emit()`, since we also need to discard the warm-up period.

This means that a throughput metric emit, for instance, goes from:

```cpp
// Keeping the number of packats sent
pkgs++;

// ...

// finish function
void finish() {
  emit(tput, pkts / simTime().dbl());
}
```

To this:

```cpp
// Keeping the number of packats sent AFTER THE WARM-UP PERIOD
if (simTime() >= getWarmupPeriod()) {
  pkgs++;
}

// ...

// finish function
void finish() {
  emit(tput, pkts / (simTime().dbl() - getWarmupPeriod().dbl()));
}
```

## 2.2. Representativeness of the simulation results

Since the output measure from a simulation run can be a _random variable_ or a
_stochastic process_, **_never ever attempt to make inferences by the trajectory
of one output_**, since a single trajectory may not represent the behaviour of
the system.

Furthermore, the points in a trajectory **are _not independent_**. For example,
the delay of packet $k$ is strongly positively correlated with the delay of
packet $k-1$.

Even removing the initial bias, the dynamics of the simulated system still
depend on the **initial conditions**.

To obtain statistically sound outputs, we need to **obtain _IID observations_**
of the phenomenon we are investigating.

What we can do is perform something similar to what we did for the warm-up period,
called **independent replications**:

1. Run $N$ replicas of the same simulation scenatio with _independent conditions_
   (i.e., independent RNG seeds)
1. Discard the $N$ initial transients.
1. Calculate the average of the output for each replica
1. Since the replicas are independent, we proceed to compute the mean and variance
   of the averages, along with a _confidence interval_.

Another approach is the **Batch Mean**:

1. Perform **one** long simulation
1. Slice the output into $N$ interval
1. Assume each interval to be independent to the others (which is seldom true)
1. Discard the initial transient

Then, like the other approach, we compute the mean, the variance and the
confidence interval.
