简体中文 | [English](README_en.md)

# InsightFace Paddle

## 1. 介绍
`InsightFacePaddle`是基于PaddlePaddle实现的，开源深度人脸检测、识别工具。`InsightFacePaddle`目前提供了三个预训练模型，包括用于人脸检测的 `BlazeFace`、用于人脸识别的 `ArcFace` 和 `MobileFace`。

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
| enable_mkldnn | bool | False | 是否开启 `MKLDNN` 进行预测，当 `--use_gpu` 为 `False` 时该参数有效，默认值为 `False`。 |
| cpu_threads | int | 1 | CPU 预测时开启的线程数量，当 `--use_gpu` 为 `False` 时该参数有效，默认值为 `1`。 |
| input | str | - | 要预测的视频文件或图像文件的路径，或包含图像文件的目录。 |
| output | str | - | 保存预测结果的目录。|
| det | bool | False | 是否进行检测。 |
| det_thresh | float | 0.8 | 检测后处理的阈值，默认值为0.8。 |
| rec | bool | False | 是否进行识别。 |
| index | str | - | 索引文件的路径。 |
| cdd_num | int | 10 | 识别中检索阶段的候选数量，默认值为10。 |
| rec_thresh | float | 0.5 | 识别中的检索阈值，由于剔除相似度过低的候选项。默认值为0.4。 |
| max_batch_size | int | 1 | 识别中 batch_size 上限，默认值为1。 |
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
insightfacepaddle --det --input ./demo/friends/query/friends1.jpg --output ./demo/friends/output
```

检测结果图位于路径 `./demo/friends/output` 下：
<div align="center">
<img src="./demo/friends/output/friends1.jpg"  width = "800" />
</div>

* Video
```bash
insightfacepaddle --det --input ./demo/friends/query/friends.mp4 --output ./demo/friends/output
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
insightfacepaddle --det --rec --index ./demo/friends/index.bin --input ./demo/friends/query/friends2.jpg --output ./demo/friends/output
```

检测结果图位于路径 `./demo/friends/output` 下：
<div align="center">
<img src="./demo/friends/output/friends2.jpg"  width = "800" />
</div>

* Video
```bash
insightfacepaddle --det --rec --index ./demo/friends/index.bin --input ./demo/friends/query/friends.mp4 --output ./demo/friends/output
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
args.output = "./demo/friends/output"
input_path = "./demo/friends/query/friends1.jpg"

predictor = face.InsightFace(args)
predictor.predict(input_path)
```

* NumPy
```python
import cv2

parser = face.parser()
args = parser.parse_args()

args.det = True
args.output = "./demo/friends/output"
path = "./demo/friends/query/friends1.jpg"
img = cv2.imread(path)

predictor = face.InsightFace(args)
predictor.predict(img)
```

* Video
```python
parser = face.parser()
args = parser.parse_args()

args.det = True
args.output = "./demo/friends/output"
input_path = "./demo/friends/query/friends.mp4"

predictor = face.InsightFace(args)
predictor.predict(input_path)
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
predictor.predict(input_path)
```

* NumPy
```python
import cv2

parser = face.parser()
args = parser.parse_args()

args.rec = True
args.index = "./demo/friends/index.bin"
path = "./demo/friends/query/Rachel.png"
img = cv2.imread(path)

predictor = face.InsightFace(args)
predictor.predict(img)
```

3. 检测+识别系统串联

* Image(s)
```python
parser = face.parser()
args = parser.parse_args()

args.det = True
args.rec = True
args.index = "./demo/friends/index.bin"
args.output = "./demo/friends/output"
input_path = "./demo/friends/query/friends1.jpg"

predictor = face.InsightFace(args)
predictor.predict(input_path)
```

* NumPy
```python
import cv2

parser = face.parser()
args = parser.parse_args()

args.det = True
args.rec = True
args.index = "./demo/friends/index.bin"
args.output = "./demo/friends/output"
path = "./demo/friends/query/friends1.jpg"
img = cv2.imread(path)

predictor = face.InsightFace(args)
predictor.predict(img)
```

* Video
```python
parser = face.parser()
args = parser.parse_args()

args.det = True
args.rec = True
args.index = "./demo/friends/index.bin"
args.output = "./demo/friends/output"
input_path = "./demo/friends/query/friends.mp4"

predictor = face.InsightFace(args)
predictor.predict(input_path)
```
