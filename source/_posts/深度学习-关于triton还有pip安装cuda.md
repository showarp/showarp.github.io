---
title: 转载-关于windows下triton的安装还有使用pip安装cuda减少系统复杂性
date: 2025-02-19 20:28:57
categories:
    - 机器学习
    - 深度学习
tags:
    - 转载
    - 优化
    - cuda
---
这是来自[triton-windows](https://github.com/woct0rdho/triton-windows/issues/43)的一个issue，个人感觉受益匪浅故此转载，以防以后需要的时候找不到该文章。这样使用conda安装不同pytorch环境的时候就不用去重新安装系统cuda了，可以直接使用pip安装在项目中，并使用环境变量的方式指定cuda的路径，从而避免重复安装系统cuda。

ALMOST all of the CUDA package libraries can be pip-installed and used instead of relying on a system-wide installation of CUDA.  This allows you to:
* Not worry if a user has installed the correct CUDA version; and/or
* Allow users to use a system-wide installation (e.g. for other programs that might require it) while still allowing them to use a compatible version for your specific program's needs...all without having to uninstall/install it system-wide.

To give an example of the **_most common libraries_**...

## 1) pip install CUDA

```pip install nvidia-cuda-runtime-cu12==12.4.127```  
```pip install nvidia-cublas-cu12==12.4.5.8```  
```pip install nvidia-cuda-nvrtc-cu12==12.4.127```  
```pip install nvidia-cuda-nvcc-cu12==12.4.131```  
```pip install nvidia-cufft-cu12==11.2.1.3```  

All of these libraries originate from CUDA release 12.4.1 DESPITE the different version numbers.  This is just the way Nvidia does it.  For example, they might not make any changes to "cublas," but other libraries had significant changes so they issue a new CUDA release - hence the number for cublas might stay the same.

You can see all of the pip-installable specific libraries for all cuda releases in the [.json files here](https://developer.download.nvidia.com/compute/cuda/redist/)

## 2) pip install CuDNN

```pip install nvidia-cudnn-cu12==9.1.0.70```

CuDNN 9+ is massively forwards and backwards compatible.  There are edge cases for very old versions I won't go into.  However, if you want to find the pip install a specific version you can [go here](https://developer.download.nvidia.com/compute/cudnn/redist/cudnn/) and also [check out here](https://developer.download.nvidia.com/compute/cudnn/redist/) to get the pip-installable numbers.

<details><summary>HERE IS A LIST I COMPILED, BUT IT WON'T BE UPDATED IN THIS POST</summary>

```
CUDNN:

8.9.7.29 = pip install nvidia-cudnn-cu12==8.9.7.29 # per nvidia, cuda 12.2 update 1 recommended for newer GPUs
9.1.0 = pip install nvidia-cudnn-cu12==9.1.0.70 # per nvidia, cuda 12.4 recommended for newer gpus
9.1.1 = pip install nvidia-cudnn-cu12==9.1.1.17 # per nvidia, cuda 12.4 recommended for newer gpus
9.2.0 = pip install nvidia-cudnn-cu12==9.2.0.82 # per nvidia, cuda 12.5 recommended for newer gpus
9.2.1 = pip install nvidia-cudnn-cu12==9.2.1.18
9.3.0 = pip install nvidia-cudnn-cu12==9.3.0.75
9.4.0 = pip install nvidia-cudnn-cu12==9.4.0.58
9.5.0 = pip install nvidia-cudnn-cu12==9.5.0.50
9.5.1 = pip install nvidia-cudnn-cu12==9.5.1.17

CUDA 12.6.2:

pip install nvidia-cuda-runtime-cu12==12.6.77
pip install nvidia-cublas-cu12==12.6.3.3
pip install nvidia-cuda-nvcc-cu12==12.6.77
pip install nvidia-cuda-nvrtc-cu12==12.6.77

pip install nvidia-cuda-cupti-cu12==12.6.80
pip install nvidia-cuda-sanitizer-api-cu12==12.6.77
pip install nvidia-cufft-cu12==11.3.0.4
pip install nvidia-curand-cu12==10.3.7.77
pip install nvidia-cusolver-cu12==11.7.1.2
pip install nvidia-cusparse-cu12==12.5.4.2
pip install nvidia-cuda-cuxxfilt-cu12==12.6.77
pip install nvidia-npp-cu12==12.3.1.54
pip install nvidia-nvfatbin-cu12==12.6.77
pip install nvidia-nvjitlink-cu12==12.6.77
pip install nvidia-nvjpeg-cu12==12.3.3.54
pip install nvidia-nvml-dev-cu12==12.6.77
pip install nvidia-nvtx-cu12==12.6.77
pip install nvidia-cuda-opencl-cu12==12.6.77

CUDA 12.6.1:

pip install nvidia-cuda-runtime-cu12==12.6.68
pip install nvidia-cublas-cu12==12.6.1.4
pip install nvidia-cuda-nvcc-cu12==12.6.68
pip install nvidia-cuda-nvrtc-cu12==12.6.68

pip install nvidia-cuda-cupti-cu12==12.6.68
pip install nvidia-cuda-sanitizer-api-cu12==12.6.68
pip install nvidia-cufft-cu12==11.2.6.59
pip install nvidia-curand-cu12==10.3.7.68
pip install nvidia-cusolver-cu12==11.6.4.69
pip install nvidia-cusparse-cu12==12.5.3.3
pip install nvidia-cuda-cuxxfilt-cu12==12.6.68
pip install nvidia-npp-cu12==12.3.1.54
pip install nvidia-nvfatbin-cu12==12.6.68
pip install nvidia-nvjitlink-cu12==12.6.68
pip install nvidia-nvjpeg-cu12==12.3.3.54
pip install nvidia-nvml-dev-cu12==12.6.68
pip install nvidia-nvtx-cu12==12.6.68
pip install nvidia-cuda-opencl-cu12==12.6.68

CUDA 12.6.0:

pip install nvidia-cuda-runtime-cu12==12.6.37
pip install nvidia-cublas-cu12==12.6.0.22
pip install nvidia-cuda-nvcc-cu12==12.6.20
pip install nvidia-cuda-nvrtc-cu12==12.6.20

pip install nvidia-cuda-cupti-cu12==12.6.37
pip install nvidia-cuda-sanitizer-api-cu12==12.6.34
pip install nvidia-cufft-cu12==11.2.6.28
pip install nvidia-curand-cu12==10.3.7.37
pip install nvidia-cusolver-cu12==11.6.4.38
pip install nvidia-cusparse-cu12==12.5.2.23
pip install nvidia-cuda-cuxxfilt-cu12==12.6.20
pip install nvidia-npp-cu12==12.3.1.23
pip install nvidia-nvfatbin-cu12==12.6.20
pip install nvidia-nvjitlink-cu12==12.6.20
pip install nvidia-nvjpeg-cu12==12.3.3.23
pip install nvidia-nvml-dev-cu12==12.6.37
pip install nvidia-nvtx-cu12==12.6.37
pip install nvidia-cuda-opencl-cu12==12.6.37

CUDA 12.5.1:

pip install nvidia-cuda-runtime-cu12==12.5.82
pip install nvidia-cublas-cu12==12.5.3.2
pip install nvidia-cuda-nvcc-cu12==12.5.82
pip install nvidia-cuda-nvrtc-cu12==12.5.82

pip install nvidia-cuda-cupti-cu12==12.5.82
pip install nvidia-cuda-sanitizer-api-cu12==12.5.81
pip install nvidia-cufft-cu12==11.2.3.61
pip install nvidia-curand-cu12==10.3.6.82
pip install nvidia-cusolver-cu12==11.6.3.83
pip install nvidia-cusparse-cu12==12.5.1.3
pip install nvidia-cuda-cuxxfilt-cu12==12.5.82
pip install nvidia-npp-cu12==12.3.0.159
pip install nvidia-nvfatbin-cu12==12.5.82
pip install nvidia-nvjitlink-cu12==12.5.82
pip install nvidia-nvjpeg-cu12==12.3.2.81
pip install nvidia-nvml-dev-cu12==12.5.82
pip install nvidia-nvtx-cu12==12.5.82
pip install nvidia-cuda-opencl-cu12==12.5.39

CUDA 12.5.0:

pip install nvidia-cuda-runtime-cu12==12.5.39
pip install nvidia-cublas-cu12==12.5.2.13
pip install nvidia-cuda-nvcc-cu12==12.5.40
pip install nvidia-cuda-nvrtc-cu12==12.5.40

pip install nvidia-cuda-cupti-cu12==12.5.39
pip install nvidia-cuda-sanitizer-api-cu12==12.5.39
pip install nvidia-cufft-cu12==11.2.3.18
pip install nvidia-curand-cu12==10.3.6.39
pip install nvidia-cusolver-cu12==11.6.2.40
pip install nvidia-cusparse-cu12==12.4.1.24
pip install nvidia-cuda-cuxxfilt-cu12==12.5.39
pip install nvidia-npp-cu12==12.3.0.116
pip install nvidia-nvfatbin-cu12==12.5.39
pip install nvidia-nvjitlink-cu12==12.5.40
pip install nvidia-nvjpeg-cu12==12.3.2.38
pip install nvidia-nvml-dev-cu12==12.5.39
pip install nvidia-nvtx-cu12==12.5.39
pip install nvidia-cuda-opencl-cu12==12.5.39

CUDA 12.4.1:

pip install nvidia-cuda-runtime-cu12==12.4.127
pip install nvidia-cublas-cu12==12.4.5.8
pip install nvidia-cuda-nvcc-cu12==12.4.131
pip install nvidia-cuda-nvrtc-cu12==12.4.127
pip install nvidia-cufft-cu12==11.2.1.3

pip install nvidia-cuda-cupti-cu12==12.4.127
pip install nvidia-cuda-sanitizer-api-cu12==12.4.127
pip install nvidia-cufft-cu12==11.2.1.3
pip install nvidia-curand-cu12==10.3.5.147
pip install nvidia-cusolver-cu12==11.6.1.9
pip install nvidia-cusparse-cu12==12.3.1.170
pip install nvidia-cuda-cuxxfilt-cu12==12.4.127
pip install nvidia-npp-cu12==12.2.5.30
pip install nvidia-nvfatbin-cu12==12.4.127
pip install nvidia-nvjitlink-cu12==12.4.127
pip install nvidia-nvjpeg-cu12==12.3.1.117
pip install nvidia-nvml-dev-cu12==12.4.127
pip install nvidia-nvtx-cu12==12.4.127
pip install nvidia-cuda-opencl-cu12==12.4.127

CUDA 12.4.0:

pip install nvidia-cuda-runtime-cu12==12.4.99
pip install nvidia-cublas-cu12==12.4.2.65
pip install nvidia-cuda-nvcc-cu12==12.4.99
pip install nvidia-cuda-nvrtc-cu12==12.4.99

pip install nvidia-cuda-cupti-cu12==12.4.99
pip install nvidia-cuda-sanitizer-api-cu12==12.4.99
pip install nvidia-cufft-cu12==11.2.0.44
pip install nvidia-curand-cu12==10.3.5.119
pip install nvidia-cusolver-cu12==11.6.0.99
pip install nvidia-cusparse-cu12==12.3.0.142
pip install nvidia-cuda-cuxxfilt-cu12==12.4.99
pip install nvidia-npp-cu12==12.2.5.2
pip install nvidia-nvfatbin-cu12==12.4.99
pip install nvidia-nvjitlink-cu12==12.4.99
pip install nvidia-nvjpeg-cu12==12.3.1.89
pip install nvidia-nvml-dev-cu12==12.4.99
pip install nvidia-nvtx-cu12==12.4.99
pip install nvidia-cuda-opencl-cu12==12.4.99

CUDA 12.2.2:

pip install nvidia-cuda-runtime-cu12==12.2.140
pip install nvidia-cublas-cu12==12.2.5.6
pip install nvidia-cuda-nvcc-cu12==12.2.140
pip install nvidia-cuda-nvrtc-cu12==12.2.140

pip install nvidia-cuda-cupti-cu12==12.2.142
pip install nvidia-cuda-sanitizer-api-cu12==12.2.140
pip install nvidia-cufft-cu12==11.0.8.103
pip install nvidia-curand-cu12==10.3.3.141
pip install nvidia-cusolver-cu12==11.5.2.141
pip install nvidia-cusparse-cu12==12.1.2.141
pip install nvidia-cuda-cuxxfilt-cu12==12.2.140
pip install nvidia-npp-cu12==12.2.1.4
pip install nvidia-nvjitlink-cu12==12.2.140
pip install nvidia-nvjpeg-cu12==12.2.2.4
pip install nvidia-nvml-dev-cu12==12.2.140
pip install nvidia-nvtx-cu12==12.2.140
pip install nvidia-cuda-opencl-cu12==12.2.140

CUDA 12.1.1:

pip install nvidia-cuda-runtime-cu12==12.1.105
pip install nvidia-cublas-cu12==12.1.3.1
pip install nvidia-cuda-nvrtc-cu12==12.1.105
pip install nvidia-cuda-nvcc-cu12==12.1.105

pip install nvidia-cuda-cupti-cu12==12.1.105
pip install nvidia-cuda-sanitizer-api-cu12==12.1.105
pip install nvidia-cufft-cu12==11.0.2.54
pip install nvidia-curand-cu12==10.3.2.106
pip install nvidia-cusolver-cu12==11.4.5.107
pip install nvidia-cusparse-cu12==12.1.0.106
pip install nvidia-cuda-cuxxfilt-cu12==12.1.105
pip install nvidia-npp-cu12==12.1.0.40
pip install nvidia-nvfatbin-cu12==12.1.105
pip install nvidia-nvjitlink-cu12==12.1.105
pip install nvidia-nvjpeg-cu12==12.2.0.2
pip install nvidia-nvml-dev-cu12==12.1.105
pip install nvidia-nvtx-cu12==12.1.105
pip install nvidia-cuda-opencl-cu12==12.1.105

CUDA 12.1.0:

pip install nvidia-cuda-runtime-cu12==12.1.55
pip install nvidia-cublas-cu12==12.1.0.26
pip install nvidia-cuda-nvcc-cu12==12.1.66
pip install nvidia-cuda-nvrtc-cu12==12.1.55

pip install nvidia-cuda-cupti-cu12==12.1.62
pip install nvidia-cuda-sanitizer-api-cu12==12.1.55
pip install nvidia-cufft-cu12==11.0.2.4
pip install nvidia-curand-cu12==10.3.2.56
pip install nvidia-cusolver-cu12==11.4.4.55
pip install nvidia-cusparse-cu12==12.0.2.55
pip install nvidia-cuda-cuxxfilt-cu12==12.1.55
pip install nvidia-npp-cu12==12.0.2.50
pip install nvidia-nvfatbin-cu12==12.1.55
pip install nvidia-nvjitlink-cu12==12.1.55
pip install nvidia-nvjpeg-cu12==12.1.0.39
pip install nvidia-nvml-dev-cu12==12.1.55
pip install nvidia-nvtx-cu12==12.1.66
pip install nvidia-cuda-opencl-cu12==12.1.56

CUDA 11.8.0:

pip install nvidia-cuda-runtime-cu11==11.8.89
pip install nvidia-cublas-cu11==11.11.3.6
pip install nvidia-cuda-nvcc-cu11==11.8.89
pip install nvidia-cuda-nvrtc-cu11==11.8.89

pip install nvidia-cuda-cupti-cu11==11.8.87
pip install nvidia-cuda-sanitizer-api-cu11==11.8.86
pip install nvidia-cufft-cu11==10.9.0.58
pip install nvidia-curand-cu11==10.3.0.86
pip install nvidia-cusolver-cu11==11.4.1.48
pip install nvidia-cusparse-cu11==11.7.5.86
pip install nvidia-cuda-cuxxfilt-cu11==11.8.86
pip install nvidia-npp-cu11==11.8.0.86
pip install nvidia-nvfatbin-cu11==11.8.86
pip install nvidia-nvjitlink-cu11==11.8.86
pip install nvidia-nvjpeg-cu11==11.9.0.86
pip install nvidia-nvml-dev-cu11==11.8.86
pip install nvidia-nvtx-cu11==11.8.86
pip install nvidia-cuda-opencl-cu11==11.8.86
```
</details>

## 3) pip install triton wheel

For example:

```pip install https://github.com/woct0rdho/triton-windows/releases/download/v3.1.0-windows.post8/triton-3.1.0-cp311-cp311-win_amd64.whl```
* make sure and install the correct wheel in the releases page

## 4) Include something like this in the entry point for your program, at the very beginning of your script
* Change this depending what all cuda-related libraries your program ACTUALLY relies on

```python
import sys
import os
from pathlib import Path

def set_cuda_paths():
    venv_base = Path(sys.executable).parent.parent
    nvidia_base_path = venv_base / 'Lib' / 'site-packages' / 'nvidia'
    cuda_path_runtime = nvidia_base_path / 'cuda_runtime' / 'bin'
    cuda_path_runtime_lib = nvidia_base_path / 'cuda_runtime' / 'bin' / 'lib' / 'x64'
    cuda_path_runtime_include = nvidia_base_path / 'cuda_runtime' / 'include'
    cublas_path = nvidia_base_path / 'cublas' / 'bin'
    cudnn_path = nvidia_base_path / 'cudnn' / 'bin'
    nvrtc_path = nvidia_base_path / 'cuda_nvrtc' / 'bin'
    nvcc_path = nvidia_base_path / 'cuda_nvcc' / 'bin'
    
    paths_to_add = [
        str(cuda_path_runtime),
        str(cuda_path_runtime_lib),
        str(cuda_path_runtime_include),
        str(cublas_path),
        str(cudnn_path),
        str(nvrtc_path),
        str(nvcc_path),
    ]
    
    current_value = os.environ.get('PATH', '')
    new_value = os.pathsep.join(paths_to_add + [current_value] if current_value else paths_to_add)
    os.environ['PATH'] = new_value
    
    # i blame triton
    triton_cuda_path = nvidia_base_path / 'cuda_runtime'
    os.environ['CUDA_PATH'] = str(triton_cuda_path)

set_cuda_paths()
```

This will prepend the relevant PATH/CUDA_PATH variables (but only when the program runs) to specify where you pip-installed all of the necessary CUDA/CuDNN/Triton libraries. This will force your program to FIRST look for the libraries where you pip-installed them but WILL NOT otherwise remove the other paths that other program may rely on.

### Remember...if your program creates a new process/subprocess you must either pass these environment variables or, alternatively, re-invoke the "set_cuda_paths" function to set them for the new process/subprocess.

## 5)  Additional steps

```Triton``` requires ```ptxas.exe``` and, assuming you set the paths within something like the ```set_cuda_paths``` function above, Triton will look for this important file in ```\Lib\site-packages\nvidia\cuda_runtime\bin```.  If it's not found, it will give an error.

However, pip-installing ```nvidia-cuda-nvcc-cu12``` puts ```ptxas.exe``` here instead: ```\Lib\site-packages\nvidia\cuda_nvcc\bin\ptxas.exe```.

![Image](https://github.com/user-attachments/assets/5b78c301-b9b5-4450-a2e4-b357d1243ccb)

Therefore, I recommend manually copying this file to the ```cuda_runtime\bin``` directory as seen here:

![Image](https://github.com/user-attachments/assets/6719cd4c-c943-4df0-85fe-d26c19cbda05)

Alternatively, you can do this automatically so a user doesn't have to worry about it, as seen in this example:

https://github.com/BBC-Esq/VectorDB-Plugin-for-LM-Studio/blob/d3f6fb0c4dd4e7050454298f866f92eb2176a470/src/replace_sourcecode.py#L104

### Also...

When pip-install for some reason a "lib" folder is missing that ```triton``` requires.  It looks for this folder at ```\Lib\site-packages\nvidia\cuda_runtime\```

From release v3.1.0-windows.post8 onwards, "cuda_12.4_lib.zip" is provided.  The contents of this .ZIP file merely need to be placed within the ```\Lib\site-packages\nvidia\cuda_runtime\```` directory where you pip-installed the CUDA-related libraries.

Alternatively, if you encounter compatibility issues or simply want to download a different CUDA release version for the ```lib``` folder, [I have provided a link near the bottom](https://github.com/woct0rdho/triton-windows/issues/43#issuecomment-2604721075) of this github issue to my repository that allows you to do this.  Again, once downloaded, place the folder in the ```\Lib\site-packages\nvidia\cuda_runtime\``` directory.

### Chances are if you're using Triton, CUDA, etc. you're using other libraries as well.  Below is additional information I've created to help people save time.

## Intro

Too many users simply install the latest version of CUDA thinking "latest must mean greatest."  The reality is, very few libraries are compatible with the "latest" CUDA.  Typically, libraries (even Torch) are NOT compatible with the latest CUDA.  This leads to the situation where novice users simply install the latest CUDA thinking they've done things correctly, but in reality, they need an older version.  The following tables try to clarify this for CUDA as well as other common libraries.

## Torch

The ```torch``` library's pre-built wheels (i.e. not compiling from source) are only tested with certain versions of CUDA.  (Compiling from source allows for more permutations of compatibility...)

First, ```torch``` only provides pre-built wheels that have been specifically tested on CUDA release 12.4.1 or 12.1.1 as follows:

| Pytorch Wheel | PyTorch Versions Supported                              |
| ------------- | ------------------------------------------------------- |
| cu126         | 2.6.0                                                   |
| cu124         | 2.5.1, 2.5.0, 2.4.1, 2.4.0                              |
| cu121         | 2.5.1, 2.5.0, 2.4.1, 2.4.0, 2.3.1, 2.3.0, 2.2.2...2.1.0 |

* Notice how torch is NOT 100% compatible with CUDA 12.1.0 or 12.4.0, for example, or any other version.
* Also, ```torch==2.6.0``` is the only one that is compatible with CUDA 12.6 currently.

Below is a table of the version of Triton that Torch says its compatible with:

| Torch | Triton |
| ----- | ------ |
| 2.6.0 | 3.2.0  |
| 2.5.1 | 3.1.0  |
| 2.5.0 | 3.1.0  |
| 2.4.1 | 3.0.0  |
| 2.4.0 | 3.0.0  |
| 2.3.1 | 2.3.1  |
| 2.3.0 | 2.3.0  |
| 2.2.2 | 2.2.0  |

* In other words, ```torch==2.5.1``` is NOT compatible with ```triton==3.0.0```, at least according to PyTorch.

PyTorch's official "matrix" of compatibility regarding CUDA/CuDNN can be seen here:

https://github.com/pytorch/pytorch/blob/main/RELEASE.md#release-compatibility-matrix

And here are the most recent versions and their pip-installable counterparts:

| Torch Version     | cuda-nvrtc | cuda-runtime | cublas   | cufft     | cudnn    | triton |
| ----------------- | ---------- | ------------ | -------- | --------- | -------- | ------ |
| 2.6.0 (CUDA 12.6) | 12.6.77    | 12.6.77      | 12.6.4.1 | 11.3.0.4  | 9.5.1.17 | 3.2.0  |
| 2.6.0 (CUDA 12.4) | 12.4.127   | 12.4.127     | 12.4.5.8 | 11.2.1.3  | 9.1.0.70 | 3.2.0  |
| 2.5.1 (CUDA 12.4) | 12.4.127   | 12.4.127     | 12.4.5.8 | 11.2.1.3  | 9.1.0.70 | 3.1.0  |
| 2.5.1 (CUDA 12.1) | 12.1.105   | 12.1.105     | 12.1.3.1 | 11.0.2.54 | 9.1.0.70 | 3.1.0  |

NOT installing a version of CUDA that ```torch``` tests with may lead to errors, but not 100%, just be aware if you want to use a different version of CUDA in your program.

You can get the most up-to-date information by examining the ```generate_binary_build_matrix.py``` script on torch's github repository.

## Xformers (for Windows only)

Xformers pre-built wheels are STRICTLY tied to a specific version of ```torch```.  You WILL encounter errors if you don't install a correct version.

NOTE: Windows wheels up to and including ```torch==2.4.0``` are available on pypi (e.g. ```pip install xformers```).  Afterwards, only PyTorch themselves compile them them here:
[https://download.pytorch.org/whl/cu124/xformers/](https://download.pytorch.org/whl/cu124/xformers/) 
[https://download.pytorch.org/whl/cu121/xformers/](https://download.pytorch.org/whl/cu121/xformers/)

| Xformers Version | Torch Version | Notes                                                                  |
| ---------------- | ------------- | ---------------------------------------------------------------------- |
| v0.0.29.post1    | 2.5.1         |                                                                        |
| v0.0.29          | 2.5.1         | BUG, don't use                                                         |
| v0.0.28.post3    | 2.5.1         |                                                                        |
| v0.0.28.post2    | 2.5.0         |                                                                        |
| v0.0.28.post1    | 2.4.1         |                                                                        |
| v0.0.27.post2    | 2.4.0         |                                                                        |
| v0.0.27.post1    | 2.4.0         |                                                                        |
| v0.0.27          | 2.3.0         | Release notes confusingly say "some operation might require torch 2.4" |
| v0.0.26.post1    | 2.3.0         |                                                                        |
| v0.0.25.post1    | 2.2.2         |                                                                        |

## Triton
### Triton 3.0.0
* I've only tested version 3.0.0 and earlier wheels located here: https://github.com/jakaline-dev/Triton_win/releases
* However, I noticed this repository also has 3.0.0 wheels.
* **_Regardless of which ones you use, Triton 3.0.0 only supports up to Python 3.11_**

### Triton 3.1.0+

* Use this repository's.
 * **_Requires torch 2.4.0+_**
 * "The wheels are built against CUDA 12.5, and they should work with other CUDA 12.x."
 * **_Supports Python 3.12_**

## Flash Attention 2

First, you WILL NOT see Windows releases for FA2 for every Linux release...that's just a decision by the repo's owner.  Here is the best repo I've found for Windows wheels:

https://github.com/bdashore3/flash-attention/releases/

```Flash Attention 2``` is very particular with both ```torch``` and ```cuda``` (almost as bad as ```xformers```).  The full compatibility for the pre-built Windows wheels are as follows:

NOTICE how FA2 only supports specific versions...

| FA2 Version  | Torch Versions      | CUDA Versions |
| ------------ | ------------------- | ------------- |
| v2.7.1.post1 | 2.3.1, 2.4.0, 2.5.1 | 12.4.1        |
| v2.7.0.post2 | 2.3.1, 2.4.0, 2.5.1 | 12.4.1        |
| v2.6.3       | 2.2.2, 2.3.1, 2.4.0 | 12.3.2        |
| v2.6.1       | 2.2.2, 2.3.1        | 12.3.2        |
| v2.5.9.post2 | 2.2.2, 2.3.1        | 12.2.2        |
| v2.5.9.post1 | 2.2.2, 2.3.0        | 12.2.2        |
| v2.5.8       | 2.2.2, 2.3.0        | 12.2.2        |
| v2.5.6       | 2.1.2, 2.2.2        | 12.2.2        |
| v2.5.2       | 2.1.2, 2.2.0        | 12.2.2        |
| v2.4.2       | 2.1.2, 2.2.0        | 12.2.2        |

* Flash Attention 2 only supports the following model architectures: https://huggingface.co/docs/transformers/v4.47.1/en/perf_infer_gpu_one

## As you can see, if using FA2 there is no need to install any CUDA version above 12.4.1.  Again, if you compile from source its different...but when using pre-built wheels you NEVER need above CUDA 12.4.1.

IMPORTANT: Newer versions of FA2 actually regressed...they no longer support CUDA 12.4.1.  See here for more details:

https://github.com/BBC-Esq/VectorDB-Plugin-for-LM-Studio/blob/d3f6fb0c4dd4e7050454298f866f92eb2176a470/src/constants.py#L2799

For versions of FA2 that come out after this post, you can check compatibility by examining the "publish.yml" file on their repository:

```flash-attention/.github/workflows/publish.yml```
[link here](https://github.com/Dao-AILab/flash-attention/blob/6b1d059eda21c1bd421f3d352786fca2cab61954/.github/workflows/publish.yml#L35)

Remember, you'll still need to doublecheck whether a corresponding Windows release has been made at:

https://github.com/bdashore3/flash-attention/releases/

## Pydantic

Python 3.12.4 is incompatible with pydantic.v1 as of pydantic==2.7.3
https://github.com/langchain-ai/langchain/issues/22692
***Everything should now be fine as long as Langchain 0.3+ is used, which requires pydantic version 2+***

Other libraries can be checked at: https://pyreadiness.org/3.12/

## In conclusion, please consider pip-installing CUDA/CuDNN libraries to simplify the procedure for novice programmers like myself.  Hope this helps!
