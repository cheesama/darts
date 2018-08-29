# Differentiable Architecture Search
Code accompanying the paper
> [DARTS: Differentiable Architecture Search](https://arxiv.org/abs/1806.09055)\
> Hanxiao Liu, Karen Simonyan, Yiming Yang.\
> _arXiv:1806.09055_.

<p align="center">
  <img src="img/darts.png" alt="darts" width="48%">
</p>
The algorithm is based on continuous relaxation and gradient descent in the architecture space. It is able to efficiently design high-performance convolutional architectures for image classification (on CIFAR-10 and ImageNet) and recurrent architectures for language modeling (on Penn Treebank and WikiText-2). Only a single GPU is required.

## Requirements
```
Python >= 3.5.5, PyTorch == 0.3.1, torchvision == 0.2.0, mlflow
```
NOTE: PyTorch 0.4 is not supported at this moment and would lead to OOM.

## Datasets
Instructions for acquiring PTB and WT2 can be found [here](https://github.com/salesforce/awd-lstm-lm). While CIFAR-10 can be automatically downloaded by torchvision, ImageNet needs to be manually downloaded (preferably to a SSD) following the instructions [here](https://github.com/pytorch/examples/tree/master/imagenet).

## Architecture Search
To carry out architecture search using 1st-order approximation, run
```
cd cnn && python train_search.py     # for conv cells on CIFAR-10
cd rnn && python train_search.py     # for recurrent cells on PTB
```
2nd-order approximation can be enabled by adding the `--unrolled` flag.

Snapshots of the most likely convolutional & recurrent cells over time:
<p align="center">
<img src="img/progress_convolutional.gif" alt="progress_convolutional" width="38%">
<img src="img/progress_recurrent.gif" alt="progress_recurrent" width="48%">
</p>

## Architecture Evaluation
To evaluate our best cells, run
```
cd cnn && python train.py --auxiliary --cutout            # CIFAR-10
cd rnn && python train.py                                 # PTB
cd rnn && python train.py --data ../data/wikitext-2 \     # WT2
            --dropouth 0.15 --emsize 700 --nhidlast 700 --nhid 700 --wdecay 5e-7
cd cnn && python train_imagenet.py --auxiliary            # ImageNet
```
Customized architectures are supported through the `--arch` flag once specified in `genotypes.py`.

Expected performance on CIFAR-10 (4 runs) and PTB:
<p align="center">
<img src="img/cifar10.png" alt="cifar10" width="40%">
<img src="img/ptb.png" alt="ptb" width="40%">
</p>

## Visualization
Package [graphviz](https://graphviz.readthedocs.io/en/stable/index.html) is required to visualize the learned cells
```
python visualize.py DARTS
```
where `DARTS` can be replaced by any customized architectures in `genotypes.py`.

## Citation
If you use any part of this code in your research, please cite our [paper](https://arxiv.org/abs/1806.09055):
```
@article{liu2018darts,
  title={DARTS: Differentiable Architecture Search},
  author={Liu, Hanxiao and Simonyan, Karen and Yang, Yiming},
  journal={arXiv preprint arXiv:1806.09055},
  year={2018}
}
```
