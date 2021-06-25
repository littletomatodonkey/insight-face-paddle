# InsightFace Paddle

## Introduction
`InsightFacePaddle` is an open source deep face detection and recognition toolkit, powered by PaddlePaddle. `InsightFacePaddle` provide three related pretrained models now, include BlazeFace for face detection, ArcFace and MobileFace for face recognition.

## Installation
1. Install PaddlePaddle

PaddlePaddle 2.0 or later is required for `InsightFacePaddle`. You can use the following steps to install PaddlePaddle.

```bash
# for GPU
pip3 install paddlepaddle-gpu

# for CPU
pip3 install paddlepaddle
```
More details about installation refer to [PaddlePaddle](https://www.paddlepaddle.org.cn/).

2. Install requirements

`InsightFacePaddle` dependencies are listed in `requirements.txt`, you can use the following command to install the dependencies.

```bash
pip3 install --upgrade -r requirements.txt -i https://mirror.baidu.com/pypi/simple
```

3. Install `InsightFacePaddle`

* [Recommanded] You can use `pip` to install the lastest version `InsightFacePaddle` from `pypi`.

```bash
pip3 install --upgrade insightface-paddle
```

* You can also build whl package and install by following commands.

```bash
cd ./InsightFacePaddle
python3 setup.py bdist_wheel
pip3 install dist/*
```

## Quick Start

`InsightFacePaddle` support two ways of use, including `Commad Line` and `Python API`.

### Command Line

You can use `InsightFacePaddle` in Command Line.

#### Get help

You can get the help about `InsightFacePaddle` by following command.

```bash
insightfacepaddle -h
```

The args are as follows:
| args |  type | default | help |
| ---- | ---- | ---- | ---- |
| det_model | str | BlazeFace | The detection model. |
| rec_model | str | MobileFace | The recognition model. |
| use_gpu | bool | True | Whether use GPU to predict. Default by True. |
| enable_mkldnn | bool | False | Whether use MKLDNN to predict, valid only when --use_gpu is False. Default by False. |
| cpu_threads | int | 1 | The num of threads with CPU, valid only when --use_gpu is False. Default by 1. |
| input | str | | The path of video to be predicted. Or the path or directory of image file(s) to be predicted. |
| output | str | | The directory to save prediction result. |
| det | | | Whether to detect. |
| threshold | float | 0.5 | The threshold of detection postprocess. Default by 0.5. |
| rec | | | Whether to recognize. |
| base_lib | str | | The path of base lib file. |
| cdd_num | int | 10 | The number of candidates in the recognition retrieval. Default by 10. |
| max_batch_size | int | 1 | The maxium of batch_size to recognize. Default by 1. |
| build_lib | str | | The base lib path to build. |
| img_dir | str | | The img(s) dir used to build base lib. |
| label | str | | The label file path used to build base lib. |



#### Build base lib

Before start predicting, you have to build the base lib.

```bash
insightfacepaddle --build_lib ./base.txt --img_dir ./data/imgs --label ./data/label.txt
```

#### Predict

1. Detection only

* Image(s)
```bash
insightfacepaddle --det --input ./demo.jpeg --output ./output/
```

* Video
```bash
insightfacepaddle --det --input ./demo.mp4 --output ./output/
```


2. Recognition only

* Image(s)
```bash
insightfacepaddle --rec --base_lib ./base.txt --input ./demo.jpep --output ./output/
```

* Video
```bash
insightfacepaddle --rec --base_lib ./base.txt --input ./demo.mp4 --output ./output/
```


3. Detect and recognize

* Image(s)
```bash
insightfacepaddle --det --rec --base_lib ./base.txt --input ./demo.jpeg --output ./output/
```

* Video
```bash
insightfacepaddle --det --rec --base_lib ./base.txt --input ./demo.mp4 --output ./output/
```

### Python

You can use `InsightFacePaddle` in Python. First, import `InsightFacePaddle`:

```python
import insightface as face
```

#### Get help

```python
parser = face.parser()
help_info = parser.print_help()
print(help_info)
```

#### Building base lib

```python
parser = face.parser()
args = parser.parse_args()

args.build_lib = "./base.txt"
args.img_dir = "./data/imgs"
args.label = "./data/label.txt"
predictor = face.InsightFace(args)
predictor.build_lib()
```

#### Prediction

1. Detection only

* Image(s)
```bash
parser = face.parser()
args = parser.parse_args()

args.output = "./output"
args.input = "./demo.jpeg"
args.det = True

predictor = face.InsightFace(args)
predictor.deploy_predict()
```

* Video
```bash
parser = face.parser()
args = parser.parse_args()

args.output = "./output"
args.input = "./demo.mp4"
args.det = True

predictor = face.InsightFace(args)
predictor.deploy_predict()
```


2. Recognition only

* Image(s)
```bash
parser = face.parser()
args = parser.parse_args()

args.output = "./output"
args.input = "./demo.jpeg"
args.base_lib = "./base.txt"
args.rec = True

predictor = face.InsightFace(args)
predictor.deploy_predict()
```

* Video
```bash
parser = face.parser()
args = parser.parse_args()

args.output = "./output"
args.input = "./demo.mp4"
args.base_lib = "./base.txt"
args.rec = True

predictor = face.InsightFace(args)
predictor.deploy_predict()
```

3. Detection and recognition

* Image(s)
```bash
parser = face.parser()
args = parser.parse_args()

args.output = "./output"
args.input = "./demo.jpeg"
args.base_lib = "./base.txt"
args.rec = True
args.det = True

predictor = face.InsightFace(args)
predictor.deploy_predict()
```

* Video
```bash
parser = face.parser()
args = parser.parse_args()

args.output = "./output"
args.input = "./demo.mp4"
args.base_lib = "./base.txt"
args.rec = True
args.det = True

predictor = face.InsightFace(args)
predictor.deploy_predict()
```
