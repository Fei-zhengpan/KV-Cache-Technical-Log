# KV-Cache-Technical-Log

Notes on memory bottlenecks, cache compression, token selection, and hardware-aware designs

# What is KV cache?

<img width="530" height="398" alt="image" src="https://github.com/user-attachments/assets/7cd6edd6-c3b4-4f16-bcc8-a6de1a0df737" />

In autoregressive decoding, the Key and Value tensor of previous tokens are reused across generation steps.

q1 need k1,v1

q2 need k1,v1,k2,v2

...

so to aviod recomputation, we can cache the previous kv, which is so call KV cache.

# The memory footprint of KV cache

KV cache trades memory for faster decoding, but shifts the bottleneck to memory: storing and reading historical KV dominates long-context decoding. Large memory footprint also limits the maximum context length.

KV cache size M_kv can be calculated as:

𝑀_𝑘𝑣≈2∙𝐿∙𝐵∙𝑆∙𝐻_𝑘𝑣∙𝐷_ℎ𝑒𝑎𝑑∙𝑏𝑦𝑡𝑒𝑠 𝑝𝑒𝑟 𝑒𝑙𝑒𝑚𝑒𝑛𝑡

𝐿: number of layers

𝐵: batch size

𝑆 : context length

𝐻_𝑘𝑣  ​: number of KV heads

𝐷_ℎ𝑒𝑎𝑑  : head dimension

𝑏𝑦𝑡𝑒𝑠 : bytes per element

Use llama-7B, FP16 as a example, 1-million (2^20) token context need 512GB memory, which is much larger than a single GPU (H100, 80GB). KV cache is one of the biggest challenge in modern LLM.

# Optimizations to reduce KV cache











