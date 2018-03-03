---
layout: post
title: 部署fast-style-transfer实现图像绘画风格的自动迁移
---

春天到啦，阳光明媚，拿起相机出去拍了几张花儿的图片。家里有个老筒子也在学摄影，天天念叨“画意摄影”，用Photoshop折腾不亦乐乎。现在都是人工智能时代咯，能不能自动实现画意摄影的风格迁移呢？于是我就开始了折腾之旅。 作为图书馆员的我，首先想到的是把图书馆的照片弄成画意风，晕。

## 图片风格迁移实现的途径

图片风格迁移的实现主要得益于人工神经网络，也就是深度学习技术的普及，以及计算机GPU运算能力的日益强大。利用特定的特征工程 (feature engineering) 方法，让神经网络学会某张绘画作品画风的特征，然后再迁移到另外一张图片上。想对原理深究的筒子可以查看下面两篇论文，数学不好的我就不看了，哈哈：

* [Perceptual Losses for Real-Time Style Transfer and Super-Resolution](http://cs.stanford.edu/people/jcjohns/eccv16/) 
* [Instance Normalization](https://arxiv.org/abs/1607.08022) 

GitHub上面现成的绘画风格迁移Repo一大堆，有些是用Keras框架实现的。本以为Keras能解决掉底层框架版本的兼容性问题。试了之后才发现，好些基于Keras的框架都只能对尺寸很小的图片进行处理。还是只能回归到直接利用底层框架实现的Repo才行。又因为PyTorch目前只支持Linux和Mac OS X，Theano也都停止维护了，最后证明还是TensorFlow靠谱。

最终我用Repo的是[lengstrom的fast-style-transfer](https://github.com/lengstrom/fast-style-transfer)。本文也将介绍如何在Windows 10下面部署这个应用，实施绘画风格的迁移。

###  硬件需求和注意事项

实施卷积神经网络的应用，对计算机性能较高。推荐使用英伟达GeForce GTX1060或性能更好的GPU，以及Intel i5 四核或更好的处理器。

**注意：请使用散热较好的台式机，而不要使用笔记本电脑来进行实施，以免因散热问题引起计算机损坏。**

### 安装并配置Python

首先需要部署基于Python的科学运算环境。在Windows下面部署这样的环境，最简便的方法便是使用现成的[Python的Anaconda发布](https://www.anaconda.com/download/)了。进入网页，下载安装包到本地，并安装到Windows。下载页面标注的Python的具体版本并无所谓，因为我们后面会专门为图像风格迁移创建专门的虚拟环境。

安装好之后，首先从开始菜单，进入Anaconda Prompt。使用下面的命令创建名为style-transfer的虚拟环境。

```bash
conda create --name=style-transfer python=3.5
```

接下来，安装基础Anaconda配置中的Python库，里面包含了Numpy等常用的计算库，和Jupyter Notebook等常用的工具（虽然这次用不上Jupyter Notebook）。

```bash
activate style-transfer
conda install anaconda
```

网速可能较慢，多等一阵子就好了。

### 安装TensorFlow

fast-style-transfer本身使用的是1.0.0版本的TensorFlow。但是1.0.0版的TensorFlow用到的CUDA版本和英伟达显卡驱动版本也比较陈旧。经过试验，可以使用目前最新的1.5版的TensorFlow，只需要对原始代码做出一处修改即可。

在安装TensorFlow之前，需要安装CUDA等基础程序。先搜索并安装微软的Visual Studio Community 2015，全部用默认设置就好了。然后从英伟达的网站下载CUDA® Toolkit 9.0，以及配套的cuDNN v7.0。英伟达的网站会要求注册，并同意用户协议才能免费下载。请注意，CUDA和cuDNN的版本非常重要，不能随意更换。

CUDA Toolkit是使用图形界面安装，并包含了配套的显卡驱动。安装好之后，再解压缩cuDNN，并复制到CUDA Toolkit的安装目录中。

接下来，使用下面的命令来安装1.5.0版的TensorFlow：

```bash
activate style-transfer
pip install --ignore-installed --upgrade tensorflow-gpu==1.5.0
```

## 安装并运行fast-style-transfer

## 安装并配置fast-style-transfer

首先需要从[lengstrom的fast-style-transfer Repo](https://github.com/lengstrom/fast-style-transfer)下载整个Repo，并解压缩。

分别下载下面两个文档，它们是训练fast-style-transfer模型过程中需要用到的。

* [imagenet-vgg-verydeep-19.mat](http://www.vlfeat.org/matconvnet/models/beta16/) 
* [MSCOCO train2014 DB](http://msvocds.blob.core.windows.net/coco2014/train2014.zip) 

下载后解压缩到fast-style-transfer目录的data子目录中。

下面分为两个步骤：第一步，训练神经网络，学会目标图片的风格，生成并保存下模型文件。第二部，用生成的模型，迁移绘画风格。

## 模型训练

使用了下面这水墨画风格的图片来训练模型：

![一幅国画](https://scanthony.github.io/images/target-style.jpg)

使用一下命令来开始模型训练：

```bash
cd /file/folder/of/fast-style-transfer
python style.py --style "./img/target-style.jpg" --checkpoint-dir "./mychk" --content-weight 1.5e1 --checkpoint-iterations 2000 --batch-size 9
```

其中的`--style`参数是目标图片的存储位置，`--checkpoint-dir`参数是训练好的模型的存储位置。`--batch-size`参数是每个批次并行训练的数量，这个参数设置的越大，整体耗时越短，但是设置太大了会引起显存溢出。在8GB显存的英伟达 GTX 1070 显卡上，设置了大小为9的batch size，经试用能够正常运行。

训练在我的电脑上花费了大概三四个钟头（没有太仔细地看表计时），最后会得到一个`fns.ckpt`文档，里面存储了模型学习到的绘画风格。

## 完成风格迁移

这一步很快当，用GPU实现的话不到10秒钟。使用如下的命令来完成风格迁移：

```bash
cd /file/folder/of/fast-style-transfer
python evaluate.py --checkpoint "./mychk/fns.ckpt" --in-path "./img/orinigal-image.jpg" --out-path "./img/processed-image.jpg"
```

下面列出几幅图片供大家观看吧。

滨海新区图书馆，被誉为国内最美的图书馆，没有“之一”。但是水墨画风格迁移的效果并不理想：

![原图：滨海新区图书馆](https://scanthony.github.io/images/Tianjin-Binhai-Library.jpg) ![国画风：滨海新区图书馆](https://scanthony.github.io/images/Tianjin-Binhai-Library-styled.jpg)

国画果然还是适合花花草草，来试试自己拍摄的一张照片，效果好多了：

![原图：樱花](https://scanthony.github.io/images/sakura.jpg) ![国画风：樱花](https://scanthony.github.io/images/sakura-styled.jpg)

好了，就写到这里。麻麻再也不用担心我用Photoshop加工照片伤眼睛啦~