---
title: Systems and Simulations
---

# 1. Indice

- [1. Indice](#1-indice)
- [2. Systems](#2-systems)
  - [2.1. Performance Evaluations](#21-performance-evaluations)
- [3. Simulations](#3-simulations)
  - [3.1. Types of Simulations Model](#31-types-of-simulations-model)
  - [3.2. How to build a Good Simulator](#32-how-to-build-a-good-simulator)

# 2. Systems

A **system** is the subject of our performance evaluations, and is defined as:

> A _collection of entities_ that act and interact together towards the
> accomplishment of some logical goal

The collection of entities that describes the system depends on the objective
of the evaluations. The same system might be analyzed based on different
entities in different cases.

The description of a system is given by its **State**, that might change over time.
Depending on how the state variable of system change, we define the system as:

- **Continuous System**: if the change are continuous over time
- **Discrete System**: if the change happen instantaneously at a given time

## 2.1. Performance Evaluations

When we do a performance evaluation of a system, we **monitor** its state over time.
This way we can understand how it _evolves over time_, trace its _statistics_,
or computing some _limit values_.

This can be done essentially in three ways:

1. **Measurements**: we can simply let the _real system_ (or a prototype) work and
   measure the output. The problem with this technique is that sometimes we do
   not have access to the system we want to analyze nor he are able to create
   prototype of it.
2. **Analytical Models**: We can set up an _analytical model of the
   system_ (i.e. _equations_) that binds the desired quantities. The problem with
   this technique is that it might be difficult to find these relations, and even
   more difficult to actually solve them. We can always simplify the sets of equations,
   abstracting and leaving only "important stuff", but identifying the things that
   are "important" is not always possible prior of starting.
3. **Simulations Models**: We can set up a _software replica_ of the system, giving
   it some input and recording the output it produces. This way we can replicate
   only the parts of the system that are meaningful to our analysis. The problem
   with this technique is often the prices of the resources needed to run the simulations.

Putting all together, ideally, we can build a simulation model of the system
and validate it by comparing its result with a analytical model in a
simple basic scenario. After a complete simulation study, we can start building
a prototype of the real system.

# 3. Simulations

Simulations means _**mimicking a system on a computer**_.

In order to do so we need a specific software, called _simulation software_ or _simulator_,
which goals are:

- Understanding the functioning of the system
- Predicting the behavior of the system
- Support decisions on the system
- Training on a system
- Entertaining

In order to classify a simulation model as a _good simulation model_, its
output must be sufficiently close to reality.

Therefore, our aim is to tweak the system to make the approximated output
similar to the real output (or at least statistically equivalent).

Provided we are able to obtain a good model, almost everything can be simulated.
The presence of a model can allow us to evaluate alternatives, for example
what happens if the link between the endpoints of a communication is an
Ethernet cable or a fiber optic cable, or the "experimental settings", allowing
to change everything the system interacts with, which is very hard in the real world.
Other things we are able to do are the evaluation the scalability assessment, measuring
what happens when we multiply the input (e.g., a number of users) by a factor $k$,
or the manipulation of timescales, simulating the evolution of the universe in
a few seconds/minutes, or observe how the instructions of a computer program
are fetched and executed by a processor, which will typically take times in
the order of nanoseconds.

One of the downsides of using simulated models is the presence of _stochastic quantities_:
assuming you are simulating the arrival of packets at a router, the length of
these packets and their inter-arrival times will often be generated randomly,
maybe following some probability distributions.
This means that the node throughput is itself a random variable. This means that
each measure obtained from the simulator will be a _sample of an output random
variable_, and should be treated as such.

Another downside of building simulator is the building itself. Writing down a
simulator can be very difficult.
Furthermore, simulation analysis (not writing down the code, but using that
code) requires a lot of time, and (mostly) a lot of experience.

It is difficult to realize that a simulator is nothing more than a replica of
a model of a system, and not of the system itself. This means that we are
simplifying things. Taking the wrong modeling steps in the modeling part, can
disrupt the entire work.

## 3.1. Types of Simulations Model

Simulations means different things to different people:

1. **Static vs. Dynamic**: in _static models_ the output is time-invariant and
   can be obtained as $y = f(x)$, where $f$ is the model, and $x$ the input. In
   _Dynamic models_ we are interested in observing how the model changes over
   time, adding the difficulty of replicating something that is supposed to
   happen as time progresses.
2. **Continuous vs. Discrete**: This depends on what we want to evaluate and how
   we want to do it.
3. **Deterministic vs. Stochastic**: in _deterministic models_ we do not have
   random components present. On the contrary, in _stochastic models_ random
   components will produce a set of random output from a given input.

We will work mainly with _**Discrete Event Simulators**_ (`DES`), which are
_dynamic_, _discrete_ and _stochastic_.

## 3.2. How to build a Good Simulator

The first step of building a good simulator is **identifying the state variables**.
Among these variables, since we are working with _discrete-event simulations_,
we must have a special variable that will be responsible to simulate the
flowing of time, called the **simulation clock**.

The whole simulation will then be running in **simulated time**, defined as:

> The time as measured **within** the simulation.

Simulated time (_st_) **is not** real time (_rt_), otherwise we would be
talking about _emulation_.
In order to simulate one second of _st_ we might need either more or less than
one second of _rt_.

In general, there is **no constant relationship** between the _rt-flow_ and the
_st-flow_.

To make time evolve in a simulator, we use the _simulation clock_.
In order to advance the _simulation clock_ we can follow different approaches:

1. **Next-event time advance (event-based)**: Simulated time advances only
   when an _event_ is processed. Supposing the event processes an event at time
   $t_1$. Processing the event itself implies _generating another event_ at time
   $t_2$. The system clock advances only based on the event sequence,
   therefore it is clear that _rt-duration_ of one second of _st-time_ varies
   depending on how many events are in that second.
2. **Fixed-increment time advance (quantum-based)**: the system clock advances by
   a _fixed quantum_ called $D$. At time $kD$, we process all the events that occurred
   between $(k-1)D$ and $kD$. This implies:
   - _Possible time waste_: if the granularity of events $\gg D$, we waste a
     lot of _real time_ just to advance the clock when nothing happens.
   - _Possible simultaneous processing_: if the granularity is $\ll D$, different
     events happening in the same _simulated time-slot_, will be processed at
     the same time. This allows us to use _discrete variable_ as temporal quantum.

Quantum-based simulations make sense if the system is **intrinsically time-discrete**.
Moreover, fixed-increments can be recreated by using _fake periodical events_
in \_event-based simulations.

For these reasons we will assume that our `DES` will use the **Next-Time
Advance Mechanism**.
