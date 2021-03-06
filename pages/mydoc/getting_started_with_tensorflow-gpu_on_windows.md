---
title: "Getting Started With TensorFlow-gpu On Windows"
keywords: tensorflow, gpu
date: 2020-12-28 03:43:45 -0800
last_updated: December 28, 2020
tags: [deep_learning]
summary: "TensorFlow has been a powerful yet subtle deep learning framework for the research community and the industry
alike. Today let me devote this post to getting readers started with TensorFlow's GPU packages on Windows, from
essentially the very beginning."
sidebar: none
permalink: getting_started_with_tensorflow-gpu_on_windows.html
folder: mydoc
---

TensorFlow has been a powerful yet subtle deep learning framework for the research community and the industry alike.
Today let me devote this post to getting readers started with TensorFlow's GPU packages, from essentially the very
beginning. Throughout our discussion, I will showcase all installation procedures for a typical Windows 10 OS. Similar
procedures apply to <font face="Lora">Unix</font>/<font face="Lora">Linux</font>-based OSes, such as <font face="Lora">
Ubuntu-18.04</font>, and will be explained in detail in upcoming posts.

## My Operating System And Other Settings
My <font face="Lora">LEGION</font> (a sub-brand of <font face="Lora">LENOVO</font>) laptop has Windows 10 installed. The
machine's important specs are found by running command `systeminfo` in Windows 10's `CMD` terminal (immaterial specs are
omitted as `...` for simplicity):

```
...
OS Name:                   Microsoft Windows 10 Home China
OS Version:                10.0.18363 N/A Build 18363
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Workstation
OS Build Type:             Multiprocessor Free
...
System Manufacturer:       LENOVO
System Model:              81LC
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 158 Stepping 10 GenuineIntel ~2208 Mhz
BIOS Version:              LENOVO 9VCN12WW, 8/6/2018
Windows Directory:         C:\WINDOWS
System Directory:          C:\WINDOWS\system32
Boot Device:               \Device\HarddiskVolume1
...
Total Physical Memory:     8,065 MB
Available Physical Memory: 1,832 MB
Virtual Memory: Max Size:  21,889 MB
Virtual Memory: Available: 12,674 MB
Virtual Memory: In Use:    9,215 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              \\LAPTOP-BFD2CE3C
Hotfix(s):                 22 Hotfix(s) Installed.
...
Network Card(s):           3 NIC(s) Installed.
...
Hyper-V Requirements:      VM Monitor Mode Extensions: Yes
                           Virtualization Enabled In Firmware: No
                           Second Level Address Translation: Yes
                           Data Execution Prevention Available: Yes
```

## Install Pycharm (Optional But Recommended)
Many believe <font face="Lora">PyCharm</font> is one of the best (if not the best) Integrated Development Environments
(IDEs) for Python programming. It comes with powerful tools for code editing, navigating, refactoring, debugging, etc.
The <font face="Lora">community version</font> of this software is free and you can download it through
[its official website](https://www.jetbrains.com/pycharm/download). To facilitate our discussion, I am supposing that
your machine already has PyCharm installed.

## Install Python & Conda
&#000;<font face="Lora">Conda</font> is an open-source package management system and environment management system that
runs on multiple OSes. It was created for Python programs but it can package and distribute software for nearly any
language (e.g. C++/Java), with a focus on data sciences. Conda manager gives you the ability to create multiple
environments with different versions of Python and other libraries. This becomes useful when some codes are written with
specific versions of a library. The Conda package and environment manager are included in all versions
of <font face="Lora">Anaconda</font>, <font face="Lora">Miniconda</font>,
and <font face="Lora">Anaconda Repository</font>.

&#000;<font face="Lora">Anaconda</font> is the world's most popular open-source, multi-channel Python distribution
platform for free public Conda package hosting. The <font face="Lora">individual edition</font> of this software is free
and you can download it through [its official website](https://www.anaconda.com/products/individual). To facilitate our
discussion, I am supposing that your machine already has Anaconda installed.

The good news is Anaconda provides integrated package installation and environment management utilities supported by
Conda, for Python IDEs like PyCharm. The bad news is, however, not all Python packages are allowed to be installed from
within Anaconda, such as <font face="Lora">jieba</font>, a frequently used natural language processing package for
Chinese mandarin.

For the next step, you have two options to install TensorFlow from scratch, using `pip` or `conda`. As a personal
preference, I suggest using `pip`since it provides users with more flexibility.

## Install TensorFlow Using pip
It is advisable to create a new, dedicated environment using Conda to accommodate TensorFlow. You can create a Conda
environment from the installed Anaconda Navigator and have almost all the required packages there. Although searching
plainly for "tensorflow" will prompt you to install TensorFlow, it might not be the specific version that serves your
purpose. To resolve this, you probably have to download the specific version from TensorFlow's official website (e.g.
`tensorflow-2.3.0`). If downloading from e.g. [PyPI](https://pypi.org) with the web browser
(e.g. <font face="Lora">Chrome</font>, <font face="Lora">Internet Explorer</font>) is unsuccessful due to poor Internet
connections, you are encouraged to try an alternative software, such as <font face="Lora">BitTorrent</font>.

## Install CUDA Drivers
Before you start, check that your machine should have GPUs, and should have their drivers correctly installed. Normally,
you don't need to worry about this if your machine is pre-installed with Windows 10 and its corresponding hardware
drivers, for example. Otherwise, you'll probably have to download and install the appropriate GPU drivers. I recommend
you to search [NVIDIA's official website](https://www.nvidia.com/en-us/geforce/drivers) for authentic NVIDIA Geforce
drivers, rather than test your Google-Fu by making a random search.

If you want to use the GPU version of TensorFlow you must have a CUDA-enabled GPU. (My <font face="Lora">LEGION</font>
laptop has an <font face="Lora">NVIDIA GeForce GTX 1060 Graphic Card</font>, which is a CUDA-enabled GPU.) Then you need
to install <font face="Lora">CUDA</font> and <font face="Lora">cuDNN</font> with their appropriate versions.

## Install Appropriate CUDA Version
The version numbers should be determined by the TensorFlow version you have installed. For example, after running a
TensorFlow-imported Python script, my laptop generated the following console output, explicitly specifying that the
required .dll version was `cudart64_101.dll`, and hence the required CUDA version was 10.1 (Interesting enough,
`tensorflow-2.4.0` would instead require CUDA version `11.0` and `cudart64_110.dll`.):

```
2020-12-27 08:24:01.624720: W tensorflow/stream_executor/platform/default/dso_loader.cc:59] Could not load dynamic library 'cudart64_101.dll'; dlerror: cudart64_101.dll not found
2020-12-27 08:24:01.625448: I tensorflow/stream_executor/cuda/cudart_stub.cc:29] Ignore above cudart dlerror if you do not have a GPU set up on your machine.
```

Next, find CUDA 10.1 in its appropriate format, depending on your machine's operating system (Windows/Ubuntu/...),
architecture (x86_64/ppc/...), OS version (Windows 10/Windows Server 2019/...), etc. You might choose installer types
between "network" (recommended when you have a low-bandwidth Internet connection) and "local" (recommended when you have
access to a high-bandwidth Internet connection) installer types.

## Install Appropriate cuDNN Version
Next, you should find the appropriate cuDNN version depending on your CUDA version, and find the appropriate choice of
that cuDNN version according to your machine's operating system, architecture, OS version, etc. as before. For CUDA
10.1, the newest appropriate cuDNN version is `8.0.4` (as of Oct. 2020). After unzipping the downloaded cuDNN .zip file,
merge the folder into the CUDA installation folder. That is, copy and paste files in the unzipped cuDNN folder into the
same hierarchical places in the CUDA installation folder. (This iterative work is done automatically in Windows 10 by
only manipulating the top-level folder.) 

## Maybe You'll Need To Download Archive cuDNN Versions Together
After that, rerun the TensorFlow-imported Python script and see if error messages still appear. If so, search Google for
the missing .dll file, which may be contained in an archive version of cuDNN. A little bit strange, though. For example,
my laptop output a different error message at this moment:

```
2020-12-27 08:29:02.355320: W tensorflow/stream_executor/platform/default/dso_loader.cc:59] Could not load dynamic library 'cudnn64_7.dll'; dlerror: cudnn64_7.dll not found
2020-12-27 08:29:02.355321: W tensorflow/core/common_runtime/gpu/gpu_device.cc:1753] Cannot dlopen some GPU libraries. Please make sure the missing libraries mentioned above are installed properly if you would like to use GPU. Follow the guide at https://www.tensorflow.org/install/gpu for how to download and setup the required libraries for your platform.
Skipping registering GPU devices...
```

By searching Google, you probably will find out that `cudnn64_7.dll` is included in cuDNN version `7.6.4`, for example.
After placing this .dll into folder `bin` of the CUDA installation folder, a third try suggested that all required files
concerning our GPUs should be in place:

```
2020-12-30 19:10:49.972247: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cudart64_101.dll
2020-12-30 19:10:54.278534: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library nvcuda.dll
2020-12-30 19:10:55.162969: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1716] Found device 0 with properties: 
pciBusID: 0000:01:00.0 name: GeForce GTX 1060 computeCapability: 6.1
coreClock: 1.6705GHz coreCount: 10 deviceMemorySize: 6.00GiB deviceMemoryBandwidth: 178.99GiB/s
2020-12-30 19:10:55.163257: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cudart64_101.dll
2020-12-30 19:10:55.168925: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cublas64_10.dll
2020-12-30 19:10:55.172793: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cufft64_10.dll
2020-12-30 19:10:55.174744: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library curand64_10.dll
2020-12-30 19:10:55.179697: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cusolver64_10.dll
2020-12-30 19:10:55.183451: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cusparse64_10.dll
2020-12-30 19:10:55.304199: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cudnn64_7.dll
2020-12-30 19:10:55.304419: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1858] Adding visible gpu devices: 0
2020-12-30 19:10:55.305040: I tensorflow/core/platform/cpu_feature_guard.cc:142] This TensorFlow binary is optimized with oneAPI Deep Neural Network Library (oneDNN)to use the following CPU instructions in performance-critical operations:  AVX2
To enable them in other operations, rebuild TensorFlow with the appropriate compiler flags.
2020-12-30 19:10:55.315578: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x27e9b16a5e0 initialized for platform Host (this does not guarantee that XLA will be used). Devices:
2020-12-30 19:10:55.315855: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (0): Host, Default Version
2020-12-30 19:10:55.316185: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1716] Found device 0 with properties: 
pciBusID: 0000:01:00.0 name: GeForce GTX 1060 computeCapability: 6.1
coreClock: 1.6705GHz coreCount: 10 deviceMemorySize: 6.00GiB deviceMemoryBandwidth: 178.99GiB/s
2020-12-30 19:10:55.316619: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cudart64_101.dll
2020-12-30 19:10:55.316912: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cublas64_10.dll
2020-12-30 19:10:55.317086: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cufft64_10.dll
2020-12-30 19:10:55.317262: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library curand64_10.dll
2020-12-30 19:10:55.317398: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cusolver64_10.dll
2020-12-30 19:10:55.317537: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cusparse64_10.dll
2020-12-30 19:10:55.317676: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library cudnn64_7.dll
2020-12-30 19:10:55.317839: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1858] Adding visible gpu devices: 0
2020-12-30 19:10:58.230954: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1257] Device interconnect StreamExecutor with strength 1 edge matrix:
2020-12-30 19:10:58.231117: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1263]      0 
2020-12-30 19:10:58.231209: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1276] 0:   N 
2020-12-30 19:10:58.231444: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1402] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 4694 MB memory) -> physical GPU (device: 0, name: GeForce GTX 1060, pci bus id: 0000:01:00.0, compute capability: 6.1)
2020-12-30 19:10:58.237027: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x27ec2fe9ed0 initialized for platform CUDA (this does not guarantee that XLA will be used). Devices:
2020-12-30 19:10:58.237211: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (0): GeForce GTX 1060, Compute Capability 6.1
```

## Performance Test
A code snippet from [TensorFlow's official guide page](https://www.tensorflow.org/guide/gpu) verified the usability of
GPUs:

```python
import tensorflow as tf
print("Num GPUs Available: ", len(tf.config.experimental.list_physical_devices('GPU')))
```

and the console output the number of GPUs available:

```
Num GPUs Available:  1
```

Another code snippet convinced you that your GPUs performed computations as expected:

```python
import tensorflow as tf
tf.debugging.set_log_device_placement(True)

# Create some tensors
a = tf.constant([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
b = tf.constant([[1.0, 2.0], [3.0, 4.0], [5.0, 6.0]])
c = tf.matmul(a, b)

print(c)
```

and the console output the location to execute operations:

```
Executing op MatMul in device /job:localhost/replica:0/task:0/device:GPU:0
tf.Tensor(
[[22. 28.]
 [49. 64.]], shape=(2, 2), dtype=float32)
```

If all prerequisites are installed correctly, the GPUs should perform computation instead of CPU whenever possible
without being explicitly told to do so. Finally, you should run the TensorFlow-imported Python script with clean console
outputs and should observe significant execution speed-ups. You can also verify this by looking at the Performance tab
of Windows 10's Task Manager by pressing `Ctrl+Alt+Delete`. (Ignore GPU 0 which is an integrated Graphic Card.)

<center>
    <img src="{{ "images/20201228-1.PNG" }}" alt="Performance Tab"/>
    <font face="Lora">Figure 1: Details about GPU 1 in the Performance tab of Task Manager while using TensorFlow's GPU functionalities.</font>
</center>

## Troubleshoot
### Difference Between TensorFlow-GPU, TensorFlow-CPU, and TensorFlow
What are the differences between installing TensorFlow and TensorFlow-GPU, if both support GPU operations in the
presence of GPU hardware, CUDA, and cuDNN? According to a highly upvoted answer (updated for recently released TF
versions) in [stackoverflow](https://stackoverflow.com/questions/52624703/difference-between-installation-libraries-of-tensorflow-gpu-vs-cpu),
the answers can be summarized below. Please note that running the script always work in all combinations, and
the <font face="Lora">yes</font>/<font face="Lora">no</font> in the table means "<font face="Lora">if the package will
work out of the box when executing</font> `import tensorflow as tf`":

```
 Support for TensorFlow libraries for hardware type | tensorflow/tf | tensorflow-gpu/tf-gpu |
 cpu-only                                           |      yes      |     no (~tf-like)     |
 gpu with cuda+cudnn installed                      |      yes      |     yes               |
 gpu without cuda+cudnn installed                   |      yes      |     no (~tf-like)     |
```
<center><font face="Lora">Table 1: How TensorFlow/TensorFlow-GPU behave when installed on different hardware settings.</font></center><br/>

Please note that "~tf-like" means even though the library is TensorFlow-GPU, it would behave like the TensorFlow
library. However, I am not sure about the behaviors when it comes to installing the TensorFlow-CPU library, since it is
no longer recommended. You could have it downloaded and installed by executing `pip install tensorflow-cpu` and see what
will happen. Two more comments in the post are worth mentioning, i.e.
- From TensorFlow version 2.0 onward, these libraries are not separated, and you could simply install TensorFlow to make
use of GPUs, supposing that you have GPU hardware and appropriate CUDA/cuDNN installed.
- Under a conda environment, installation of CUDA/cuDNN will automatically be done using `conda install tensorflow`, but
`pip install tensorflow` won't so that you've got to install them manually.

{% include warning.html content="Regarding the last comment, I have tried installing TensorFlow-GPU on both Windows 10
and servers running Linux, and found that `conda` won't automatically search for and install all required CUDA/cuDNN
packages associated with TensorFlow on Windows 10." %}

## References
[Easy TensorFlow: A Comprehensive Guide Website For TensorFlow Users](https://www.easy-tensorflow.com)

{% include links.html %}
