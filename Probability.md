---
title: Probability
---

# 1. Indice

- [1. Indice](#1-indice)
- [2. Probability](#2-probability)
- [3. Probability](#3-probability)
  - [3.1. Random Experiments](#31-random-experiments)
  - [3.2. Axioms of Probability](#32-axioms-of-probability)
    - [3.2.1. Example](#321-example)
  - [3.3. Uniform Probability Model](#33-uniform-probability-model)
    - [3.3.1. Basic Principle of Counting](#331-basic-principle-of-counting)
      - [3.3.1.1. Example: Extracting Balls](#3311-example-extracting-balls)

# 2. Probability

# 3. Probability

The definition of Probability has been discussed for long.

For us, we will define it as a **relative frequency**. Given $N$ independent
conditions, and $k$ tries of an Event $E$, the probability is defined as:

$$
P(E) = \lim_{N\to+\infty}{k \over N}
$$

Solving an infinite limits is bothersome, so we will use some tricks like
_symmetry_ to simplify many calculations.

## 3.1. Random Experiments

Some examples of random experiments are:

- Throw of 6-face dices

- A horse race

- Integrity test of a Device

When performing an experiment, **we** decide a _desired outcome_
(certain number on top of the dice, horse order at arrival, times the device fails)

Once we decided the outcome, the next step is to understand the **Sample Space Set**.

On our example:

$$
\begin{matrix}
  S_1 = \Set{1,2,3,4,5,6} \\[0.5em]
  S_2 = \Set{\text{permutation of 1..7}}\\[0.5em]
  S_3 = [0, +\infty)
\end{matrix}
$$

After defining the sample space, an event is defined as a **set of possible outcome of the Sample Space**: &emsp;$E \subseteq S$

Some special events are:

- **Null Event**: $\empty$

- **Certain Event**: $S$

Since events are sets, we can use _Set Algebra_ to manipulate them:

- **Union** $(\cup)$: The union of two Events $E \cup F$ are the events
  that appear either on $E$ or $F$ (or both)

- **Intersection** $(\cap)$: The intersection of two Events $E \cap F = EF$
  are the events that appear in both $E$ and $F$

- **Complement** $(E^C)$: The complement of an Event $E$ is the event $F$ that
  produces a _Null Set_ when intersected $E^C \cap E = \empty$

These three algebraic relations are related by _De Morgan's Law_:

$$
(A \cup B)^C = A^C \cap B^C \\
(A \cup B)^C = A^C \cup B^C \\
$$

<div class="grid2">
<div>

Let $E, F, G$ be three arbitrary events on the space $S$, as shown in the picture on the right.

We can describe _set algebra equations_ to describe the following arguments:

1. Only $E$ occurs: &emsp; $E \cap (F \cup G)^C = EF^CG^C$

2. Both $E$ and $G$, but not $F$: &emsp; $E \cap G \cap F^C = EGF^C$

3. At least one of the events: &emsp; $E \cup F \cup G$

4. At least two of the events: &emsp; $(EG \cup EF \cup FG)$

5. All three occurs: &emsp; $EFG$

6. None occur: &emsp; $(E \cup F \cup G)^C$

7. At most one occur: &emsp; $(EG \cup EF \cup GF)^C$

8. Exactly two: &emsp; $(EG \cup EF \cup FG) \cap (EFG)^C$

9. At most three: &emsp; $S$

Notice that the results we found are valid for every possible configuration of the three sets.

</div>
<div>
<figure class="90">
<img class="100" src="./images/probability/set-examples.png">
<figcaption>

Two possible sets configurations.

</figcaption>
</figure>
</div>
</div>

## 3.2. Axioms of Probability

The axioms on which probability works on are:

1. $0 \le P(E) \le 1$

2. $P(S) = 1$

3. $E_i \vert E_iE_j = \empty \wedge i \ne j \Rightarrow P(\bigcup_i E_j) = \sum_i{P(E_i)}$

Starting from these three axioms, we can derive a couple of _useful properties_:

1. $P(E^C) = 1 - P(E)$

2. $P(E_1 \cup E_2) = P(E_1) + P(E_2) - P(E_1E_2)$

### 3.2.1. Example

Let's try to solve this problem:

> 28\% of the American males smoke cigarettes.
> 7\% smoke cigars.
> 5\% smoke both.
> What is the percentage of non-smokers?

Instead of trying to guess the correct answer, we try **modeling the problems in
terms of events**.

Calling $E$ the event "smokes cigarettes" and $F$ the event "smokes cigars", we
can tell that:

$$
\begin{matrix}
  P(E) = 0.28 \\
  P(F) = 0.07 \\
  P(EF) = 0.05
\end{matrix}
$$

The problems asks the quantity $P((E \cup F)^C)$:

$$
\begin{align*}
  P((E \cup F)^C) &= 1 - P(E \cup F) \\
  &= 1 - [P(E) + P(F) - P(EF)] \\
  &= 1 - 0.28 - 0.07 + 0.05 = 0.7
\end{align*}
$$

Thus, we can say with certainty that $70\%$ of Americans are non-smokers.

## 3.3. Uniform Probability Model

In many cases, the sample space of a random experiment has a **finite
cardinality** $(N = \vert S\vert)$.
Furthermore, the sample includes **equally likely outcomes**, like the throw of
fair dice or a fair coin.
From the axioms, we can derive that the probability of the occurance of an
event is exactly $p = \frac{1}{N}$

In these case, we are in a **Uniform Probability Model** (`UPM`).
In this case, the probability of an event $E$:

$$
P(E) = \frac{\vert E\vert}{\vert S\vert}
$$

### 3.3.1. Basic Principle of Counting

Given an experiment $C$ that is composed of two sub-experiments $C_1$ and $C_2$,
having respectively $N_1$ and $N_2$ possible outcomes, the number of possible
outcomes of experiments $C$ is equal to $N_1 \cdot N_2$.

More in general, with $k$ sub-experiments, the number of possible outcomes of
experiments $C$ is equal to:

$$
\prod_{i=1}^k{N_i}
$$

#### 3.3.1.1. Example: Extracting Balls

> Take an _opaque_ (can't see inside) urn with 6 black balls and 5 white balls.
> What is the probability that, extracting two **at random**
> (without replacing them), you get a black and a white one (whatever the order)?

The fact that we are doing the experiment at random, tells us that there are no
reason to think that extracting black balls or white balls should have more probability.

We can model this experiment by numbering the balls:

- **Black Balls**: $b_i \quad \wedge \quad i = 1, ..., 6$

- **White Balls**: $b_i \quad \wedge \quad i = 7, ..., 11$

We can model the sample space as:

$$
S = \Set{(b_i, b_j) \vert 1 \le i,j \le 11, i \ne j}
$$

To compute the cardinality of this set, we observe that the random experiment
is composed of two sub-experiments:

1. Extract a ball from a set of 11

2. Extract a ball from a set of 10

By the principle of counting, we have $11 \cdot 10 = 110$ possible outcomes,
each and every one equally likely, thus we are in a `UPM`.

Now, we need to define the event that interests us:

$$
\begin{align*}
E &= \Set{(b_i, b_j), 1 \le i \le 6, 7 \le j \le 11}
    \cup
    \Set{(b_i, b_j), 7 \le i \le 11, 1 \le j \le 6} \\
  &= E_1 \cup E_2
\end{align*}
$$

Each of the subsets has $6 \cdot 5 = 30$ elements.
Hence, $\vert E \vert = \vert E_1 \vert + \vert E_2 \vert = 60$

The probability of our desired outcome is:

$$
  P(E) = \frac{\vert E \vert}{\vert S \vert} = \frac{60}{110} = \frac{6}{11}
$$
