<div align="center" markdown>

# 图像测量技术实验作业


</div>

## Introduction

Use mindsopre framework and mindcv library to complete the implementation of Swin Transformer algorithm.Swin-Transformer is the best paper of ICCV 2021.
Swin Transformer is a deep learning model based on Transformer with SOTA performance in the visual field. It has better performance and accuracy than ViT.

The version of  **MindSpore is 2.1.1**.

## Environment Creation

- **CUDA version is 11.6:.** 

    ```pycon
    # Download Code
    >>> git clone https://github.com/ChaselLau666/Image-Measurement-Technology-Work.git
    # create a conda environment
    >>> conda create -n ms python=3.8
    # Install mindspore 2.1.1
    >>> conda install mindspore=2.1.1 -c mindspore -c conda-forge
    ```
    See [Installation](https://www.mindspore.cn/install/) for details


## Data set download

You should Install download library

```pycon
pip install download
```

Below is a code  to download the dataset

```pycon
from download import download
dataset_url = "https://mindspore-website.obs.cn-north-4.myhuaweicloud.com/notebook/datasets/vit_imagenet_dataset.zip"
path = "./"
path = download(dataset_url, path, kind="zip", replace=True)
```



**Image classification demo**

Right click on the image below and save as `dog.jpg`.

<p align="left">
  <img src="https://user-images.githubusercontent.com/8156835/210049681-89f68b9f-eb44-44e2-b689-4d30c93c6191.jpg" width=360 />
</p>

Classify the downloaded image with a pretrained SoTA model:


## Training

It is easy to train  model on a standard or customized dataset using `train.py`.
The mindcv module provides relevant definition components. You only need to define the location of the data set and the relative parameters of the model. The model parameters have been encapsulated in `ckg`.

- Model Parameters

    ```shell
    mode: 0
    distribute: True
    num_parallel_workers: 1
    val_while_train: True
    val_interval: 1
    # dataset
    dataset: "imagenet"
    data_dir: "/path/to/imagenet"
    shuffle: True
    dataset_download: False
    batch_size: 16
    drop_remainder: True
    # augmentation
    image_resize: 224
    scale: [0.08, 1.0]
    ratio: [0.75, 1.333]
    hflip: 0.5
    interpolation: "bicubic"
    re_prob: 0.1
    mixup: 0.2
    cutmix: 1.0
    cutmix_prob: 1.0
    crop_pct: 0.875
    color_jitter: [0.4, 0.4, 0.4]
    auto_augment: "randaug-m7-mstd0.5"

    # model
    model: "swin_tiny"
    num_classes: 1000
    pretrained: False
    ckpt_path: ""
    keep_checkpoint_max: 1
    ckpt_save_policy: "top_k"
    ckpt_save_dir: "./ckpt"
    epoch_size: 1000
    dataset_sink_mode: True
    amp_level: "O2"

    # loss
    loss: "CE"
    loss_scale: 1024.0
    label_smoothing: 0.1

    # lr scheduler
    scheduler: "cosine_decay"
    lr: 0.00006
    min_lr: 1e-6
    warmup_epochs: 32
    decay_epochs: 568
    lr_epoch_stair: False

    # optimizer
    opt: "adamw"
    weight_decay: 0.025
    filter_bias_and_bn: True
    use_nesterov: False
    ```

    Above are the model parameters
- Training

    ```shell
    # training
    python train1.py --config /home/xiangchengliu/mindcv/configs/swintransformer/swin_tiny.yaml --data_dir /home/xiangchengliu/mindcv/dataset  --distribute False
    ```
    During training, you need to specify training parameters and data sets.
    The above path is an absolute path
    


## View training results

In order to view the training status, mindinsight must be installed

```pycon
pip install mindinsight
```
In order to view the loss changes during training, you can run

```pycon
mindinsight start --summary-base-dir=./summary_dir
```

## References

Currently, MindCV supports the model families listed below. More models with pre-trained weights are under development and will be released soon.

<details open markdown>
<summary> Supported models </summary>

* ResNet (v1b/v1.5) - https://arxiv.org/abs/1512.03385
* Vision Transformer (ViT) - https://arxiv.org/abs/2010.11929
* Swin Transformer - https://arxiv.org/abs/2103.14030


Please see [configs](./configs) for the details about model performance and pretrained weights.

</details>

## Used Algorithms

<details open markdown>
<summary> Supported algorithms </summary>

* Augmentation
    * [AutoAugment](https://arxiv.org/abs/1805.09501)
    * [RandAugment](https://arxiv.org/abs/1909.13719)
    * [Repeated Augmentation](https://openaccess.thecvf.com/content_CVPR_2020/papers/Hoffer_Augment_Your_Batch_Improving_Generalization_Through_Instance_Repetition_CVPR_2020_paper.pdf)
    * RandErasing (Cutout)
    * CutMix
    * MixUp
    * RandomResizeCrop
    * Color Jitter, Flip, etc
* Optimizer
    * Adam
    * AdamW
    * [Lion](https://arxiv.org/abs/2302.06675)
    * Adan (experimental)
    * AdaGrad
    * LAMB
    * Momentum
    * RMSProp
    * SGD
    * NAdam
* LR Scheduler
    * Warmup Cosine Decay
    * Step LR
    * Polynomial Decay
    * Exponential Decay
* Regularization
    * Weight Decay
    * Label Smoothing
    * Stochastic Depth (depends on networks)
    * Dropout (depends on networks)
* Loss
    * Cross Entropy (w/ class weight and auxiliary logit support)
    * Binary Cross Entropy  (w/ class weight and auxiliary logit support)
    * Soft Cross Entropy Loss (automatically enabled if mixup or label smoothing is used)
    * Soft Binary Cross Entropy Loss (automatically enabled if mixup or label smoothing is used)
* Ensemble
    * Warmup EMA (Exponential Moving Average)

</details>


## License

This project follows the [Apache License 2.0](LICENSE.md) open-source license.

## Acknowledgement

MindCV is an open-source project jointly developed by the MindSpore team, Xidian University, and Xi'an Jiaotong University.
Sincere thanks to all participating researchers and developers for their hard work on this project.
We also acknowledge the computing resources provided by [OpenI](https://openi.pcl.ac.cn/).

## Citation

If you find this project useful in your research, please consider citing:

```latex
@misc{MindSpore Computer Vision 2022,
    title={{MindSpore Computer Vision}:MindSpore Computer Vision Toolbox and Benchmark},
    author={MindSpore Vision Contributors},
    howpublished = {\url{https://github.com/mindspore-lab/mindcv/}},
    year={2022}
}
```
# Image-Measurement-Technology-Work
Use mindsopre framework and mindcv library to complete the implementation of Swin Transformer algorithm
