---
layout: default
title: STAT 391
parent: STAT
---

# STAT 391

# Lecture 1 - March 28

- $S$ is sample space
- $x \in S$ is an outcome
- Examples:
    1. Coin toss: $S = \{0, 1\}$
    2. Die roll: $S = \{0, \dots, 6\}$
    3. Stopping time: $S = \{0^{*}1\}$
- Event: $E \subseteq S$
    - $2^{S} \equiv \{E | E \subseteq S\} \equiv P(S)$
        - $2^{S}$ stands for the power set of $S$
    - We use events because measure theory requires it
    - We use events because we (often) don't care about $Prob(x)$ alone
    - They can be used for continuous sample spaces
    - e.g. $S$ is photos, $E$ could be photos of dogs
- Probs
    - $P : 2^{S} \rightarrow [0, 1]$
        - This means that each subset of $S$ is mapped by a probability function to that chances of occurring, between $0$ and $1$ (inclusive).
    - $P(E)$ is the probability of $E$ occurring
- Random variables
    - $f: S \rightarrow \mathbb{R}$
        - $f$ is a function that assigns each $x \in S$ a numerical value
        - This is helpful because it allows us to represent random events as numbers
- Axioms of probability:
    1. $P(E) \geq 0$
        - All events have a probability $0$ or greater
    2. $P(S) = 1$
        - The probabilities of of all outcomes sum to $1$
    3. $[E \cap E' = \phi] \rightarrow [P(E \cup E') = P(E) + P(E')]$
        - For two events which are distinct, the probability of either occurring is equal to the sum of the probabilities of the individual events
- References to boolean algebra:
    - $A \cup B$
        - $A$ or $B$
    - $A \cap B$
        - $A$ and $B$
    - $S$
        - true
    - $\phi$
        - false
- Uniform distribution
    - e.g. coin or fair dice
    - all have the same probability
    - Uniform distribution over area: $P(E) = \frac{Area(E)}{Area(S)}$
        - $Area(z)$ is normalization constant, often $Z$
    - $P(E) \propto Area(E)$
        - The probability of $E$ is proportional to the area of $E$
        - $Z$ must exist for something to be proportional to another function
    - The proportional function to the probability is the *unnormalized* probability
        - e.g. $Area(E)$