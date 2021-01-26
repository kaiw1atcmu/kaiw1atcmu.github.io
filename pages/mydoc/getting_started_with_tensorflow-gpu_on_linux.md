---
title: "Getting Started With TensorFlow-gpu On Linux"
keywords: tensorflow, gpu
date: 2021-01-25 21:40:22 -0800
last_updated: January 25, 2021
tags: [deep_learning]
summary: "TensorFlow has been a powerful yet subtle deep learning framework for the research community and the industry
alike. Today let me devote this post to getting readers started with TensorFlow's GPU packages on Linux, from
essentially the very beginning."
sidebar: none
permalink: getting_started_with_tensorflow-gpu_on_linux.html
folder: mydoc
---

TensorFlow has been a powerful yet subtle deep learning framework for the research community and the industry alike.
Today let me devote this post to getting readers started with TensorFlow's GPU packages, from essentially the very
beginning. Throughout our discussion, I will showcase all installation procedures
for <font face="Lora">Ubuntu-18.04</font>, a typical <font face="Lora">Unix</font>/<font face="Lora">Linux</font>-based
OS.

## Our Operating System And Other Settings
Our institutional server has Ubuntu-18.04 LTS installed. For NVIDIA GPUs, The machine's important GPU specs are found by
running the console tool `lspci` in Ubuntu's terminal (immaterial specs are omitted as `...` for simplicity):

```
...
17:00.0 VGA compatible controller: NVIDIA Corporation GV104 [GeForce GTX 1180] (rev a1)
...
65:00.0 VGA compatible controller: NVIDIA Corporation GV104 [GeForce GTX 1180] (rev a1)
...
```

## Install Pycharm (Optional And Not Recommended)
Many believe <font face="Lora">PyCharm</font> is one of the best (if not the best) Integrated Development Environments
(IDEs) for Python programming. It comes with powerful tools for code editing, navigating, refactoring, debugging, etc.
The <font face="Lora">community version</font> of this software is free and you can download it through
[its official website](https://www.jetbrains.com/pycharm/download). However, we do not recommend installing PyCharm on
Ubuntu since we're using it only as a remote server, and debugging from an integrated development environment offers no
further convenience.

## Install Python & Conda
&#000;<font face="Lora">Conda</font> is an open-source package management system and environment management system that
runs on multiple OSes. It was created for Python programs but it can package and distribute software for nearly any
language (e.g. C++/Java), with a focus on data sciences. Conda manager gives you the ability to create multiple
environments with different versions of Python and other libraries. This becomes useful when some codes are written with
specific versions of a library. The Conda package and environment manager is included in all versions
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
preferences, I suggest using `pip` since it provides users with more flexibility.

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
you don't need to worry about this if your machine is running Ubuntu since its corresponding hardware drivers are
packaged into the CUDA Toolkit installation files.

If you want to use the GPU version of TensorFlow you must have a CUDA-enabled GPU. (Our institutional server has a pair
of <font face="Lora">NVIDIA GeForce GTX 2080 Graphic Card</font>, which are CUDA-enabled GPUs.) Then you need to install
<font face="Lora">CUDA</font> and <font face="Lora">cuDNN</font> with their appropriate versions.

## Install Appropriate CUDA Version
The version numbers should be determined by the TensorFlow version you have installed. For example, after running a
TensorFlow-imported Python script, our institutional server generated the following console output, explicitly
specifying that the required .dll version was `cudart64_101.dll`, and hence the required CUDA version was 10.1
(Interesting enough, `tensorflow-2.4.0` would instead require CUDA version `11.0` and `cudart64_110.dll`.):

```
2021-01-25 17:17:53.730911: W tensorflow/stream_executor/platform/default/dso_loader.cc:59] Could not load dynamic library 'libcudart.so.10.1'; dlerror: libcudart.so.10.1: cannot open shared object file: No such file or directory
2021-01-25 17:17:53.730953: I tensorflow/stream_executor/cuda/cudart_stub.cc:29] Ignore above cudart dlerror if you do not have a GPU set up on your machine.
```

Next, find CUDA 10.1 in its appropriate format, depending on your machine's operating system (Windows/Ubuntu/...),
architecture (x86_64/ppc/...), OS version (Windows 10/Windows Server 2019/...), etc. You might choose installer types
between "network" (recommended when your have low-bandwidth Internet connection) and "local" (recommended when your have
access to high-bandwidth Internet connection) installer types. You could start by choosing the `runfile (local)` option.
After downloading the runfile (local) installer, run the command `sudo sh cuda_10.1.105_418.39_linux.run` and follow the
command-line prompts. Type "accept" at the end of the line to continue.

<center>
    <img src="{{ "images/20210125-1.PNG" }}" alt="Prompt To Accept End User License Agreement"/>
    <font face="Lora">Figure 1: Prompt To Accept End User License Agreement.</font>
</center>

You need only to install the CUDA Toolkit 10.1 if you already have its Driver installed. In this case, simply check the
box preceding CUDA Toolkit 10.1 and leave other boxes unchecked.

<center>
    <img src="{{ "images/20210125-2.PNG" }}" alt="Prompt To Run Installer"/>
    <font face="Lora">Figure 2: Prompt To Run Installer.</font>
</center>

If CUDA Toolkit 10.1 is already installed, you could proceed by upgrading the existing installation.

<center>
    <img src="{{ "images/20210125-3.PNG" }}" alt="Prompt To Proceed With Existing Installation"/>
    <font face="Lora">Figure 3: Prompt To Proceed With Existing Installation.</font>
</center>

Before finishing up, you're asked to update the cuda symbolic link so that it points to the CUDA Toolkit version you
just installed, or ignore the update.

<center>
    <img src="{{ "images/20210125-4.PNG" }}" alt="Prompt To Update Symbolic Link"/>
    <font face="Lora">Figure 4: Prompt To Update Symbolic Link.</font>
</center>

After installation, you will probably see system output.

```
Driver:   Not Selected
Toolkit:  Installed in /usr/local/cuda-10.1/
Samples:  Not Selected

Please make sure that
 -   PATH includes /usr/local/cuda-10.1/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-10.1/lib64, or, add /usr/local/cuda-10.1/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-10.1/bin

Please see CUDA_Installation_Guide_Linux.pdf in /usr/local/cuda-10.1/doc/pdf for detailed information on setting up CUDA.
***WARNING: Incomplete installation! This installation did not install the CUDA Driver. A driver of version at least 418.00 is required for CUDA 10.1 functionality to work.
To install the driver using this installer, run the following command, replacing <CudaInstaller> with the name of this run file:
    sudo <CudaInstaller>.run --silent --driver

Logfile is /var/log/cuda-installer.log
```

## Install Appropriate cuDNN Version
Next, you should find the appropriate cuDNN version depending on your CUDA version, and find the appropriate choice of
that cuDNN version according to your machine's operating system, architecture, OS version, etc. as before. For CUDA
10.1, the newest appropriate cuDNN version is `8.0.4` (as of Jan. 2021). The downloaded cuDNN file should be in a format
called solitairetheme8. Convert this .solitairetheme8 file to an ordinary .tgz file by running
`cp cudnn-10.1-linux-x64-v8.0.4.30.solitairetheme8 cudnn-10.1-linux-x64-v8.0.4.30.tgz`, and decompress the .tgz file to
a folder called `cuda`by default by running `tar -xvzf cudnn-10.1-linux-x64-v8.0.4.30.tgz`.

Next, you should find out where your CUDA installation path reside in Ubuntu's filesystem hierarchy. Normally, your
CUDA path should be `/usr/...` or `/usr/local/cuda/` or `/usr/local/cuda/cuda-10.1/`, etc. For our institutional server,
it is `/usr/local/cuda-10.1/`. Then merge the untarred cuda folder into the CUDA installation folder. That is, copy and
paste files in the untarred cuda folder into the same hierarchical places in the CUDA installation folder.

## Maybe You'll Need To Download Archive cuDNN Versions Together
After that, rerun the TensorFlow-imported Python script and see if error messages still appear. If so, search Google for
the missing .dll file, which may be contained in an archive version of cuDNN. A little bit strange, though. For example,
my laptop output a different error message at this moment:

```
2021-01-25 17:35:52.035282: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcudart.so.10.1
2021-01-25 17:35:52.786907: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcuda.so.1
2021-01-25 17:35:54.092861: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1716] Found device 0 with properties: 
pciBusID: 0000:17:00.0 name: GeForce RTX 2080 computeCapability: 7.5
coreClock: 1.71GHz coreCount: 46 deviceMemorySize: 7.79GiB deviceMemoryBandwidth: 417.23GiB/s
2021-01-25 17:35:54.093257: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1716] Found device 1 with properties: 
pciBusID: 0000:65:00.0 name: GeForce RTX 2080 computeCapability: 7.5
coreClock: 1.71GHz coreCount: 46 deviceMemorySize: 7.79GiB deviceMemoryBandwidth: 417.23GiB/s
2021-01-25 17:35:54.093278: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcudart.so.10.1
2021-01-25 17:35:54.094454: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcublas.so.10
2021-01-25 17:35:54.095313: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcufft.so.10
2021-01-25 17:35:54.095536: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcurand.so.10
2021-01-25 17:35:54.096642: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcusolver.so.10
2021-01-25 17:35:54.097472: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcusparse.so.10
2021-01-25 17:35:54.097606: W tensorflow/stream_executor/platform/default/dso_loader.cc:59] Could not load dynamic library 'libcudnn.so.7'; dlerror: libcudnn.so.7: cannot open shared object file: No such file or directory
2021-01-25 17:35:54.097612: W tensorflow/core/common_runtime/gpu/gpu_device.cc:1753] Cannot dlopen some GPU libraries. Please make sure the missing libraries mentioned above are installed properly if you would like to use GPU. Follow the guide at https://www.tensorflow.org/install/gpu for how to download and setup the required libraries for your platform.
Skipping registering GPU devices...
```

By searching Google, you probably will find out that `libcudnn.so.7` is included in cuDNN version `7.6.4`, for example.
After placing this .so.7 into folder `lib64` of the CUDA installation folder, a third try suggested that all required
files concerning our GPUs should be in place:

```
2021-01-26 12:30:13.414261: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcudart.so.10.1
2021-01-26 12:30:14.691369: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcuda.so.1
2021-01-26 12:30:14.695558: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1716] Found device 0 with properties:
pciBusID: 0000:17:00.0 name: GeForce RTX 2080 computeCapability: 7.5
coreClock: 1.71GHz coreCount: 46 deviceMemorySize: 7.79GiB deviceMemoryBandwidth: 417.23GiB/s
2021-01-26 12:30:14.695921: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1716] Found device 1 with properties:
pciBusID: 0000:65:00.0 name: GeForce RTX 2080 computeCapability: 7.5
coreClock: 1.71GHz coreCount: 46 deviceMemorySize: 7.79GiB deviceMemoryBandwidth: 417.23GiB/s
2021-01-26 12:30:14.695948: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcudart.so.10.1
2021-01-26 12:30:14.697132: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcublas.so.10
2021-01-26 12:30:14.698086: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcufft.so.10
2021-01-26 12:30:14.698323: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcurand.so.10
2021-01-26 12:30:14.699483: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcusolver.so.10
2021-01-26 12:30:14.700406: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcusparse.so.10
2021-01-26 12:30:14.702946: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcudnn.so.7
2021-01-26 12:30:14.704251: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1858] Adding visible gpu devices: 0, 1
2021-01-26 12:30:14.708097: I tensorflow/core/platform/cpu_feature_guard.cc:142] This TensorFlow binary is optimized with oneAPI Deep Neural Network Library (oneDNN)to use the following CPU instructions in performance-critical operations:  AVX2 AVX512F FMA
To enable them in other operations, rebuild TensorFlow with the appropriate compiler flags.
2021-01-26 12:30:14.729698: I tensorflow/core/platform/profile_utils/cpu_utils.cc:104] CPU Frequency: 3699850000 Hz
2021-01-26 12:30:14.730796: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x65d63a0 initialized for platform Host (this does not guarantee that XLA will be used). Devices:
2021-01-26 12:30:14.730818: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (0): Host, Default Version
2021-01-26 12:30:14.956527: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x65237a0 initialized for platform CUDA (this does not guarantee that XLA will be used). Devices:
2021-01-26 12:30:14.956557: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (0): GeForce RTX 2080, Compute Capability 7.5
2021-01-26 12:30:14.956563: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (1): GeForce RTX 2080, Compute Capability 7.5
2021-01-26 12:30:14.957134: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1716] Found device 0 with properties:
pciBusID: 0000:17:00.0 name: GeForce RTX 2080 computeCapability: 7.5
coreClock: 1.71GHz coreCount: 46 deviceMemorySize: 7.79GiB deviceMemoryBandwidth: 417.23GiB/s
2021-01-26 12:30:14.957523: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1716] Found device 1 with properties:
pciBusID: 0000:65:00.0 name: GeForce RTX 2080 computeCapability: 7.5
coreClock: 1.71GHz coreCount: 46 deviceMemorySize: 7.79GiB deviceMemoryBandwidth: 417.23GiB/s
2021-01-26 12:30:14.957588: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcudart.so.10.1
2021-01-26 12:30:14.957624: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcublas.so.10
2021-01-26 12:30:14.957646: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcufft.so.10
2021-01-26 12:30:14.957666: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcurand.so.10
2021-01-26 12:30:14.957686: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcusolver.so.10
2021-01-26 12:30:14.957706: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcusparse.so.10
2021-01-26 12:30:14.957726: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcudnn.so.7
2021-01-26 12:30:14.959309: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1858] Adding visible gpu devices: 0, 1
2021-01-26 12:30:14.959359: I tensorflow/stream_executor/platform/default/dso_loader.cc:48] Successfully opened dynamic library libcudart.so.10.1
2021-01-26 12:30:16.405871: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1257] Device interconnect StreamExecutor with strength 1 edge matrix:
2021-01-26 12:30:16.405906: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1263]      0 1
2021-01-26 12:30:16.405912: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1276] 0:   N N
2021-01-26 12:30:16.405918: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1276] 1:   N N
2021-01-26 12:30:16.407102: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1402] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 4721 MB memory) -> physical GPU (device: 0, name: GeForce RTX 2080, pci bus id: 0000:17:00.0, compute capability: 7.5)
2021-01-26 12:30:16.407899: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1402] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:1 with 4719 MB memory) -> physical GPU (device: 1, name: GeForce RTX 2080, pci bus id: 0000:65:00.0, compute capability: 7.5)
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
Num GPUs Available:  2
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
outputs and should observe significant execution speed-ups. You can also verify this by running the NVIDIA tool
`nvidia-smi` in Ubuntu's terminal:

```shell script
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.33.01    Driver Version: 440.33.01    CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce RTX 2080    Off  | 00000000:17:00.0 Off |                  N/A |
| 55%   83C    P2   202W / 215W |   2704MiB /  7982MiB |     84%      Default |
+-------------------------------+----------------------+----------------------+
|   1  GeForce RTX 2080    Off  | 00000000:65:00.0 Off |                  N/A |
| 63%   85C    P2   202W / 215W |   2704MiB /  7979MiB |     88%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      9581      C   python                                      2693MiB |
|    1      9581      C   python                                      2693MiB |
+-----------------------------------------------------------------------------+
```

## Troubleshoot

## References
[A Comprehensive Guide Website For TensorFlow Veterans And Novices Alike](https://www.easy-tensorflow.com)

{% include links.html %}