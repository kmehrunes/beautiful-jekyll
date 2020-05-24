---
layout: post
title: Building the World's Best Network Simulator I
subtitle: Introduction
cover-img: /assets/img/simulation.jpg
tags: [networks, distributed systems, design]
---

## Background
I was going through some of the old work that I did and I stumbled upon my work
with Nokia Bell Labs. Although that was over two years ago, it still remains one
of the weirdest and most interesting projects I have worked on to date. Weird as
in we started talking about running basic mathematical simulation and ended up
having to deal with virtual time dilation.

## The Basic Goal
At that time, the team that I joined was working on multi-path networking. Where
you have multiple channels (ethernet, WiFi, LTE..etc) and instead of sending and
receiving data through one, you can utilize all of them. There
was already [Multi-path TCP (MPTCP)](https://en.wikipedia.org/wiki/Multipath_TCP) and some attempts at achieving that in lower layers (IP for example).

Multi-path in theory sounds great but getting it right is far from an easy task.
So we need to test scenarios, a lot of them actually, against different approaches
to the problem. The goal was to find the best way to do that.

## The Options
In order to test existing solutions, and the ones we will come up with, we have
four options: mathematical models, computer simulations, full deployment, and a
hybrid approach.

### Mathematical Models
Mathematical models provide abstract expressive definitions. They are also
universally testable and do not rely on an implementation or a certain platform.
Those abstract models are essential when a proof of correctness needs to be
provided.

While mathematical models have a lot of benefits they still cannot be used to
test the implementation of a system which users and other systems will interact
with.

### Computer Simulations
Simulations offer the means by which one can evaluate a certain implementation in
one or more scenarios meant to mimic the real use case scenarios. Another major
advantage of using simulations is that they could reproduced under the same
simulation platform, and they have a deterministic output (unless stochastic
proccesses were involved).

Simulations are one step closer to reality than mathematical models, but eventually
reality might not match the simulation.

### Full Deployment
This is where everything is implemented, installed on real devices, connected to
real networks, and we have real traffic. It does not get more real than that, but
it also does not get more expensive and time consuming.

### Hybrid Approach
The final approach is to combine a deployed systems with a virtaul simulated
network. This combines the advantages of simulations and full deployment while
mitigating some of the shortcomings of both.

We ended up going with a hybrid approach.

## Existing Solutions
Before we go into what we did, here are some of the existing solutions which I
considered but ended up not going with for one reason or another. I won't go into
much details but they're here for reference in case you needed one for your own:
1. Network Simulation Cradle (NSC)
2. Direct Code Execution Cradle (DCE Cradle)
3. Virtual Machine Simulation Integration (VMSimInt)
4. Common Open Research Emulator (CORE)

It would have been great to use any of these but unfortunately none really provided
what we were looking for. Some only allow host network stacks, limited only to
Linux or BSD, didn't account for time rate differences, and other problems.

## Conclusion
Since none of the existing solutions really solved our problem, I decided to go
ahead an implement our own simulation framework. Needless to say, writing everything
from scratch is absolute madness, especially when it comes to channel simulation.
So reusing existings bits and pieces and tools was essential, and ns3 was the
saviour.

In the next installment I'll explain the overall design of the system.
