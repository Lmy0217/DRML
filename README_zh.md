# FullScale 分支
[![English](https://img.shields.io/badge/README-English-blue.svg)](README.md)

本分支是 [master 分支](https://github.com/Lmy0217/DRML)的额外模块，其中全尺度 (FullScale) 模型使用改进的 [Continuous Kendall–Tau](http://www.sciencedirect.com/science/article/pii/S0165168415002686) (CKT) 损失函数进行训练。论文[下载](ASMMC2018.pdf)。

在 master 分支主文件夹 `DRML` 内克隆本分支

```shell
git clone -b fullscale https://github.com/Lmy0217/DRML.git FullScale
cd ./FullScale
```
如果需要训练好的模型和结果，请递归克隆（先安装 [Git LFS](https://git-lfs.github.com/)）

```shell
git clone -b fullscale --recursive https://github.com/Lmy0217/DRML.git FullScale
cd ./FullScale
```

## 预训练
* 运行 `cnn.lua` 生成预训练模型 `../results/cnn_0.t7`

```shell
th cnn.lua
```
之后的步骤与 master 分支（除第一步）相同（在 master 分支主文件夹 `DRML` 内）。

## 微调
微调步骤与 master 分支相同 （在本分支主文件夹 `FullScale` 内）。

不同模型在 CUHK01 测试集上的 P-R 曲线

![](./pr.jpg)

FullScale 模型增加实验时间可以获得更高的 P-R 曲线。

## 引用
如果发现本分支对你的研究有帮助，请考虑引用：
```
@article{luo2017deep,
  title={Deep Residual Metric Learning for Human Re-identification in Video Surveillance-based Affective Computing},
  author={Mingyuan Luo, Wei Huang, Peng Zhang, Jing Li, Min Wan, Huijun Ding, Guang Chen},
  journal={Affective Social Multimedia Computing (ASMMC)},
  year={2017}
}
```

## 许可证
继承 [master 分支许可证](https://github.com/Lmy0217/DRML/blob/master/LICENSE)。
