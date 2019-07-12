---
title: Fast.ai环境配置指南(Windows10篇)
date: 2018/01/20
categories:
  - 技能备忘
tags:
  - Fast.ai
  - 深度学习
  - 环境搭建
  - 编程
  - Windows10
abbrlink: b0983528
---

文 | Leon Shia

> 因很多人推荐去年推出的Fast.ai系列深度学习课程，决定上手学习。本指南记录了我在Windows10上搭建深度学习环境的详细过程以及踩过的坑(跑过了Lesson1.ipynb)，有需要的可以参考。本指南主要参考[https://github.com/philferriere/dlwin](https://github.com/philferriere/dlwin)，有一些改动。另外，课程的Forum [http://forums.fast.ai/](http://forums.fast.ai/)也是很不错的资源。

<!--more-->

# 安装VS2015
Visual Studio 2015 Community Edition Update 3安装过程如下。最好解压后安装，否则中间可能会出现“安装包丢失或损坏”，解决办法是将安装文件解压出来的package文件夹复制到丢失的位置，然后点击重试。添加以下系统环境变量：
- 在`PATH`中添加`C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin`
- 在`INCLUDE`中添加`C:\Program Files (x86)\Windows Kits\10\Include`
- 在`LIB`中添加`C:\Program Files (x86)\Windows Kits\10\Lib\10.0.10240.0\um\x64`和`C:\Program Files (x86)\Windows Kits\10\Lib\10.0.10240.0\ucrt\x64`

{% asset_img Install_VS2015_Step-1.png %}
{% asset_img Install_VS2015_Step-2.png %}
{% asset_img Install_VS2015_Step-3.png %}

# 安装Anaconda
官网下载并安装Anaconda 4.4.0或最新版，我安装在C:\Fastai\anaconda3-4.4.0文件夹中，文件夹的位置和名字可以不一样，添加以下系统环境变量(注意替换C:\Fastai为你的安装位置)：
- 创建系统环境变量`PYTHON_HOME`并添加`C:\Fastai\anaconda3-4.4.0`
- 在`PATH`中添加`%PYTHON_HOME%`，`%PYTHON_HOME%\Scripts`，`%PYTHON_HOME%\Library\bin`

{% asset_img Install_Anaconda_4.4.0.jpg %}

# 创建深度学习环境dlwin27
这个过程会比较长，注意`python = 2.7`字段，Fast.ai课程推荐用2.7版本的python。即使上面安装的是Anaconda3.6，这里也可以创建2.7的环境，而且这个环境是独立，即便后面安装有问题也可以直接用`conda env remove -n dlwin27`删去重新再来，而不用担心卸载不完全的问题，这就是使用Anaconda的优点。
```Shell
conda create --yes -n dlwin27 python=2.7  numpy scipy mkl-service m2w64-toolchain libpython jupyter
```

# 安装CUDA 8.0.61
从NVidia官网[https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)下载CUDA 8.0 (64-bit) ，我安装在C:\Fastai\cuda-8.0.61文件夹中，添加以下系统环境变量(注意替换C:\Fastai为你的安装位置)：
- 创建系统环境变量`CUDA_PATH`并添加`C:\Fastai\cuda-8.0.61`
- 在`PATH`中添加`%CUDA_PATH%\bin`和`%CUDA_PATH%\libnvvp`

{% asset_img Install_CUDA_8.0.61_Step-1.png %}
{% asset_img Install_CUDA_8.0.61_Step-2.png %}
{% asset_img Install_CUDA_8.0.61_Step-3.png %}
{% asset_img Install_CUDA_8.0.61_Step-4.png %}

# 安装cuDNN v5.1 for CUDA 8.0
在官网[https://developer.nvidia.com/rdp/cudnn-download](https://developer.nvidia.com/rdp/cudnn-download)下载cuDNN 5.1，将解压出来的三个文件夹 (bin, include, lib)中的文件复制到C:\Fastai\cuda-8.0.61对应同名文件夹中。并执行以下命令(会显示移动了5个文件)：
```Shell
cd C:\Fastai\anaconda3-4.4.0\envs\dlwin27
md discard & move cu*.dll discard
```
{% asset_img Install_cuDNN_5.1_for_CUDA_8.0.jpg %}

# 安装必要的python包
- 在主文件夹下(一般为C:\Users\YOUR NAME\)下的`.condarc`文件中添加以下内容，方便后面下载安装文件，注意该文件为**隐藏文件**：
```
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults
```
- 执行`activate dlwin27`激活环境，再安装python包时才能安装到该环境中，以后每次使用该环境时都要执行这一句，执行完后命令行前面会出现(dlwin27)如下，退出时执行`deactivate`即可。

{% asset_img Execute_activate_dlwin27.jpg %}

- 有时没有conda对应的包就用pip安装，注意前面三个包的版本号，最后几个包是运行Lesson1.ipynb时提示要安装的。
```Shell
conda install -y pygpu==0.7.5
conda install -y theano==1.0.1
pip install keras==1.2.2
pip install matplotlib
pip install pandas
conda install -y PIL
pip install sklearn
conda install -y bcolz
conda install -c menpo ffmpeg
conda install -y h5py
```
- 在主文件夹下(一般为C:\Users\YOUR NAME\)下的`\.keras\keras.json`文件中的内容应改为和下面一致，如果原文件中有`"image_data_format": "channels_last"`这一条，可能是因为安装了较高版本的keras。
```
{
    "floatx": "float32",
    "epsilon": 1e-07,
    "backend": "theano",
    "image_dim_ordering": "th"
}
```
- 创建系统环境变量`THEANO_FLAGS_CPU`并添加`floatX=float32,device=cpu`
- 创建系统环境变量`THEANO_FLAGS_GPU`并添加
```
floatX=float32,device=cuda0,dnn.enabled=False,gpuarray.preallocate=0.8
```
- (注意替换C:\Fastai为你的安装位置)创建系统环境变量`THEANO_FLAGS_GPU_DNN`并添加
```
floatX=float32,device=cuda0,optimizer_including=cudnn,gpuarray.preallocate=0.8,dnn.conv.algo_bwd_filter=deterministic,dnn.conv.algo_bwd_data=deterministic,dnn.include_path=C:/Fastai/cuda-8.0.61/include,dnn.library_path=C:/Fastai/cuda-8.0.61/lib/x64
```

# 测试theano
将以下内容保存为cpu_gpu_test.py放在工作路径下(命令行前面会显示)
```python
from theano import function, config, shared, tensor
import numpy
import time

vlen = 10 * 30 * 768  # 10 x #cores x # threads per core
iters = 1000

rng = numpy.random.RandomState(22)
x = shared(numpy.asarray(rng.rand(vlen), config.floatX))
f = function([], tensor.exp(x))
print(f.maker.fgraph.toposort())
t0 = time.time()
for i in range(iters):
    r = f()
t1 = time.time()
print("Looping %d times took %f seconds" % (iters, t1 - t0))
print("Result is %s" % (r,))
if numpy.any([isinstance(x.op, tensor.Elemwise) and
              ('Gpu' not in type(x.op).__name__)
              for x in f.maker.fgraph.toposort()]):
    print('Used the cpu')
else:
    print('Used the gpu')
```
执行以下命令可查看前面设置的theano flag。(以后安装TensorFlow学习时也可将KERAS_BACKEND设置为tensorflow)
```Shell
set KERAS_BACKEND=theano
set | findstr /i theano
```
执行以下三个命令中的一个，即可在CPU、GPU、GPU with cuDNN三种模式之间切换，其中GPU with cuDNN模式要求GPU的计算能力达到3.0，如果低于3.0则会自动屏蔽cuDNN加速，可以在官网查到。(https://developer.nvidia.com/cuda-gpus)
```Shell
set THEANO_FLAGS=%THEANO_FLAGS_CPU%
set THEANO_FLAGS=%THEANO_FLAGS_GPU%
set THEANO_FLAGS=%THEANO_FLAGS_GPU_DNN%
```
执行`python cpu_gpu_test.py`可查看上面测试文件的测试结果，其中CPU模式下的时间为16s左右，GPU和GPU with cuDNN模式下的时间都在1s以内。

# 安装Cygwin(可选)
- 官网下载Cygwin安装文件，安装时一起安装wget、tmux、dos2unix、git、vim、openssh，即search中输入安装包名，点击安装包后的skip直到变成版本号。(keep是保留已安装的包，uninstall是卸载已安装的包)
- 将地址[git-completion](https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash)的代码保存为`C:\Fastai\cygwin64\home\YOUR NAME\.git-completion.bash`，并在该目录下的`.bashrc`文件中加入`source ~/.git-completion.bash`
- 在`C:\Fastai\cygwin64\home\YOUR NAME\.bashrc`加入`cd f:\workspace`这样开启Cygwin时当前目录就是f:\workspace
注意在Cygwin命令行跳到同样位置要用`cd \cygdrive\f\workspace`
- 去掉`C:\Fastai\cygwin64\home\YOUR NAME\.inputrc`文件中`set completion-ignore-case on`行前面的注释符号#
- 其它一些设置
```git
git config --system core.fileMode false
git config --system core.quotepath false
git config --system color.ui true

git config --global user.name "webstiffener"
git config --global user.email web.stiffener@foxmail.com

git config --system alias.st status
git config --system alias.ci "commit -s"
git config --system alias.co checkout
git config --system alias.br branch
git config --system alias.ll "log --pretty=fuller --stat --graph --decorate"
git config --system alias.ls "log --pretty=oneline --graph --decorate"
git config --system alias.ss "status -sb"

```






