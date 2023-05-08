---
layout: default
title: May Articles
parent: Articles
---

# May Articles

---

## [Go smol or go home](https://www.harmdevries.com/post/model-size-vs-compute-overhead/)

- Notation:
    - $N$: number of model parameters
    - $D$: number of training tokens
    - $C$: amount of compute
    - $L(N, D)$: loss as a function of parameters and tokens
- While there are "compute optimal" models, it is typically best to use extra relative compute and train smaller models
    - Allows for better post-training usage
    - Many practitioners already train on extra tokens
- Past a certain "critical model size", extra compute leads to diminishing returns on loss
    - We can use scaling laws to find critical model size
    - ~30% of Chinchilla optimal
- LLaMa-7B was trained on 1T tokens and has not reached critical model size
- Chinchilla scaling laws: $L(N, D) = E + \frac{A}{E^{\alpha}} + \frac{B}{D^{\beta}}$
    - $E = 1.69$, $A = 406.4$, $B = 410.7$, $\alpha = 0.32$, $\beta = 0.28$
- Typical compute budget is: $C = 6ND$
- Compute optimal number of parameters and tokens:
    - $N_{opt}(C) = G(\frac{C}{6})^{\frac{\beta}{\alpha + \beta}}$
    - $D_{opt}(C) = G^{-1}(\frac{C}{6})^{\frac{\beta}{\alpha + \beta}}$
    - $G = (\frac{\alpha A}{\beta B})^{\frac{1}{\alpha + \beta}}$
- If we want to scale $N$ down by $k_N$ and scale $D$ up by $k_D$ while retaining the same loss as Chinchilla optimal:
    - The new compute cost: $C_{new} = 6(k_N N_{opt})(K_D D_{opt})$
    - Percent compute overhead: $C_{overhead} = \frac{C_{new} - C}{C} \cdot 100$
    - Data scaling factor is independent of (original) compute budget C
- We can reduce model size to 75% with only a ~3% compute overhead
    - Half model size, overhead ~20%, quarter model size, overhead 188%
- If inference is never run, Chinchilla is optimal, else varying degrees of taking smaller models
- Critical model size: how many parameters necessary to practically train a model to a certain loss
    - About 30% Chinchilla optimal with 100% overhead
    - Reasonable to select a model size 40-60% smaller than optimal with 10-42% compute overhead
- LLaMa: $k_N = 0.57$ with 12% compute overhead
    - 6.9B parameters, 1T tokens, 4.14e22 FLOP
- SantaCoder: $k_N = .46$ with 24% compute overhead
    - 1.1B parameters, 236B tokens, 1.56e21 FLOP
- Limitations:
    - Chinchilla laws might not be accurate for extrapolation
    - Smaller models might achieve the same perplexity, but less emergent ability
    - Training smaller models for longer might have parallelization issues

---
