# Knowledge Distillation via N-to-One Representation Matching

By Xiaolong Liu, Lujun Li, Chao Li and Anbang Yao.

This repository is an official PyTorch implementation of "NORM: Knowledge Distillation via N-to-One Representation Matching" published by ICLR 2023. NORM, the first knowledge distillation work with a novel N-to-one representation matching formulation, which can be readily used to boost the performance of efficient neural networks for different visual recognition tasks.

## Installation

This framework is implemented under Pytorch 1.10.1, Python 3.8.5 and CUDA 11.6. It's better to implement an independent virtual environment on machine. 

```
conda create -n pt1101 python=1.10.1
source activate pt1101
pip install torch  # install pytorch if you do not have it
```

## A quick demo
Once the installation is done, you can follow the below steps to run a quick demo.
    
    # assume that you are under the root directory of this project,
    # and you have activated your virtual environment if needed.
    bash scripts/fetch_pretrained_imagenet_teacher.sh
    bash scripts/train_imagenet_resnet50_mobilenet.sh
It will download and save the pretrained models in ./save/models

## Running
Here is an example of how to use the code to train student using NORM.
```
python train_cifar_student.py --path_t ./save/models/resnet32x4_vanilla/ckpt_epoch_240.pth 
--distill NORM --model_t resnet32x4 --model_s resnet8x4 -r 0.1 -a 0.9 -b 0 --trial test
```
where
- `--path_t`: specify the path of teacher.
- `--distill`: specify the method of distillation. We have implemented NORM and NORM_CRD for teacher student pairs on CIFAR100 and NORM for ImageNet.
- `--model_t`: name of the teacher model. All the implemented can be found in arguments in train_cifar_sutdent.py and tranin_imagenet_student.py.
- `--model_s`: name of the student model. More options can refer to files the same as the teacher.
- `-r`: the weight of the CE loss between logits and the ground truth for student model.
- `-a`: the weight of the normal KD loss, which uses KL divergence between teacher and student logits.
- `-b`: the weight of NORM loss.
- `-s`: the NORM multiple parameter.
- `--trial`: the comments attach to each experiment.
other more parameters can refer to the train file.

## Results

we provide part of our results as follows:
experiments on CIFAR100 with same network structure.

| Teacher                  |    WRN-40-2     | WRN-40-2 | ResNet56 | ResNet110 | ResNet110 | ResNet32x4 | VGG13 |
|--------------------------|:---------------:|:--------:|:--------:|:---------:|:---------:|:----------:|:-----:|
| Student                  |    WRN-16-2     | WRN-40-1 | ResNet20 | ResNet20  | ResNet32  | ResNet8x4  | VGG8  |
| Teacher                  |      75.61      |  75.61   |  72.34   |   74.31   |   74.31   |   79.42    | 74.64 |
| Student(reported in CRD) |      73.26      |  71.98   | 69.06    |  69.06    |  71.14    |  72.50     | 70.36|
| Student(our reproduced)  |      73.80      |  71.70   |  69.53   |   69.53   |   71.56   |   72.87    |70.75|
| Student(w/ 1 plain FT)   |      72.59      |  71.14   |  68.09   |   68.09   |   70.17   |   73.51    | 70.17|
| Student(w/ 1 default FT) |      73.72      |  72.09   |  69.55   |   69.55   |   71.64   |   73.72    |70.64|
| KD                       |      74.92      |  73.54   |  70.66   |   70.67   |   73.08   |   73.33    | 72.98 |
| FitNet                   |      73.58      |  72.24   |  69.21   |   68.99   |   71.06   |   73.50    | 71.02 
| AT                       |      74.08      |  72.77   |  70.55   |   70.22   |   72.31   |   73.44    | 71.43 
| SP                       |      73.83      |  72.43   |  69.67   |   70.04   |   72.69   |   72.94    | 72.68 
| CC                       |      73.56      |  72.21   |  69.63   |   69.48   |   71.48   |   72.97    | 70.71 
| VID                      |      74.11      |  73.30   |  70.38   |   70.16   |   72.61   |   73.09    | 71.23 
| RKD                      |      73.35      |  72.22   |  69.61   |   69.25   |   71.82   |   71.90    | 71.48 
| PKT                      |      74.54      |  73.45   |  70.34   |   70.25   |   72.61   |   73.64    | 72.88 
| AB                       |      72.50      |  72.38   |  69.47   |   69.53   |   70.98   |   73.17    | 70.94 
| FT                       |      73.25      |  71.59   |  69.84   |   70.22   |   72.37   |   72.86    | 70.58 
| FSP                      |      72.91      |   n/a    |  69.95   |   70.11   |   71.89   |   72.62    | 70.23 
| NST                      |      73.68      |  72.24   |  69.60   |   69.53   |   71.96   |   73.30    | 71.53 
| CRD                      |      75.48      |  74.14   |  71.16   |   71.46   |   73.48   |   75.51    | 73.94 
| SRRL                     |       n/a       |  74.64   |   n/a    |    n/a    |    n/a    |   75.39    |  n/a  
| SemCKD                   |       n/a       |  74.41   |   n/a    |    n/a    |    n/a    |   76.23    | 74.43 
| ReviewKD                 |      76.12      |  75.09   |  71.89   |    n/a    |   73.89   |   75.63    | 74.84 
| SimKD                    |       n/a       |  75.56   |   n/a    |    n/a    |    n/a    |   78.08    |  n/a  
| DistPro                  |      76.36      |   n/a    |  72.03   |    n/a    |   73.74   |    n/a     |  n/a  
| NORM     (w/ 1 plain FT) |  75.57   |  74.78   |   70.70   |   71.01   |   73.27    | 76.76 |73.64
| NORM                     |      75.65      |  74.82   |  71.35   |   71.55   |   73.67   |   76.49    | 73.95 
| NORM+KD                  |      76.26      |  75.42   |  71.61   |   72.00   |   73.95   |   76.98    | 74.46 
| NOMR+CL                  |      76.02      |  75.37   |  71.51   |   71.90   |   73.81   |   76.49    | 73.58 


Experiments on CIFAR100 with heterogeneous network structure.

| Teacher                      |    VGG13    |  ResNet50   | ResNet50 |  ResNet32x4  |  ResNet32x4 |  WRN-40-2  |
------------------------------|:-----------:|:-----------:|:--------:|:------------:|:------------:|:----------:|
 Student                      | MobileNetV2 | MobileNetV2 |   VGG8   | ShuffleNetV1 | ShuffleNetV2 |ShuffleNetV1|
 Teacher                      |    74.64    |    79.34    |  79.34   |    79.42     |    79.42     |   75.61    |
 Student (reported in CRD)    |    64.60    |    64.60    |  70.36   |    70.50     |    71.82     |   70.50    |
 Student (our reproduced)     |    64.81    |    64.81    |  70.75   |    71.63     |    72.96     |   71.63    |
 Student (w/ 1 plain FT)      |    63.95    |    63.95    |  70.17   |    71.82     |    72.55     |   71.82    |
 Student (w/ 1 residual FT)   |    64.13    |    64.13    |  70.64   |    71.76     |    72.71     |   71.76    |
 KD                           |    67.37    |    67.35    |  73.81   |    74.07     |    74.45     |   74.83    |
 FitNet                       |    64.14    |    63.16    |  70.69   |    73.59     |    73.54     |   73.73    |
 AT                           |    59.40    |    58.58    |  71.84   |    71.73     |    72.73     |   73.32    |
 SP                           |    66.30    |    68.08    |  73.34   |    73.48     |    74.56     |   74.52    |
 CC                           |    64.86    |    65.43    |  70.25   |    71.14     |    71.29     |   71.38    |
 VID                          |    65.56    |    67.57    |  70.30   |    73.38     |    73.40     |   73.61    |
 RKD                          |    64.52    |    64.43    |  71.50   |    72.28     |    73.21     |   72.21    |
 PKT                          |    67.13    |    66.52    |  73.01   |    74.10     |    74.69     |   73.89    |
 AB                           |    66.06    |    67.2     |  70.65   |    73.55     |    74.31     |   73.34    |
 FT                           |    61.78    |    60.99    |  70.29   |    71.75     |    72.50     |   72.03    |
 NST                          |    58.16    |    64.96    |  71.28   |    74.12     |    74.68     |   74.89    |
 CRD                          |    69.73    |    69.11    |  74.30   |    75.11     |    75.65     |   76.05    |
 SRRL                         |     n/a     |     n/a     |   n/a    |    75.18     |     n/a      |    n/a     |
 SemCKD                       |     n/a     |     n/a     |   n/a    |     n/a      |    77.62     |    n/a     |
 ReviewKD                     |    70.37    |    69.89    |   n/a    |    77.45     |    77.78     |   77.14    |
 SimKD                        |     n/a     |     n/a     |   n/a    |    77.18     |     n/a      |    n/a     |
 DistPro                      |     n/a     |     n/a     |   n/a    |    77.18     |    77.54     |   77.24    |
 NORM (w/ plain FT)           |    69.37    |    70.94    |  74.37   |    75.93     |    77.34     |   76.61    |
 NORM                         |    68.94    |    70.56    |  75.17   |    77.42     |    78.07     |   77.06    |
 NORM+KD                      |    69.38    |    71.17    |  75.67   |    77.79     |    78.32     |   77.63    |
 NOMR+CL                      |   69.17     |    71.08    |  75.51   |    77.50     |    77.96     |   77.09    |

Results on ImageNet.

| Teacher         |     Student      |  CC   |  SP   |  ONE  | SSKD  |  KD   |  AT   |  OFD  |  RKD  |  CRD  | SRRL  | SemCKD | ReviewKD | SimKD | DistPro | NORM|
|-----------------|:----------------:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:------:|:--------:|:-----:|:-------:|:---:|
| ResNet34(73.31) | ResNet18(70.13)  | 69.96 | 70.62 | 70.55 | 71.62 | 70.68 | 70.59 | 71.08 | 71.34 | 71.17 | 71.73 | 70.87  |  71.61   | 71.66 |  71.89  |72.14|
| ResNet50(76.16) | MobileNet(69.63) |  n/a  |  n/a  |  n/a  |  n/a  | 70.68 | 70.72 | 71.25 | 71.32 | 71.40 | 72.49 |  n/a   |  72.56   |  n/a  |  73.26  |74.26|


The weights trained by us can be downloaded from the following tables:
Weights on CIFAR-100 with same structure.

| Teacher  |                                            wrn-40-2                                            |                                            wrn-40-2                                             | resnet56  |  resnet110   | resnet110 | resnet32x4 |   vgg13   |
|----------|:----------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------:|:---------:|:------------:|:---------:|:----------:|:---------:|
| Student  |                                            wrn-16-2                                            |                                            wrn-40-1                                             | resnet20  |  	resnet20   | 	resnet32 | resnet8x4  |   vgg8    |
| accuracy |[75.83](https://drive.google.com/file/d/1pX_bB2pERyKvM_HZVd39XTApUkaB-M1z/view?usp=share_link)  | [75.08](https://drive.google.com/file/d/1G5ZLFzWpb3QArxmDC294C39iekuRI2v0/view?usp=share_link) | [71.45](https://drive.google.com/file/d/1IkUTkivcB2bDuTr_KCxcSh4Cv67eYYtF/view?usp=share_link) | 	  [71.73](https://drive.google.com/file/d/1AdELKGnAYkVRckSdZgHz79Wt2tVn_oym/view?usp=share_link) | [73.65](https://drive.google.com/file/d/1UoTTI3ZVJ_E-WUeLG0b3aprXRgZESrwW/view?usp=share_link) | [76.74](https://drive.google.com/file/d/178rE0i-32pejRJnwtSpNozjQpJGOjn8z/view?usp=share_link)  | [74.19](https://drive.google.com/file/d/1oRUyy_VChgoLcu4P3Yj0etRMGNFrdbYO/view?usp=share_link) |

Weights on CIFAR-100 with different structure.

| Teacher  |    VGG13    |                                            ResNet50                                            | ResNet50  |  ResNet32x4  |  ResNet32x4  |   WRN-40-2   |
|----------|:-----------:|:----------------------------------------------------------------------------------------------:|:---------:|:------------:|:------------:|:------------:|
| Student  | MobileNetV2 |                                          MobileNetV2                                           |   VGG8    | ShuffleNetV1 | ShuffleNetV2 | ShuffleNetV1 |
| accuracy |  [69.11](https://drive.google.com/file/d/1-Xk2q7qthfbBtXnR6XuBl8aN6rRKGNnF/view?usp=share_link)  | [71.27](https://drive.google.com/file/d/1ixtRd2JEXeUGGTS2e8c23KSc4tEdH0y3/view?usp=share_link) | [75.43](https://drive.google.com/file/d/1kkYpT1OprZB9ah-T-B3r8qN5TrNS3dvq/view?usp=share_link) |  [77.19](https://drive.google.com/file/d/16S1m69zrdPSugjs1J90vDrIbbSTUc-aj/view?usp=share_link)   |  [78.09](https://drive.google.com/file/d/1HS-cPVvocJmgO07kkndGHVBBIayyBzH5/view?usp=share_link)   |  [77.13](https://drive.google.com/file/d/1Hu6kASoFB_yKxaibQZHi6_fbLbYrY9VQ/view?usp=share_link)   |

Weights on ImageNet.

| Teacher  |                                            resnet34                                            | resnet50  |  resnet50  |
|----------|:----------------------------------------------------------------------------------------------:|:---------:|:----------:|
| Student  |                                            resnet18                                            | mobilenet | Resnet50_4 |
| accuracy | [72.14](https://drive.google.com/file/d/129RVoVvBRf6qndf1FO3A3WA1JBaAtiOx/view?usp=share_link) | [74.26](https://drive.google.com/file/d/13xpFnhHBrCjlF61NLhsiFoZd9exsrFbo/view?usp=share_link) | [67.58](https://drive.google.com/file/d/1jIUjfxLv2TvudAZPk7g7H80bg4ggQ3zb/view?usp=share_link)  |

## Citation
If you find this repo useful for your research, please consider citing the paper.
```angular2html
@inproceedings{liunorm,
  title={NORM: Knowledge Distillation via N-to-One Representation Matching},
  author={Liu, Xiaolong and Li, Lujun and Li, Chao and Yao, Anbang},
  booktitle={The Eleventh International Conference on Learning Representations}
}


```
## Acknowledgement
We have push our work forward by referring to the work of [RepDistiller](https://github.com/HobbitLong/RepDistiller), and we show much gratitude for them.







