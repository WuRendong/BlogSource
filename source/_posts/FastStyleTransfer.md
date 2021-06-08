---
title: 体验快速文件风格转换
date: 2020-05-30 17:39:14
tags: [Deep Learning, Tensorflow, CNN, AI, Machine Learning]
top_img: /resource/filetransfer/cnn.png
cover: /resource/filetransfer/cnn.png
categories: 
- [Deep Learning, CNN]
keywords: Deep Learning, Tensorflow, CNN, AI, Machine Learning
---

# 1 概述
对于搞IT的同学来说，技术的更迭一直是很头疼的事情，从原来的PC时代到现在的移动互联网，中间冒出太多东西了，干了这行就必须不断的学习，不然就很容易被行业淘汰了，机器学习是最近几年比较火的一个技术，其中的深度学习应该是最受欢迎的分支了，在市场上已经有大量的应用，大家看到的智能语音助手，智能翻译，智能相机，无人驾驶等都是使用了深度学习的算法。还有笔者之前学习了Udacity的课程，里面的项目有根据一段剧本，经过对模型的训练，可以生成新的剧本，还有根据一批图片比如人脸来训练相关模型，可以生成一个新的人脸。当时看到这些的时候还是觉得好神奇的。那时候就听说了使用神经网络可以大大减少传统编程所需要的时间，让程序员失业，所以心里的焦虑油然而生。

这片文章主要是为了先睹为快，并没有设计太多的技术细节，对于一个新的事物，先知其然再知其所以然，循序渐进。文中涉及的项目叫做**快速风格变换**，就是将输入的照片根据另外一张图片的风格，比如油画的风格，生成同样风格的一张新图片。项目是MIT的一位小哥基于tensorflow做出来的，采用的卷积神经网络（Convolutional Neural Network, 简称CNN），可以在下面的地址找到。

> ​		https://github.com/lengstrom/fast-style-transfer



先来看一下效果，下面有三张图，图一是转换成的图片风格，图二是需要被转换的图片，图三是图二根据图一的风格转换成的新图片。

{% gallery %}
![](/resource/filetransfer/style/udnie.jpg)
![](/resource/filetransfer/sample/stata.jpg)
![](/resource/filetransfer/sample/stata_udnie_header.jpg)
{% endgallery %}



# 2 项目运行说明

## 2.1 先安装Anaconda

   在深度学习中会有很多依赖的包，不同的项目可能需要的包差别也比较大，比如有的项目1需要python 2和支持包A而项目2需要python 3和支持包B，这样当你同时工作在项目1和项目2的时候切换环境就十分必要，另外在工作的环境里，经常也需要对依赖的包进行管理，Anaconda就是一个很好的工具，可以很方便的进行环境和包的管理。

   Anaconda的安装方法可以在[这里](https://docs.anaconda.com/anaconda/install/)找到，最常用的命令如下

  > - 创建新环境
  >   conda create -n <环境名>
  > - 查看已经创建的环境
  >   conda env list 或 conda info \--envs
  > - 安装包
  >   conda install <包名>  也可以使用pip install <包名>
  > - 启动环境
  >   source activate <环境名> 或 conda deactivate <环境名>
  > - 停用环境
  >   source deactivate <环境名> 或 conda deactivate <环境名>
  > - 删除环境
  >   conda remove -n <环境名> \--all  或 conda remove \--name <环境名> \--all
  >   注意：目前如果环境没有安装任何包，执行环境删除会报错，可以先随便安装一个包再删除。



## 2.2 创建虚拟环境

   - 创建环境tf-gpu，其中安装了tensorflow-gpu，如果不支持tensorflow-gpu可以使用tensorflow
     > conda create -n tf-gpu  tensorflow-gpu=2.1.0
   - 激活环境
     > conda activate tf-gpu
   - 打开notebook，在这个项目中我没有用到，jupyterlab跟jupyter notebook类似，都是功能强大的记事本，jupyter notebook是基于web的交互计算环境，用来创建jupyter notebook文档，支持多种语言，如Python，Julia，R等，用来进行数据分析，数据可视化，和进一步的交互性，探索性计算。而Jupyterlab是下一代的用户界面，包含记事本，它包含模块化结构，它可以在同一个窗口以tab的形式打开多个记事本或文件（例如HTML，文本，Markdown等），类似一个集成开发环境。
     > conda intall jupyterlab
     > jupyter lab
   - 安装moviepy，如果只是处理图片应该不需要。
     > pip install moviepy==1.0.2

## 2.3 训练网络

  使用style.py来训练风格转换神经网络，使用python style.py可以看到所有需要的参数，训练网络需要花费很长的时间，如果在本机跑的话机器会疯狂运转，发热很厉害，而且本地机器要求配置也比较高，可以租用类似亚马逊的云服务器来跑。

      用法示例
      python style.py --style path/to/style/img.jpg \
        --checkpoint-dir checkpoint/path \
        --test path/to/test/img.jpg \
        --test-dir path/to/test/dir \
        --content-weight 1.5e1 \
        --checkpoint-iterations 1000 \
        --batch-size 20
        
  其中\--style跟\--checkpoint-dir是必须的，其他的可以选择默认，\--style为用来训练的风格图片，\--checkpoint-dir是输出的模型文件，后面的评估会用到。

      

## 2.4 评估风格转换网络

  使用evaluate.py来评估风格转换网络。运行python evaluate.py可以看到所有的参数。

      用法示例
      python evaluate.py --checkpoint path/to/style/model.ckpt \
        --in-path dir/of/test/imgs/ \
        --out-path dir/for/results/
        
      --checkpoint指定模型，--in-path输入图片路径，--ou t-path输出图片路径

  训练的时间比较长，如果可以访问google drive，可以在[这里](https://drive.google.com/drive/folders/0B9jhaT37ydSyRk9UX0wwX3BpMzQ)找到现成的模型文件，这里也贴上我跑出来的模型文件。



  | 图片 | 模型 |
  |------|-----|
  |<img src="/resource/filetransfer/style/la_muse.jpg" alt="la_muse" style="zoom:25%;" />| [LaMuse](/resource/filetransfer/model/la-muse.ckpt)|
  |<img src="/resource/filetransfer/style/rain_princess.jpg" alt="rain-princess" style="zoom:12%;" />| [RainPrincess](/resource/filetransfer/model/rain-princess.ckpt)|
  |<img src="/resource/filetransfer/style/the_scream.jpg" alt="the_scream" style="zoom:25%;" />| [Scream](/resource/filetransfer/model/scream.ckpt)|
  |<img src="/resource/filetransfer/style/the_shipwreck_of_the_minotaur.jpg" alt="the_shipwreck_of_the_minotaur" style="zoom:15%;" />| [Wreck](/resource/filetransfer/model/wreck.ckpt)|
  |<img src="/resource/filetransfer/style/udnie.jpg" alt="udnie" style="zoom:25%;" />| [Udnie](/resource/filetransfer/model/udnie.ckpt)|
  |<img src="/resource/filetransfer/style/wave.jpg" alt="wave" style="zoom:15%;" />| [Wave](/resource/filetransfer/model/wave.ckpt)|

  下面是我用自己的图片输入产生的输出。

  <img src="/resource/filetransfer/image/input.jpg" alt="input" style="zoom:25%;" />

| LaMuse | RainPrincess | Scream | Wreck | Udnie | Wave |
|----|----|----|----|----|----|
|<img src="/resource/filetransfer/image/Yorkshire-la-muse.jpg" alt="LaMuse" style="zoom:25%;" />|<img src="/resource/filetransfer/image/Yorkshire-rain-princess.jpg" alt="RainPrincess" style="zoom:25%;" />|<img src="/resource/filetransfer/image/Yorkshire-scream.jpg" alt="Scream" style="zoom:25%;" />|<img src="/resource/filetransfer/image/Yorkshire-wreck.jpg" alt="Wreck" style="zoom:25%;" />|<img src="/resource/filetransfer/image/Yorkshire-udnie.jpg" alt="Udnie" style="zoom:25%;" />|<img src="/resource/filetransfer/image/Yorkshire-wave.jpg" alt="Wave" style="zoom:25%;" />|
   
## 2.5 风格化视频

使用transform_video.py来转换视频的风格。运行python transform_video.py可以查看所有参数。要求ffmpeg。

      用法示例
      python transform_video.py --in-path path/to/input/vid.mp4 \
        --checkpoint path/to/style/model.ckpt \
        --out-path out/video.mp4 \
        --device /gpu:0 \
        --batch-size 4
        
      --in-path输入视频，--checkpoint模型文件
      
示例效果

{% dplayer "url=/resource/filetransfer/video/foxsample.mp4"  "pic=/resource/filetransfer/video/foxsample.png" "loop=false" "theme=#FADFA3" "autoplay=false" "token=tokendemo" "screenshot=true" %}

我使用另外一个风格进行处理，效果如下

|  原视频  |  LaMuse  |
|-------|--------|
|{% dplayer "url=/resource/filetransfer/video/fox.mp4"  "pic=/resource/filetransfer/video/fox.png" "loop=false" "theme=#FADFA3" "autoplay=false" "token=tokendemo" "screenshot=true" %}|{% dplayer "url=/resource/filetransfer/video/fox-la-muse.mp4"  "pic=/resource/filetransfer/video/fox-la-muse.png" "loop=false" "theme=#FADFA3" "autoplay=false" "token=tokendemo" "screenshot=true" %}|
   
## 2.6 运行要求
最后说明下关于跑该项目的硬性要求，下面是最低要求。

- TensorFlow 0.11.0 
- Python 2.7.9, Pillow 3.4.2, scipy 0.18.1, numpy 1.11.2
- If you want to train (and don't want to wait for 4 months):
  - A decent GPU
  - All the required NVIDIA software to run TF on a GPU (cuda, etc)
- ffmpeg 3.1.3 if you want to stylize video
