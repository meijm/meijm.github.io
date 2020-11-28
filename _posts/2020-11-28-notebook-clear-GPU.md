---
layout: post
title:  "clear gpu memory"
date:   2020-11-28
categories: update
---
If you run tensorflow related programs on public gpu server with jupyter-notebook. After your network is trained, tensor data may still occupy a large amount of gpu memory(you can use 'nvidia-smi' command to check usage of gpu). Under these circumstances, GPUs are not computing at all, but other students can't employ these Idle GPU.
![out](/source/outofme.png)
Here is a easiest solution:
```python
from numba import cuda
cuda.select_device(0)
cuda.close()
```