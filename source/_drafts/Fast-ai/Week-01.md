---
abbrlink: '0'
---
###1. 深度学习的三个组件：
- infinitely flexible function
这个function指的就是神经网络，[universal approximation theorem](https://en.wikipedia.org/wiki/Universal_approximation_theorem)断言几乎所有问题都可以通过有限个神经元组成的神经网络模型来近似。
- all-purpose parameter fitting
利用Gradient Descent和Back Propagation等方法可以得到神经网络function的参数，结合这些参数和function就可以完成各种各样的任务。
- fast and scalable
硬件(GPU)的出现使得实时处理大量数据成为可能

###2. kaggle竞赛中排名
- OKish = top 50%
- Very good = top 20%
- Expert = top 10%

###3. 课程相关文件路径
- 最新的路径www.file.fast.ai
- 老版本的课程文件中提供的路径为www.platform.ai，可能导致出错
- 第一课需要下载vgg16.h5文件，可手动下载到~/.keras/models/文件夹中

###4.编程模块
- cuDNN → CUDA → Theano/TensorFlow → Keras → vgg16
- Tensorflow主要用于Mutiple GPU
- ~/.keras/keras.json → 修改Backend（Theano或TensorFlow）
- ~/.theanorc → 修改CPU或GPU

###5. 一些概念
- batch_size指的是每次迭代所用到的样本数([参考](https://www.zhihu.com/question/32673260))
- epoch指的是所有样本循环次数，每个epoch需要用到迭代次数=总样本数/batch_size。随着epoch数的增加，预测函数从under_fitting走向over_fitting，即epoch并非越大越好。

###6. 错误
- 在Windows10上运行lesson 1中`vgg = vgg16()`出现`'module' object has no attribute 'ifelse'`的错误
参考[解决办法](http://forums.fast.ai/t/getting-error-when-try-to-instanciate-vgg16/427/16)：在C:\Fastai\anaconda3-4.4.0\envs\dlwin27\Lib\site-packages\keras\backend\theano_backend.py的第1行之后加上`from theano.ifelse import ifelse`并删除对应的theano_backend.pyc文件

###待确认问题
- AWS的免费T2.micro是否仍然有效？
