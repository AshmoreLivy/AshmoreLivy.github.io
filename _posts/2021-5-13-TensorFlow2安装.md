---
layout:     post
title:      TensorFlow2安装
subtitle:   梦开始的地方
date:       2021-05-16
author:     Ashmore
header-img: img/post-web.jpg
catalog: true
tags:
    - 炼丹 - TensorFlow
---
{: id="20210516031449-u00i72z" updated="20210516031523"}

# 前言
{: id="20210516031451-dqeirkq" updated="20210516032055"}

本文按照[TensorFlow官网](https://tensorflow.google.cn/install?hl=zh-cn)实现安装。
{: id="20210516032055-hwplfo1" updated="20210516032152"}

本机GPU设备：NVIDIA GeForce GTX 1050 Ti
{: id="20210516032328-cz2helw" updated="20210516032446"}

# 使用 pip 安装 TensorFlow
{: id="20210516032440-0bxq8u8"}

安装后不知道怎么就遇到了pip命令失效的问题，通过以下方式已解决：
{: id="20210516032334-08dc7n5" updated="20210516032838"}

![](https://AshmoreLivy.github.io/BlogImage/pip%E5%A4%B1%E6%95%88.png)
{: id="20210516032838-g1ytjyt" updated="20210516033006"}

<div align="center">pip失效</div>

{: id="20210516033007-a2lftbp" updated="20210516033046"}

# GPU支持
{: id="20210516033137-qnnm31q" updated="20210516034014"}

## **软件要求**
{: id="20210516034044-fvnn9lh"}

必须在系统中安装以下 NVIDIA® 软件：
{: id="20210516034044-vfvzyqf"}

* {: id="20210516034044-axrt6tt"}[NVIDIA® GPU 驱动程序](https://www.nvidia.com/drivers)：CUDA® 11.0 需要 450.x 或更高版本。
  {: id="20210516034044-e68awrq"}
* {: id="20210516034044-xdqpfeb"}[CUDA® 工具包](https://developer.nvidia.com/cuda-toolkit-archive)：TensorFlow 支持 CUDA® 11（TensorFlow 2.4.0 及更高版本）。
  {: id="20210516034044-sj1411b" updated="20210516034128"}
* {: id="20210516034044-dzrzq86"}CUDA® 工具包附带的 [CUPTI](http://docs.nvidia.com/cuda/cupti/)。
  {: id="20210516034044-pzhoyz7"}
* {: id="20210516034044-v6wwq75"}[cuDNN SDK 8.0.4](https://developer.nvidia.com/cudnn) [cuDNN 版本](https://developer.nvidia.com/rdp/cudnn-archive)。
  {: id="20210516034044-0d0kdu7" updated="20210516034427"}
* {: id="20210516034044-zcb6ifx"}（可选）[TensorRT 6.0](https://docs.nvidia.com/deeplearning/sdk/tensorrt-install-guide/index.html)，可缩短用某些模型进行推断的延迟时间并提高吞吐量。
  {: id="20210516034044-0bfqihq"}
{: id="20210516034044-s6s3qgu"}

查看CUDA的编译器`nvcc -V`**要大写**：
{: id="20210516034044-m226ub6" updated="20210516034255"}

![](https://AshmoreLivy.github.io/BlogImage/nvcc.png)
{: id="20210516034256-kz4dvj8" updated="20210516034325"}

若cuDNN SDK未安装好，会出现如下问题：
{: id="20210516034327-svf5zeh" updated="20210516034518"}

![](https://AshmoreLivy.github.io/BlogImage/%E6%97%A0%E6%B3%95%E6%89%93%E5%BC%80GPU.png)
{: id="20210516034519-bnxnsbw" updated="20210516034529"}

注意这里有个cusolver64_10.dll的问题，因为包里边是11的版本所以没找到。改成10或者去把10下载下来放包里都可以。cuDNN SDK下载和配置的过程我本人吃了鳖，参考[Win10安装配置CUDA和cuDNN实现显卡GPU加速](https://www.pianshen.com/article/14761947171/)吧乌鱼子。
{: id="20210516034532-316g1hy" updated="20210516034954"}

完成好之后就会是这样：
{: id="20210516035004-b1wfqch" updated="20210516035043"}

![](https://AshmoreLivy.github.io/BlogImage/XLA%E5%A4%B1%E8%B4%A5.png)
{: id="20210516035106-rtx7d8k" updated="20210516035118"}

看到了`This TensorFlow binary is optimised with oneAPI Deep Neural Libtary(oneDNN)`，以及一堆Successfully opened，以及`Adding visible gpu devices：0`，说明gpu可用了，但还有个XLA devices的问题，应该是这个[XLA：优化机器学习编译器](https://tensorflow.google.cn/xla)。
{: id="20210516035046-kh7t1on" updated="20210516035755"}

我应该是去把tf-nightly重新下载了一遍，以及把又把tensorflow-gpu重新下载了一遍，最后没有这个问题了，但是那些Successfully opened莫名消失了，很苦逼。但是最后运行起来都成功了如下，就暂时不管那么多了：
{: id="20210516035719-w38yqs6" updated="20210516040106"}

![](https://AshmoreLivy.github.io/BlogImage/GPU%E6%88%90%E5%8A%9F.png)
{: id="20210516035508-vkwnibs" updated="20210516040143"}


{: id="20210516031424-nrdicst" type="doc"}
