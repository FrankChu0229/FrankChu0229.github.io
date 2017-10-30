title: TF Symposium 1.4.0 Summary
date: 2017-10-25 16:15:23
tags: [tensorflow, notes, summary]
categories: [tensorflow, notes]
description: TF Symposium Summary
---

# TF Symposium Summary

## L2L with 进化算法 by YiFei Feng

- 循环网络： 控制器； 
- evolution algo： 淘汰 -> 变异
- 用简单的模型初始化
- 重复进化步骤

## TF 与 生物学

- 生物大数据
- 机器学习问题： regression，classification
- Deep Variant: inception-v3,将在第四季或明年开源

## TF 性能 by Frank Chen

- 出现性能问题： 了解问题， 用分析器：
- Timeline TF自带分析器, 用chrome打开profile文件， 查看瓶颈
- 复制内存：复制越多， tf运算图越慢
- 如何改进： 1. 优化输入管道， 使用流水线； 2. 优化模型计算， 使用fuzed batch-norm, 使得在GPU上更快；3. 数据格式， CPU，GPU数据格式； 4. 运算图， matcol； 5. 单GPU变量：变量放到一个GPU即可； 单机多GPU， 可以尝试使用GPU来作为变量ser ver；变量过大或者过多， 多GPU共享变量； 多机多GPU， 通过网络赋值内存，比单机慢；可以使用local CPU做cache； 高级分布式培训模式：在每一个GPU集群内部，使用一个GPU作为parameter server，在外部用cpu做cache。


## TF 机器人应用 by Pi-Chuan Chang

- Challenges: safety, control, transfer
- Minitaur: -> robot platform
- openAI Gym environemt interface -> environment
- agnet: 树莓派； 微控制器计算能力不足
- ARChirecture: signal generator (sin() function -> 周期性) + balanbce controller(Fully connected layers) -> actions 
- transfer to real robot: system identification, env randomization

## TF 高层API by Yifei Feng

- Keras： 高层神经网络API， 默认使用tf作为后端
- tf.keras: 自定义的tf后端，change: from keras -> from tensorflow.keras
- tf.layers and tf.keras.layers, 共享实现方式，基本上相同

- Estimators: canned estimators, 实现好的模型
- keras 模型 -> model_to_estimator -> estimator model

### Distributed TF

- tf的一个很大优势， dev summit 2017
- estimators 可进行分布式执行
- 1.4 版本中找到

### Advice

- 使用可以使用的最高层的API
- 用tf.layers and tf.keras 编写自定义模型
- 分布式训练 Estimators， 大部分情况下最好的选择

### TF Serving

- github.com/tensorflow/serving
- c++ libraries ： tf模型保存/输出格式； 通用核心模式
- tf serving binariesL开箱急用的最佳实践， Docker容器， K8s教程

### get it
- pip apt-get 安装
- www.tf.org/serving

## TF Lite（On Smart Devices）

- offline running on small devices
- low-bandwidth, latency, power
- Chanllenges: bandwith memory computation cpus
- Tf works well on large services, tf-lite works on small devices
- small binary size, low-overhead, optimized set of kernels
- four parts: intepreter (optimized for all devices: few dependencies, small library less than 300k, fast load time, static memory plan, but no control flow), OPs/ kernels (NEON on ARM, Float & quantized, many kernels for mobile apps), model file format (flatbuffers) mmap, more efficient than protocol buffer ; Hardware Acceleration (e.g., Android: Neural Network API; IOS: Core ML); Neural Network API: Part of Android Framework, tries to use as much hardware as possible. 

### Release
- Developer preview: C++ and JAVA API
- TOCO Converter
- A set of builtin ops
- Demo applications
- Example models
- MobileNet(Float)

## Teaching Machines to Draw

- sketch-RNN: K encoder -> Z (Latent Space Vectors)-> Decoder 

--- 
