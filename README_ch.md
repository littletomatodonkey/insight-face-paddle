简体中文 | [English](README_en.md)

------------------------------------------------------------------------------------------

<p align="left">
    <a href="./LICENSE"><img src="https://img.shields.io/badge/license-Apache%202-dfd.svg"></a>
    <a href="https://github.com/littletomatodonkey/insight-face-paddle/releases"><img src="https://img.shields.io/github/v/release/littletomatodonkey/insight-face-paddle?color=ffa"></a>
    <a href=""><img src="https://img.shields.io/badge/python-3.7+-aff.svg"></a>
    <a href=""><img src="https://img.shields.io/badge/os-linux%2C%20win%2C%20mac-pink.svg"></a>
    <a href=""><img src="https://img.shields.io/pypi/format/insightface-paddle?color=c77"></a>
    <a href="https://github.com/littletomatodonkey/insight-face-paddle/graphs/contributors"><img src="https://img.shields.io/github/contributors/littletomatodonkey/insight-face-paddle?color=9ea"></a>
    <a href="https://pypi.org/project/insightface-paddle/"><img src="https://img.shields.io/pypi/dm/insightface-paddle?color=9cf"></a>
    <a href="https://github.com/littletomatodonkey/insight-face-paddle/stargazers"><img src="https://img.shields.io/github/stars/littletomatodonkey/insight-face-paddle?color=ccf"></a>
</p>

# InsightFace Paddle

## 1. 介绍

### 1.1 总览
`InsightFacePaddle`是基于PaddlePaddle实现的，开源深度人脸检测、识别工具。`InsightFacePaddle`目前提供了三个预训练模型，包括用于人脸检测的 `BlazeFace`、用于人脸识别的 `ArcFace` 和 `MobileFace`。

- 本部分内容为Whl包预测部署部分。
- 人脸识别相关内容可以参考：[人脸识别](https://github.com/deepinsight/insightface/blob/master/recognition/arcface_paddle/README_cn.md)。
- 人脸检测相关内容可以参考：[基于BlazeFace的人脸检测](https://github.com/deepinsight/insightface/blob/master/detection/blazeface_paddle/README_cn.md)。

Note: 在此非常感谢 [GuoQuanhao](https://github.com/GuoQuanhao) 基于PaddlePaddle复现了 [Arcface的基线模型](https://github.com/GuoQuanhao/arcface-Paddle)。

### 1.2 模型benchmark

在人脸检测任务中，在WiderFace数据集上，BlazeFace的速度与精度指标信息如下。

| 模型结构                  | 模型大小 | WiderFace精度   | CPU 耗时 | GPU 耗时 |
| :-------------------------: | :-----: | :-----: | :--------: | :--------: |
| BlazeFace-FPN-SSH-Paddle      | 0.65MB | 0.9187/0.8979/0.8168 | 31.7ms  |  5.6ms |
| RetinaFace      | 1.68MB | -/-/0.825 | 182.0ms  | 17.4ms |

在人脸识别任务中，基于MS1M训练集，模型指标在lfw、cfp_fp、agedb30上的精度指标以及CPU、GPU的预测耗时如下。

| 模型结构                  | lfw   | cfp_fp | agedb30  | CPU 耗时 | GPU 耗时 |
| :-------------------------: | :-----: | :------: | :-------: | :-------: | :--------: |
| MobileFaceNet-Paddle      | 0.9945 | 0.9343  | 0.9613 | 4.3ms | 2.3ms   |
| MobileFaceNet-mxnet | 0.9950 | 0.8894  | 0.9591   |  7.3ms | 4.7ms   |


**测试环境：**
* CPU: Intel(R) Xeon(R) Gold 6184 CPU @ 2.40GHz
* GPU: a single NVIDIA Tesla V100

**注：** `RetinaFace`的性能数据是使用脚本[test.py](https://github.com/deepinsight/insightface/blob/master/detection/retinaface/test.py)测试得到，这里将`RetinaFace`的image shape修改为`640x480`进行测试。`MobileFaceNet-mxnet`的性能数据是使用脚本：[verification.py](https://github.com/deepinsight/insightface/blob/master/recognition/arcface_mxnet/verification.py)测试得到。


### 1.3 可视化结果展示

基于 `InsightFacePaddle` 的预测结果，示例如下，更多示例结果请参考 [示例](./demo/friends/output/)。


<div align="center">
<img src="./demo/friends/output/friends3.jpg"  width = "800" />
</div>

### 1.4 欢迎加入深度学习技术交流群

QQ扫描二维码加入交流群（群号：`705899115`）。

<div align="center">
<img src="./qq_group.jpeg"  width = "300" />
</div>

## 2. 安装
1. 安装 PaddlePaddle

`InsightFacePaddle` 需要使用 PaddlePaddle 2.1 及以上版本，可以参考以下步骤安装。

```bash
# for GPU
pip3 install paddlepaddle-gpu

# for CPU
pip3 install paddlepaddle
```
关于安装 PaddlePaddle 的更多信息，请参考 [PaddlePaddle](https://www.paddlepaddle.org.cn/)。

2. 安装 requirements

`InsightFacePaddle` 的依赖在 `requirements.txt` 中，你可以参考以下步骤安装依赖包。

```bash
pip3 install --upgrade -r requirements.txt -i https://mirror.baidu.com/pypi/simple
```

3. 安装 `InsightFacePaddle`

* [建议] 你可以使用 `pip` 工具从 `pypi` 安装最新版本的 `InsightFacePaddle`。

```bash
pip3 install --upgrade insightface-paddle
```

* 你也可以自己构建并安装，请参考以下步骤。

```bash
cd ./InsightFacePaddle
python3 setup.py bdist_wheel
pip3 install dist/*
```

## 3. 快速开始

`InsightFacePaddle` 提供 `命令行（Command Line）` 和 `Python接口` 两种使用方式。

### 3.1 命令行

在命令行中使用 `InsightFacePaddle`。

#### 3.1.1 获取参数信息

你可以通过以下命令获取参数信息。

```bash
insightfacepaddle -h
```

`InsightFacePaddle` 的参数及其解释如下:
| 参数名（args） |  类型（type） | 默认值（default） | 说明（help） |
| ---- | ---- | ---- | ---- |
| det_model | str | BlazeFace | 检测模型名称，或本地模型文件所在目录，该目录下需包含 `inference.pdmodel`、`inference.pdiparams` 和 `inference.pdiparams.info` 三个文件。 |
| rec_model | str | MobileFace | 识别模型名称，或本地模型文件所在目录，该目录下需包含 `inference.pdmodel`、`inference.pdiparams` 和 `inference.pdiparams.info` 三个文件。 |
| use_gpu | bool | True | 是否使用 `GPU` 进行预测，默认为 `True`。 |
| enable_mkldnn | bool | False | 是否开启 `MKLDNN` 进行预测，当 `--use_gpu` 为 `False` 且 `--enable_mkldnn` 为 `True` 时该参数有效，默认值为 `False`。 |
| cpu_threads | int | 1 | CPU 预测时开启的线程数量，当 `--use_gpu` 为 `False` 时该参数有效，默认值为 `1`。 |
| input | str | - | 要预测的视频文件或图像文件的路径，或包含图像文件的目录。 |
| output | str | - | 保存预测结果的目录。|
| det | bool | False | 是否进行检测。 |
| det_thresh | float | 0.8 | 检测后处理的阈值，默认值为`0.8`。 |
| rec | bool | False | 是否进行识别。 |
| index | str | - | 索引文件的路径。 |
| cdd_num | int | 5 | 识别中检索阶段的候选数量，默认值为`5`。 |
| rec_thresh | float | 0.45 | 识别中的检索阈值，由于剔除相似度过低的候选项。默认值为`0.45`。 |
| max_batch_size | int | 1 | 识别中 batch_size 上限，默认值为`1`。 |
| build_index | str | - | 要构建的索引文件路径。 |
| img_dir | str | - | 用于构建索引的图像文件目录。 |
| label | str | - | 用于构建索引的标签文件路径。 |


#### 3.1.2 构建索引

如果使用识别功能，则在开始预测之前，必须先构建索引文件，命令如下。

```bash
insightfacepaddle --build_index ./demo/friends/index.bin --img_dir ./demo/friends/gallery --label ./demo/friends/gallery/label.txt
```

用于构建索引的数据示例如下：
<div align="center">
<img src="./demo/friends/gallery/Rachel/Rachel00035.jpg"  width = "200" />
</div>

#### 3.1.3 预测

1. 仅检测

* Image(s)

使用下图进行测试：
<div align="center">
<img src="./demo/friends/query/friends1.jpg"  width = "800" />
</div>

预测命令如下：
```bash
insightfacepaddle --det --input ./demo/friends/query/friends1.jpg --output ./output
```

检测结果图位于路径 `./output` 下：
<div align="center">
<img src="./demo/friends/output/friends1.jpg"  width = "800" />
</div>

* Video
```bash
insightfacepaddle --det --input ./demo/friends/query/friends.mp4 --output ./output
```

2. 仅识别

* Image(s)

使用下图进行测试：
<div align="center">
<img src="./demo/friends/query/Rachel.png"  width = "200" />
</div>

预测命令如下：
```bash
insightfacepaddle --rec --index ./demo/friends/index.bin --input ./demo/friends/query/Rachel.png
```
检测结果输出在终端中：
```bash
INFO:root:File: Rachel.png, predict label(s): ['Rachel']
```

3. 检测+识别系统串联

* Image(s)

使用下图进行测试：
<div align="center">
<img src="./demo/friends/query/friends2.jpg"  width = "800" />
</div>

预测命令如下：
```bash
insightfacepaddle --det --rec --index ./demo/friends/index.bin --input ./demo/friends/query/friends2.jpg --output ./output
```

检测结果图位于路径 `./output` 下：
<div align="center">
<img src="./demo/friends/output/friends2.jpg"  width = "800" />
</div>

* Video
```bash
insightfacepaddle --det --rec --index ./demo/friends/index.bin --input ./demo/friends/query/friends.mp4 --output ./output
```

### 3.2 Python

同样可以在 Python 中使用 `InsightFacePaddle`。首先导入 `InsightFacePaddle`，因为 `InsightFacePaddle` 使用 `logging` 控制日志输入，因此需要导入 `logging`。

```python
import insightface_paddle as face
import logging
logging.basicConfig(level=logging.INFO)
```

#### 3.2.1 获取参数信息

```python
parser = face.parser()
help_info = parser.print_help()
print(help_info)
```

#### 3.2.2 构建索引

```python
parser = face.parser()
args = parser.parse_args()
args.build_index = "./demo/friends/index.bin"
args.img_dir = "./demo/friends/gallery"
args.label = "./demo/friends/gallery/label.txt"
predictor = face.InsightFace(args)
predictor.build_index()
```

#### 3.2.3 预测

1. 仅检测

* Image(s)
```python
parser = face.parser()
args = parser.parse_args()

args.det = True
args.output = "./output"
input_path = "./demo/friends/query/friends1.jpg"

predictor = face.InsightFace(args)
res = predictor.predict(input_path)
print(next(res))
```

* NumPy
```python
import cv2

parser = face.parser()
args = parser.parse_args()

args.det = True
args.output = "./output"
path = "./demo/friends/query/friends1.jpg"
img = cv2.imread(path)[:, :, ::-1]

predictor = face.InsightFace(args)
res = predictor.predict(img)
print(next(res))
```

* Video
```python
parser = face.parser()
args = parser.parse_args()

args.det = True
args.output = "./output"
input_path = "./demo/friends/query/friends.mp4"

predictor = face.InsightFace(args)
res = predictor.predict(input_path)
for _ in res:
    print(_)
```

2. 仅识别

* Image(s)
```python
parser = face.parser()
args = parser.parse_args()

args.rec = True
args.index = "./demo/friends/index.bin"
input_path = "./demo/friends/query/Rachel.png"

predictor = face.InsightFace(args)
res = predictor.predict(input_path, print_info=True)
next(res)
```

* NumPy
```python
import cv2

parser = face.parser()
args = parser.parse_args()

args.rec = True
args.index = "./demo/friends/index.bin"
path = "./demo/friends/query/Rachel.png"
img = cv2.imread(path)[:, :, ::-1]

predictor = face.InsightFace(args)
res = predictor.predict(img, print_info=True)
next(res)
```

3. 检测+识别系统串联

* Image(s)
```python
parser = face.parser()
args = parser.parse_args()

args.det = True
args.rec = True
args.index = "./demo/friends/index.bin"
args.output = "./output"
input_path = "./demo/friends/query/friends2.jpg"

predictor = face.InsightFace(args)
res = predictor.predict(input_path, print_info=True)
next(res)
```

* NumPy
```python
import cv2

parser = face.parser()
args = parser.parse_args()

args.det = True
args.rec = True
args.index = "./demo/friends/index.bin"
args.output = "./output"
path = "./demo/friends/query/friends2.jpg"
img = cv2.imread(path)[:, :, ::-1]

predictor = face.InsightFace(args)
res = predictor.predict(img, print_info=True)
next(res)
```

* Video
```python
parser = face.parser()
args = parser.parse_args()

args.det = True
args.rec = True
args.index = "./demo/friends/index.bin"
args.output = "./output"
input_path = "./demo/friends/query/friends.mp4"

predictor = face.InsightFace(args)
res = predictor.predict(input_path, print_info=True)
for _ in res:
    pass
```
