# MonoFormer  

This repo is reference PyTorch implementation for training and testing depth estimation models using the method described in 
> Deep Digging into the Generalization of Self-supervised Monocular Depth Estimation  
> [Jinwoo Bae](https://jinu0418.tistory.com), Sungho Moon and [Sunghoon Im](https://sunghoonim.github.io/)  
> AAAI 2023 ([arxiv](https://arxiv.org/abs/2205.11083))  

<p align="center">
<img src="https://user-images.githubusercontent.com/33753741/211245370-7333887c-fff0-4af8-839c-b4ef78458474.gif" width="100%" height="100%">
</p>

Our code is based on [PackNet-SfM](https://github.com/TRI-ML/packnet-sfm) of TRI.  
If you find our work useful in your research, please consider citing our papers :  
```
@inproceedings{bae2022monoformer,
  title={Deep Digging into the Generalization of Self-supervised Monocular Depth Estimation},
  author={Bae, Jinwoo and Moon, Sungho and Im, Sunghoon},
  booktitle={Proceedings of the AAAI Conference on Artificial Intelligence},
  year={2023}
}
```

## Setup
```
conda create -n monoformer python=3.7
git clone https://github.com/sjg02122/MonoFormer.git
pip install -r requirements.txt
```
We ran our experimentss with PyTorch 1.10.0+cu113, Python 3.7, A6000 GPU and Ubuntu 20.04.
### Pretrained model weights
We experiment extensively on modern backbone architectures (e.g., ConvNeXt, RegionViT). MF means MonoFormer.
|Model|Abs Rel| Sq Rel| RMSE| a1|
|---|---|---|---|---|
|[MF-hybrid](https://seoultechackr-my.sharepoint.com/:u:/g/personal/sjg02122_seoultech_ac_kr/EaOXlfu4WidLswYkb87svNsBc2EJIDqlOevDCRPPrk3wiw?e=alkvpy) |0.104 |0.846 |4.580 |0.891 |
|[MF-ViT](https://seoultechackr-my.sharepoint.com/:u:/g/personal/sjg02122_seoultech_ac_kr/EUYuXNoPMXhOutE6KCt_qRkBihgHOL5BsPCJSxqY9sFdGQ?e=9blLtD) |0.118|0.942 |4.840 |0.873|
|[MF-Twins](https://seoultechackr-my.sharepoint.com/:u:/g/personal/sjg02122_seoultech_ac_kr/EW0s4xWXj1pIgvjBzULucuQBEtsaZpNhnZLPnJrPahW9Eg?e=qwFTHI) | 0.125|1.309 |4.973 |0.866 |
|[MF-RegionViT](https://seoultechackr-my.sharepoint.com/:u:/g/personal/sjg02122_seoultech_ac_kr/EYAPcwQuTI9Kv9NIYmdDnLcBEvWgJTOVOVi_E-7xIqL3vA?e=d2xMAK) |0.113 |0.893 |4.756 |0.875 |
|[MF-ConvNeXt](https://seoultechackr-my.sharepoint.com/:u:/g/personal/sjg02122_seoultech_ac_kr/EcaenxKKHt5GuOzBqUeqgbIByR6odjRsKBTUxrXLV9faBw?e=oArgyc) |0.111 | 0.760|4.533 |0.878 |
|[MF-SLaK](https://seoultechackr-my.sharepoint.com/:u:/g/personal/sjg02122_seoultech_ac_kr/EQ_z478CSIpKkVOXGe8hIXQBQRT7YhR2c-W5mx23aueoTQ?e=DnCdQv) | 0.117|0.866 |4.811 |0.878 |

## Datasets
You configure your datasets in config.py or other config yaml files. (DATA_PATH means your data root path.). 
In our experiments, we only use the KITTI datasets for training. Other datasets (e.g., ETH3D, DeMoN, and etc.) is used for testing.  

### KITTI (for Training)
The KITTI (raw) datasets can be downloaded from the [KITTI website](http://www.cvlibs.net/datasets/kitti/raw_data.php). If you want to download the datasets using command, please use the command of [PackNet-SfM](https://github.com/TRI-ML/packnet-sfm).

### Other datasets (for Evaluation)
In our experiments, we use the ETH3D, DeMoN (e.g., MVS, SUN3D, RGBD, Scenes11) and our generated texture-shifted datasets.  
It will be updated soon.

## Inferernce 
You can directly run inference on a single image or folder:
```
python3 scripts/infer.py --checkpoint <checkpoint.ckpt> --input <image or folder> --output <image or folder> [--image_shape <input shape (h,w)>]
```
You can also evaluate the model using:
```
python3 scripts/eval.py --checkpoint <checkpoint.ckpt> [--config <config.yaml>]
```

## Training
Our training is similar to PackNet-SfM.
Any training, including fine-tuning, can be done by passing either a .yaml config file or a .ckpt model checkpoint to [scripts/train.py](https://github.com/sjg02122/MonoFormer/blob/a71751e2304dcd24275a32c2bb376181b17de125/scripts/train.py#L1):  
```
python3 scripts/train.py <config.yaml or checkpoint.ckpt>
```
