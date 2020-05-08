# SupContrast: Supervised Contrastive Learning
<p align="center">
  <img src="figures/teaser.png" width="700">
</p>

This repo covers an reference implementation for the following papers in PyTorch, using CIFAR as an illustrative example:  
(1) Supervised Contrastive Learning. [Paper](https://arxiv.org/abs/2004.11362)  
(2) A Simple Framework for Contrastive Learning of Visual Representations. [Paper](https://arxiv.org/abs/2002.05709)  

## Loss Function
The loss function [`SupConLoss`](https://github.com/HobbitLong/SupContrast/blob/master/losses.py#L11) in `losses.py` takes `features` (L2 normalized) and `labels` as input, and return the loss. If `labels` is `None` or not passed to the it, it degenerates to SimCLR.

## Comparison
Results on CIFAR-10:
|          |Arch | Setting | Loss | Accuracy(%) |
|----------|:----:|:---:|:---:|:---:|
|  SupCrossEntropy | ResNet50 | Supervised   | Cross Entropy |  95.0  |
|  SupContrast     | ResNet50 | Supervised   | Contrastive   |  96.0  | 
|  SimCLR          | ResNet50 | Unsupervised | Contrastive   |  93.6  |

## Running
You might use `CUDA_VISIBLE_DEVICES` to set proper number of GPUs.  
**(1) Standard Cross-Entropy**
```
python main_ce.py --batch_size 1024 \
  --learning_rate 0.8 \
  --cosine --syncBN \
```
**(2) Supervised Contrastive Learning**  
Pretraining stage:
```
python main_supcon.py --batch_size 1024 \
  --learning_rate 0.5 \
  --temp 0.5 \
  --cosine
```
You can also specify `--syncBN` but I found it not crucial for SupContrast (`syncBN` 95.9% v.s. `BN` 96.0%).  
Linear evaluation stage:
```
python main_linear.py --batch_size 512 \
  --learning_rate 5 \
  --ckpt /path/to/model.pth
```
**(3) SimCLR**  
Pretraining stage:
```
python main_supcon.py --batch_size 1024 \
  --learning_rate 0.5 \
  --temp 0.5 \
  --cosine --syncBN \
  --method SimCLR
```
The `--method SimCLR` flag simply stops `labels` from being passed to `SupConLoss` criterion.
Linear evaluation stage:
```
python main_linear.py --batch_size 512 \
  --learning_rate 1 \
  --ckpt /path/to/model.pth
```

## t-SNE Visualization

**(1) Standard Cross-Entropy**
<p align="center">
  <img src="figures/SupCE.jpg" width="400">
</p>

**(2) Supervised Contrastive Learning**
<p align="center">
  <img src="figures/SupContrast.jpg" width="800">
</p>

**(3) SimCLR**
<p align="center">
  <img src="figures/SimCLR.jpg" width="800">
</p>

## Reference
```
@Article{khosla2020supervised,
    title   = {Supervised Contrastive Learning},
    author  = {Prannay Khosla and Piotr Teterwak and Chen Wang and Aaron Sarna and Yonglong Tian and Phillip Isola and Aaron Maschinot and Ce Liu and Dilip Krishnan},
    journal = {arXiv preprint arXiv:2004.11362},
    year    = {2020},
}
```
