# 深度残差度量学习用于基于视频监控的情感计算中的行人再识别
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/Lmy0217/DRML/pulls) [![English](https://img.shields.io/badge/README-English-blue.svg)](README.md)

本项目提出了新颖的深度残差度量学习方法用于行人再识别问题，**首次将深度残差网络与度量学习相结合**。论文[下载](ASMMC2017.pdf)。

## 环境
* 系统：Ubuntu 14.04 LTS、CPU i7-3770 @ 3.40GHz×8、GPU GT 630、内存 4G
* 依赖：
  * 支持 GPU 并行计算的 [CUDA](https://developer.nvidia.com/cuda-toolkit) 和 [cuDNN](https://developer.nvidia.com/cudnn)
  * [Torch](https://github.com/torch/torch7) 和默认安装的包（[nn](https://github.com/torch/nn)、[cunn](https://github.com/torch/cunn)、[cutorch](https://github.com/torch/cutorch)、[cudnn](https://github.com/soumith/cudnn.torch)），非默认安装的包包括 [loadcaffe](https://github.com/szagoruyko/loadcaffe) 和 [matio](https://github.com/soumith/matio-ffi.torch)
  * Matlab（R2014a version）- 用于结果可视化脚本

## 准备
### 获取项目
* 如果不需要训练好的模型和结果，请直接克隆本项目

```shell
git clone https://github.com/Lmy0217/DRML.git
cd DRML
```
* 如果需要训练好的模型和结果，请递归克隆本项目（先安装 [Git LFS](https://git-lfs.github.com/)）

```shell
git clone https://github.com/Lmy0217/DRML.git --recursive
cd DRML
```
得到的结果保存在 `./ours` 文件夹，训练好的模型保存在 `./ours/models` 文件夹。

### 数据集
* 下载 [CUHK03](http://www.ee.cuhk.edu.hk/~xgwang/CUHK_identification.html)（labeled & detected）数据集，解压后将文件（cuhk-03.mat）放入 `./datasets/cuhk03` 文件夹。
* 下载 [CUHK01](http://www.ee.cuhk.edu.hk/~xgwang/CUHK_identification.html) 数据集并将解压后的文件放入 `./datasets/cuhk01` 文件夹(现在，这个文件夹中有一个名为 “campus” 的文件夹)。
* 运行 `datasets.lua`

```shell
th datasets.lua
```

### AlexNet 模型
* 下载 [AlexNet](http://dl.caffe.berkeleyvision.org/bvlc_alexnet.caffemodel) 预训练模型放入 `./models` 文件夹。

## 训练和测试
训练包括以下两个步骤：

1. 在 CUHK03 上预训练特征提取部分。
2. 在 CUHK01 上微调特征提取部分和度量学习部分。

### 预训练
* 修改（是否使用残差）并运行 `cnn.lua` 生成预训练模型 `./results/cnn_0.t7`

```shell
th cnn.lua
```
* 运行 `pre-training.lua`，当前轮次的模型保存为 `./results/pre-training/cnn_current.t7`，每轮次损失保存在 `./results/pre-training.log`

```shell
th pre-training.lua
```
* 修改（需要测试的模型）并运行 `test-verify.lua` 进行验证性评估

```shell
th test-verify.lua
```

不同卷积模型在CUHK03测试集上的正确率

| 卷积模型                         |  正确率     |
|---------------------------------|------------|
| 3 conv. + 2 pool.               | 61.12%     |
| AlexNet (DML)                   | 76.61%     |
| **AlexNet + Full Conv.** (DRML) | **77.02%** |

AlexNet + Full Conv. 卷积模型增加实验时间可以获得更高的正确率。

### 微调
* 修改（设置某个完成预训练的模型、是否使用残差）并运行 `drml.lua` 生成微调模型  `./results/drml_0.t7`

```shell
th drml.lua
```
* 运行 `fine-tuning.lua`，当前轮次的模型保存为 `./results/fine-tuning/drml_current.t7`，每 10 轮次保存一次模型 `./results/fine-tuning/drml_[轮次].t7`，每轮次损失保存在 `./results/fine-tuning.log`

```shell
th fine-tuning.lua
```
* 修改（需要测试的模型）并运行 `test-identify.lua` 进行识别性评估，生成预测的相似度（距离） `./results/prediction.txt` （包含两列，第一列是预测相似度（距离），第二列值 1 代表正例、值 -1 代表负例），运行 `prec.m` 获得 P-R 曲线

```shell
th test-identify.lua
matlab14a  -r "run('prec.m'); exit;"
```

不同模型在CUHK01测试集上的P-R曲线

![](./pr.jpg)

DRML 模型增加实验时间可以获得更高的P-R曲线。

## 引用
如果发现本项目对你的研究有帮助，请考虑引用：
```
@article{luo2017deep,
  title={Deep Residual Metric Learning for Human Re-identification in Video Surveillance-based Affective Computing},
  author={Mingyuan Luo, Wei Huang, Peng Zhang, Jing Li, Min Wan, Huijun Ding, Guang Chen},
  journal={Affective Social Multimedia Computing (ASMMC)},
  year={2017}
}
```

## 许可证
[MIT 许可证](LICENSE)