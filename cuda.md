## 安装

在已经安装好nvdia driver的基础上，只需要安装cuda-toolkit

**查看driver版本**

```
nvidia-smi                                
Thu Jun 29 12:15:02 2023       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.116.04   Driver Version: 525.116.04   CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0  On |                  N/A |
|  0%   51C    P8    19W / 200W |    337MiB /  8192MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      1325      G   /usr/lib/xorg/Xorg                249MiB |
|    0   N/A  N/A      1607      G   /usr/bin/gnome-shell               28MiB |
|    0   N/A  N/A      2220      G   ...mviewer/tv_bin/TeamViewer       14MiB |
|    0   N/A  N/A    296705      G   ...182476128516229679,262144       41MiB |
+-----------------------------------------------------------------------------+

```

**google "cuda 12.0 download"**

https://developer.nvidia.com/cuda-12-0-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_network

注意最后不要`sudo apt-get -y install cuda`那会重新下载驱动，而应该`sudo apt-get -y install cuda-toolkit`

**修改env path**

```
# chenhui's env variables
# CLion 
CLION_PATH=/home/chenhui/App/clion-2023.1.2/bin
# Cuda
CUDA_PATH=/usr/local/cuda-12.0/bin
PATH=$PATH:$CLION_PATH:$CUDA_PATH


```



**安装错版本怎么办**

1. cuda 路径

```
$ ll /usr/local | grep cuda
lrwxrwxrwx  1 root    root      22  6月 29 10:43 cuda -> /etc/alternatives/cuda
lrwxrwxrwx  1 root    root      25  6月 29 10:43 cuda-12 -> /etc/alternatives/cuda-12
drwxr-xr-x 15 root    root    4.0K  6月 29 12:12 cuda-12.0
```

2. 把下错的版本先删掉

```mv cuda-12.2 /.trash```

3. 把软连接都改了

```
sudo ln -sf /usr/local/cuda-12.0 cuda

-sf 表示修改  
ln -sf <A> <B>
B指向A
```

4. 修改env path
5. `nvcc --version`

## Hello world

```cpp
#include <iostream>
#include <cuda_runtime.h>

__global__ void helloCUDA()
{
    printf("Hello, CUDA World!\n");
}

int main()
{
    // Launch the kernel on the GPU with one thread
    helloCUDA<<<1, 1>>>();

    // Wait for the GPU to finish
    cudaDeviceSynchronize();

    // Check for errors during the kernel launch
    cudaError_t error = cudaGetLastError();
    if (error != cudaSuccess)
    {
        std::cerr << "CUDA error: " << cudaGetErrorString(error) << std::endl;
        return -1;
    }

    std::cout << "Kernel executed successfully." << std::endl;

    return 0;
}
```



## Cuda Model

**kernel**： c++ function，使用`__global__`关键词  分配给n个cuda threads 同时执行

