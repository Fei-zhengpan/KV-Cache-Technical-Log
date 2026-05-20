# KV-Cache-Technical-Log

Notes on memory bottlenecks, cache compression, token selection, and hardware-aware designs

What is KV cache?
<img width="530" height="398" alt="image" src="https://github.com/user-attachments/assets/7cd6edd6-c3b4-4f16-bcc8-a6de1a0df737" />

In autoregressive decoding, the Key and Value tensor of previous tokens are reused across generation steps.

q1 need k1,v1
q2 need k1,v1,k2,v2
...

so to aviod recomputation, we can cache the previous kv, which is so call KV cache.

