---
layout: default
title: May Articles
parent: Articles
---

# May Articles

---

## [Go smol or go home](https://www.harmdevries.com/post/model-size-vs-compute-overhead/)

*Scaling laws and compressing model size*

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

## [Transformer Math 101](https://blog.eleuther.ai/transformer-math/)

*Basic equations about transformer language models, especially with regards to training*

- Compute Requirements:
    - $C \approx \tau T = 6PD$
    - $C$ is compute required to train the model in total FLOP
    - $C = C_{forward} + C_{backward}$
    - $C_{forward} \approx 2PD$
    - $C_{backward} \approx 4PD$
    - $\tau$ is the aggregate throughput of hardware setup, $\tau = (\text{No. GPUs) \times (\text{Actual FLOPs/GPU)$ (in FLOPs)
    - $T$ is the time spent training the model in seconds
    - $P$ is the number of parameters in the transformer model
    - $D$ is the dataset size in tokens
- Common units of $C$:
    - FLOP-seconds
    - GPU-hours (number of GPUs multiplied by the number of hours)
    - PetaFLOP-days $10^{15} \times 24 \times 3600$ total floating point operations (scaling papers)
- Parameter vs Dataset tradeoffs:
    - Compute optimal language models reccommend a dataset about $frac{1}{20}$ the size of the number of parameters
    - In practice, do not train a model with less than 200B tokens from scratch
        - Find what inference cost is accessible, what compute available, then train the largest model possible
- Compute costs:
    - GPT-NeoX gets 150 TFLOPs/A100 with normal attention, 180 TFLOPs/s/A100 with flash attention (similar to other frameworks)
    - Assume minimum 120 TFLOP/s/A100, if lower than 115 there's probably something wrong
    - Possible to achieve linear or sublinear scaling across parallel dimensions with good interconnect
- Memory:
    - int8: 1 byte per param, fp16 and bf16: 2 bytes per param, fp32: 4 bytes per param
    - Small overhead for model memory usage, typically $\leq 20\%$ and irrelevant
    - Inference also has an often negligible overhead
    - Heuristic for inference memory requirement is: $\text{Total Memory}_{\text{Inference}} \approx 1.2 \times \text{Model Memory}$
- Training memory:
    - Mixed-precision training requires storing an additional two bytes per param ($\text{memory}_{\text{model}}$)
    - AdamW, 12 bytes per param:
        - fp32, momentum, variance each 4 bytes per param
    - 8-bit optimizers, 6 bytes per param:
        - fp32: 4 bytes, momentum 1 byte, variance: 1 byte
    - SGD-like optimizer with momentum, 8 bytes per param:
        - fp32, momentum with 4 bytes per param
    - $\text{memory}_{\text{gradients}}$ typically is the model dtype, so 2 or 4 bytes per param
    - Read the article for more on activation recomputation/checkpointing
    - $\text{Total Memory}_{Training} = \text{Model Memory} + \text{Optimizer Memory} + \text{Activation Memory} + \text{Gradient Memory}$
    - Read the article for more on sharded optimizers and 3D paralleism

---
