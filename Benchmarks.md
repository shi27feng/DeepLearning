This document will contain some benchmarks of my setup under different Tensorflow setups (non CPU optimized, CPU optimized, OpenCL).

# Benchmark Setup #
- Board: Asrock Fatal1ty X370 Gaming-ITX/ac (Bios 3.40)
- CPU: AMD A12-9800E APU (4 CPU cores and 8 GPU cores [= 512 stream processors])
- RAM: 2x 8 GB DDR4 2400 Kingston HyperX
- Video Card: integrated in CPU, currently only 512 MB set as shared video memory due to the Bios but this will change with new 4.xx Bios versions, then up to 2048 MB shared video memory is usable
- Storage: 256 GB Samsung 950 Pro M.2 SSD
- Lubuntu 17.10
- Kernel: the current version of the 4.14.x branch (via [Ubuntu Mainline Kernels](http://kernel.ubuntu.com/~kernel-ppa/mainline/))
- AMDGPU Pro version 17.50
- Tensorflow 1.4.1 for CPU only tests
- Tensorflow 1.4.0 from the "dev/amd_gpu" branch of the https://github.com/lukeiwanski/tensorflow.git repository

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
# Tensorflow MNIST Example #
- Tensorflow non CPU optimized version (all 4 CPU threads were used)
```
(tensorflow-vanilla) steven@box:~/opt/git/tensorflow-models/tutorials/image/mnist$ time python3 convolutional.py
/home/steven/virtualenvs/tensorflow-vanilla/lib/python3.6/importlib/_bootstrap.py:219: RuntimeWarning: compiletime version 3.5 of module 'tensorflow.python.framework.fast_tensor_util' does not match runtime version 3.6
  return f(*args, **kwds)
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
2017-12-16 14:49:08.757512: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
Initialized!
Step 0 (epoch 0.00), 4.1 ms
Minibatch loss: 8.334, learning rate: 0.010000
Minibatch error: 85.9%
Validation error: 84.6%
Step 100 (epoch 0.12), 292.4 ms
Minibatch loss: 3.252, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.7%
...
Step 8400 (epoch 9.77), 299.6 ms
Minibatch loss: 1.595, learning rate: 0.006302
Minibatch error: 0.0%
Validation error: 0.8%
Step 8500 (epoch 9.89), 299.2 ms
Minibatch loss: 1.614, learning rate: 0.006302
Minibatch error: 1.6%
Validation error: 0.9%
Test error: 0.7%

real	43m13.401s
user	160m58.676s
sys	0m54.217s
```
- Tensorflow CPU optimized version (all 4 CPU threads were used)
```
(tensorflow) steven@box:~/opt/git/tensorflow-models/tutorials/image/mnist$ time python3 convolutional.py
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
Initialized!
Step 0 (epoch 0.00), 3.0 ms
Minibatch loss: 8.334, learning rate: 0.010000
Minibatch error: 85.9%
Validation error: 84.6%
Step 100 (epoch 0.12), 181.8 ms
Minibatch loss: 3.245, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.8%
...
Step 8400 (epoch 9.77), 185.7 ms
Minibatch loss: 1.596, learning rate: 0.006302
Minibatch error: 0.0%
Validation error: 0.8%
Step 8500 (epoch 9.89), 185.0 ms
Minibatch loss: 1.614, learning rate: 0.006302
Minibatch error: 1.6%
Validation error: 0.9%
Test error: 0.8%

real	26m39.144s
user	94m44.365s
sys	1m26.853s
```
- Tensorflow OpenCL version (iGPU + 1 CPU thread were used)
```
(tensorflow-opencl) steven@box:~/opt/git/tensorflow-models/tutorials/image/mnist$ time python3 convolutional.py
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
2017-12-16 16:07:17.911612: I ./tensorflow/core/common_runtime/sycl/sycl_device.h:70] Found following OpenCL devices:
2017-12-16 16:07:17.911797: I ./tensorflow/core/common_runtime/sycl/sycl_device.h:72] id: 0, type: GPU, name: Carrizo, vendor: Advanced Micro Devices, Inc., profile: FULL_PROFILE
Initialized!
Step 0 (epoch 0.00), 4497.7 ms
Minibatch loss: 8.334, learning rate: 0.010000
Minibatch error: 85.9%
Validation error: 84.6%
Step 100 (epoch 0.12), 445.4 ms
Minibatch loss: 3.240, learning rate: 0.010000
Minibatch error: 4.7%
Validation error: 8.1%
...
Step 8400 (epoch 9.77), 456.2 ms
Minibatch loss: 1.596, learning rate: 0.006302
Minibatch error: 0.0%
Validation error: 0.7%
Step 8500 (epoch 9.89), 455.5 ms
Minibatch loss: 1.608, learning rate: 0.006302
Minibatch error: 1.6%
Validation error: 0.9%
Test error: 0.7%

real	72m53.834s
user	36m24.795s
sys	47m18.129s
```

# Tensorflow CIFAR10 Example #
- Tensorflow non CPU optimized version (all 4 CPU threads were used)
```
(tensorflow-vanilla) steven@box:~/opt/git/tensorflow-models/tutorials/image/cifar10$ python3 cifar10_train.py
/home/steven/virtualenvs/tensorflow-vanilla/lib/python3.6/importlib/_bootstrap.py:219: RuntimeWarning: compiletime version 3.5 of module 'tensorflow.python.framework.fast_tensor_util' does not match runtime version 3.6
  return f(*args, **kwds)
Filling queue with 20000 CIFAR images before starting to train. This will take a few minutes.
2017-12-17 20:11:12.936373: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2017-12-17 20:11:17.256330: step 0, loss = 4.67 (283.1 examples/sec; 0.452 sec/batch)
2017-12-17 20:11:25.857986: step 10, loss = 4.61 (148.8 examples/sec; 0.860 sec/batch)
2017-12-17 20:11:34.117354: step 20, loss = 4.43 (155.0 examples/sec; 0.826 sec/batch)
2017-12-17 20:11:42.353447: step 30, loss = 4.48 (155.4 examples/sec; 0.824 sec/batch)
2017-12-17 20:11:50.615841: step 40, loss = 4.35 (154.9 examples/sec; 0.826 sec/batch)
2017-12-17 20:11:58.888452: step 50, loss = 4.33 (154.7 examples/sec; 0.827 sec/batch)
...
```
- Tensorflow CPU optimized version (all 4 CPU threads were used)
```
(tensorflow) steven@box:~/opt/git/tensorflow-models/tutorials/image/cifar10$ python3 cifar10_train.py
Filling queue with 20000 CIFAR images before starting to train. This will take a few minutes.
2017-12-17 19:54:18.040976: step 0, loss = 4.68 (327.0 examples/sec; 0.391 sec/batch)
2017-12-17 19:54:23.714345: step 10, loss = 4.58 (225.6 examples/sec; 0.567 sec/batch)
2017-12-17 19:54:29.196855: step 20, loss = 4.42 (233.5 examples/sec; 0.548 sec/batch)
2017-12-17 19:54:34.862630: step 30, loss = 4.37 (225.9 examples/sec; 0.567 sec/batch)
2017-12-17 19:54:40.559356: step 40, loss = 4.31 (224.7 examples/sec; 0.570 sec/batch)
2017-12-17 19:54:46.158733: step 50, loss = 4.32 (228.6 examples/sec; 0.560 sec/batch)
...
```
- Tensorflow OpenCL version (iGPU + 1 CPU thread were used)
```
(tensorflow-opencl) steven@box:~/opt/git/tensorflow-models/tutorials/image/cifar10$ python3 cifar10_train.py
Filling queue with 20000 CIFAR images before starting to train. This will take a few minutes.
2017-12-17 19:55:54.613673: I ./tensorflow/core/common_runtime/sycl/sycl_device.h:70] Found following OpenCL devices:
2017-12-17 19:55:54.613805: I ./tensorflow/core/common_runtime/sycl/sycl_device.h:72] id: 0, type: GPU, name: Carrizo, vendor: Advanced Micro Devices, Inc., profile: FULL_PROFILE
2017-12-17 20:04:10.015395: step 0, loss = 4.68 (2.6 examples/sec; 49.582 sec/batch)
2017-12-17 20:04:19.847490: step 10, loss = 4.62 (130.2 examples/sec; 0.983 sec/batch)
2017-12-17 20:04:29.133155: step 20, loss = 4.47 (137.8 examples/sec; 0.929 sec/batch)
2017-12-17 20:04:38.102069: step 30, loss = 4.46 (142.7 examples/sec; 0.897 sec/batch)
2017-12-17 20:04:47.246624: step 40, loss = 4.37 (140.0 examples/sec; 0.914 sec/batch)
2017-12-17 20:04:56.185051: step 50, loss = 4.38 (143.2 examples/sec; 0.894 sec/batch)
...
```
