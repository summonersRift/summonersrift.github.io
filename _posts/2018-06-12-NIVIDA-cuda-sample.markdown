---
layout: post
title:  "NVIDIA CUDA Sample"
date:   2018-06-12
---

<p class="intro"><span class="dropcap">N</span>VIDIA CUDA Practice</p>
The purpose of this post is to get started with simple CUDA program and add more materials over time.
Assuming you have NIVIDA cuda toolkit installed, meaning <i>nvcc</i> works)

If you are ready, a library of parallel programs-
https://www.cs.cmu.edu/%7Escandal/nesl/algorithms.html#sort

<br>
Cuda specific material<br> 
https://www.shodor.org/media/content/petascale/materials/UPModules/matrixMultiplication/moduleDocument.pdf

<b>Sample cuda Code— </b>
The following program(<b>add.cu</b>) adds two arrays x and y using CUDA. 
{% highlight c++ %}
/*filename add.cu*/
#include <iostream>
#include <math.h>
// Kernel function to add the elements of two arrays
//global identifies this function as cuda kernel
__global__
void add(int n, float *x, float *y)
{
  int index = threadIdx.x;
  int stride = blockDim.x;
  for (int i = index; i < n; i += stride)
      y[i] = x[i] + y[i];
}

int main(void)
{
  int N = 1<<20;
  float *x, *y;

  // Allocate Unified Memory – accessible from CPU or GPU
  cudaMallocManaged(&x, N*sizeof(float));
  cudaMallocManaged(&y, N*sizeof(float));

  // initialize x and y arrays on the host
  for (int i = 0; i < N; i++) {
    x[i] = 1.0f;
    y[i] = 2.0f;
  }

  // Run kernel on 1M elements on the GPU
  add<<<1, 256>>>(N, x, y);
  // <<< >>>notation to supply grid/block sizes

  // Wait for GPU to finish before accessing on host
  cudaDeviceSynchronize();

  // Check for errors (all values should be 3.0f)
  float maxError = 0.0f;
  for (int i = 0; i < N; i++)
    maxError = fmax(maxError, fabs(y[i]-3.0f));
  std::cout << "Max error: " << maxError << std::endl;

  // Free memory
  cudaFree(x);
  cudaFree(y);
  
  return 0;
}

{% endhighlight %}


<b>Run— </b><br>
&#9658; Run the following command to compile <br>
<pre>nvcc add.cu -o add_cuda</pre>

&#9658; This command to execute <br>
<pre>./add_cuda</pre>

&#9658; And this command to profile (clock/runtime) it, bsically how long it takes to run<br>
<pre>nvprof ./add_cuda</pre>


<b>References— </b>
1. Special thanks to Siraj Raval, https://www.youtube.com/watch?v=1cHx1baKqq0
<br> 
1a. https://github.com/llSourcell/An_Introduction_to_GPU_Programming
<br>
2. CUDA (https://developer.nvidia.com/cuda-downloads)

