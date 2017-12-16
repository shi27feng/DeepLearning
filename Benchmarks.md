This document will contain some benchmarks of my setup under different Tensorflow setups (non CPU optimized, CPU optimized, OpenCL).

# matmul_bench.py #
The [matmul_bench.py](https://github.com/AlphasCodes/DeepLearning/blob/master/matmul_bench.py) file contains code to measure Tops. I found the code in the internet.

- Tensorflow non CPU optimized version (all 4 CPU threads were used)
```
(tensorflow-vanilla) steven@box:~/Coding$ python3 matmul_bench.py 
/home/steven/virtualenvs/tensorflow-vanilla/lib/python3.6/importlib/_bootstrap.py:219: RuntimeWarning: compiletime version 3.5 of module 'tensorflow.python.framework.fast_tensor_util' does not match runtime version 3.6
  return f(*args, **kwds)
2017-12-16 14:20:38.123035: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
--- 37.22299838066101 seconds tensorflow---
0.03 Tops/sec
```
- Tensorflow CPU optimized version (all 4 CPU threads were used)
```
(tensorflow) steven@box:~/Coding$ python3 matmul_bench.py
--- 14.368497848510742 seconds tensorflow---
0.08 Tops/sec
```
- Tensorflow OpenCL version (iGPU + 2 CPU threads were used)
```
(tensorflow-opencl) steven@box:~/Coding$ python3 matmul_bench.py
2017-12-16 14:23:23.861575: I ./tensorflow/core/common_runtime/sycl/sycl_device.h:70] Found following OpenCL devices:
2017-12-16 14:23:23.861718: I ./tensorflow/core/common_runtime/sycl/sycl_device.h:72] id: 0, type: GPU, name: Carrizo, vendor: Advanced Micro Devices, Inc., profile: FULL_PROFILE
--- 3.8736441135406494 seconds tensorflow---
0.28 Tops/sec
```
