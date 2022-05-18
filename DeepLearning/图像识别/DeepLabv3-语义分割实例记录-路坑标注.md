---
title: DeepLabv3+语义分割实例记录-路坑标注
tags:
  - DeepLearning
  - CV
abbrlink: d2c1a00
date: 2022-04-23 17:17:05
---

# 本文默认

- 已经装配好了anaconda
- 拥有NVDIA支持进行GPU运算的显卡以及正确装配的驱动
- 已配置好了代理以及下载加速
- 操作环境为Windows 11+Kali Linux(WSL2)

# 环境配置

## 基础环境

首先创建一个anaconda的虚拟环境，进入`Anconda Prompt`，这里我就用pytorch作为环境名，python3.8为必须选项（网上大部分描写的3.6版本python现在已经不受支持，使用最新环境中会遇到诸多问题）

```shell
conda create -n pytorch python=3.8
```

安装成功后激活环境

```shell
conda activate pytorch
```

如果前面显示`(pytorch)`，则说明已经成功进入环境，然后执行pytorch安装命令（最新命令可以在官网查到）

```shell
conda install pytorch torchvision cudatoolkit=10.2 -c pytorch
```

## Deeplabv3+装配

### 克隆和安装deeplabv3+

```shell
git clone https://github.com/VainF/DeepLabV3Plus-Pytorch.git
```

clone成功后在其目录下执行

```shell
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

装配相关依赖

## 数据集收集

![VOC2007_JPEGImages](https://image.0error.net/img/2022/20220429205621.png)

这里使用经典的`VOC2007`以及`VOC2012`数据集进行处理，下载之后得到几个数据集文件在本地进行解压。

文件夹层次为VOCdevkit/VOC2007和VOCdevkit/VOC2012

VOC2007和VOC2012下面有若干分类后的文件夹

![VOC Structure](https://image.0error.net/img/2022/20220429205644.png)

- JPEGIamges放所有的数据集图片
- Annotations放所有的xml标记文件
- SegmentationClass放标注的数据集掩码文件
- ImageSets/Segmentation下存放训练集、验证集、测试集划分文件
  - train.txt给出了训练集图片文件列表（不含后缀）
  - val.txt给出了验证集图片文件的列表
  - trainval.txt给出了训练集和验证集图片文件的列表
  - test.txt给出测试集图片文件的列表

## 权重文件放置

在主目录下新建`weights`文件夹，将`.pth`标识的权重文件放入

## 测试图片

使用deeplabv3plus_mobilenet模型

```python
python predict.py --input datasets/data/VOCdevkit/VOC2007/JPEGImages/000001.jpg --dataset voc --model deeplabv3plus_mobilenet --ckpt weights/best_deeplabv3plus_mobilenet_voc_os16.pth --save_val_results_to test_results
```

## 标注工具-LabelMe安装

### 安装使用

进入Anaconda，新建一个python版本为3.6的虚拟环境，然后进入该环境，使用pip进行安装

```shell
pip install pyqt5
pip install labelme
```

接着在该环境执行命令

```shell
labelme
```

![labelme](https://image.0error.net/img/2022/20220429205140.png)

打开软件，进行标注，标注后生成json文件，最后将数据集图像文件以及标注的json文件放置在~/mydataset目录下

![label_result](https://image.0error.net/img/2022/20220429205500.png)

### 数据转换

```shell
python labelme2voc.py roadscene_train roadscene_train/data_dataset_voc --labels labels.txt
python labelme2voc.py roadscene_val roadscene_val/data_dataset_voc --labels labels.txt
```



# 模型训练

## 训练网络

### 使用deeplabv3plus_mobilenet模型

```shell
python main.py --model deeplabv3plus_mobilenet --enable_vis --vis_port 8097 --gpu_id 0 --year 2007 --crop_val --lr 0.01 --crop_size 513 --batch_size 4 --output_stride 16 --num_classes 6 --total_itrs 1000 --ckpt weights/best_deeplabv3plus_mobilenet_voc_os16.pth
```

- `--model`: 指定使用的模型
- `--enable_vis`: 启用visdom可视化
- `--vis_port`: 指定visdom使用的端口
- `--gpu_id`: 指定gpu的id
- `--year`: 指定训练数据
- `--crop_val`: 声明进行crop操作
- `--lr`: 声明learning rate（学习率）
- `--crop_size`: 指定crop之后的图片格式为513x513
- `-- batch_size`: 声明batch size
- `--output_stride`: 网络模型对图片进行指定倍数的下采样
- `--num_classes`: 设定为类别数+1
- `--total_itrs`: 迭代数目
- `--ckpt`: 预训练权重文件

## 验证网络模型

### 整体测试集

```shell
python main.py --model deeplabv3plus_mobilenet --gpu_id 0 --year 2007 --crop_val --lr 0.01 --crop_size 513 --batch_size 16 --output_stride 16 --ckpt checkpoints/best_deeplabv3plus_mobilenet_voc_os16.pth --test_only --save_val_results
```

最后结果保存在`result`文件夹下

### 图片测试

#### 单张图片测试

```shell
python predict.py --input datasets/data/VOCdevkit/VOC2007/JPEGImages/img001.jpg -- dataset voc --model deeplabv3plus_mobilenet --ckpt checkpoints/best_deeplabv3plus_mobilenet_voc_os16.pth --save_val_resu lts_to test_results1 --crop_size 513
```

#### 多张图片测试

如果是jpg图片，修改`predict.py`中

```python
files = glob(os.path.join(opts.input, '**/*.png'), recursive=True)
```

为

```python
files = glob(os.path.join(opts.input, '**/*.jpg'), recursive=True)
```

然后再执行命令即可

# 结果展示

![result1](https://image.0error.net/img/2022/18_overlay.png)

![result2](https://image.0error.net/img/2022/0_overlay.png)

![result3](https://image.0error.net/img/2022/178_overlay.png)
