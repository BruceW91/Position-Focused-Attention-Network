## Position-Focused-Attention-Network
Position Focused Attention Network for Image-Text Matching

## Introduction

This is Position Focused Attention Network, source code of position attention for Image-Text Matching (project page) from [Tencent](https://github.com/Tencent). It is built on top of the SCAN (by Kuang-Huei Lee) in PyTorch.

## Requirements and Installation
We recommended the following dependencies.

* Python 2.7
* [PyTorch](http://pytorch.org/) 0.3
* [NumPy](http://www.numpy.org/) (>1.12.1)
* [TensorBoard](https://github.com/TeamHG-Memex/tensorboard_logger)


## Download data
Download the dataset files. We use the dataset files created by SCAN [Kuang-Huei Lee](https://github.com/kuanghuei/SCAN). The position information of images can be downloaded from [here](https://drive.google.com/open?id=1ZiF1IoeExPcn9V9L78X6jEYuMxR96OLO) (for Flickr30K) and here (for MS-COCO).
The Tencent-News dataset files can be downloaded from [here](https://drive.google.com/open?id=1SiZxCMAVSmS7vegAfY6Vn7BqY4j1bLZE).

```bash
#For Flickr30K dataset
wget https://drive.google.com/open?id=1ZiF1IoeExPcn9V9L78X6jEYuMxR96OLO
#For Tencent-News dataset
wget https://drive.google.com/open?id=1SiZxCMAVSmS7vegAfY6Vn7BqY4j1bLZE
```

## Training new models

To train Flickr30K and MS-COCO models:
```bash
sh run_train.sh
```
In order to further improve the performance of PFAN on Tencent-News dataset, the whole image feautre is also considered. The details are shown in Tencent_PFAN code:
```bash
sh Tencent_PFAN/run_train.sh
```

Arguments used to train Flickr30K models and MS-COCO models are as same as those of SCAN:

For Flickr30K:

| Method      | Arguments |
| :---------: | :-------: |
|  t-i     | `--max_violation --bi_gru --agg_func=Mean --cross_attn=t2i --lambda_softmax=9 --num_epoches=30 --lr_update=15 --learning_rate=.0002 --embed_size=1024 --batch_size=128 `|
|  i-t     | `--max_violation --bi_gru --agg_func=Mean --cross_attn=i2t --lambda_softmax=4 --num_epoches=30 --lr_update=15 --learning_rate=.0002 --embed_size=1024 --batch_size=128 `|

For MS-COCO:

| Method      | Arguments |
| :---------: | :-------: |
|  t-i    | `--max_violation --bi_gru --agg_func=Mean --cross_attn=t2i --lambda_softmax=9 --num_epoches=30 --lr_update=15 --learning_rate=.0005 --embed_size=1024 --batch_size=128 `|
|  i-t    | `--max_violation --bi_gru --agg_func=Mean --cross_attn=i2t --lambda_softmax=4 --num_epoches=30 --lr_update=15 --learning_rate=.0005 --embed_size=1024 --batch_size=128 `|

For Tencent-News:

| Method      | Arguments |
| :---------: | :-------: |
|  t-i    | `--max_violation --bi_gru --agg_func=Mean --cross_attn=t2i --lambda_softmax=9 --num_epoches=30 --lr_update=15 --learning_rate=.0002 --embed_size=512 --batch_size=128 --lambda_whole=2 `|
|  i-t    | `--max_violation --bi_gru --agg_func=Mean --cross_attn=i2t --lambda_softmax=4 --num_epoches=30 --lr_update=15 --learning_rate=.0002 --embed_size=512 --batch_size=128 --lambda_whole=2 `|

## Evaluate trained models

```python
from vocab import Vocabulary
import evaluation
evaluation.evalrank("$RUN_PATH/f30k_precomp/model_best.pth.tar", data_path="$DATA_PATH", split="test")
```
