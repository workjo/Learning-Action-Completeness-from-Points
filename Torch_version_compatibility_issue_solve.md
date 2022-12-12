

# LACP code implementation issue about CUDA version

I discuss how to install a virtual environment for 'Learning Action Completeness from Points ([LACP](https://github.com/Pilhyeon/Learning-Action-Completeness-from-Points)) for Weakly-supervised Temporal Action Localization (ICCV 2021 Oral)' code implementation to meet different CUDA versions.


## Motivation
### CUDA version conflict on RTX 3090
I had tried to run the official LACP code on RTX 3090 but faced a CUDA compatibility issue.<br>
The issue is that recommended CUDA version is 10.2, but RTX 3090 is only compatible with CUDA version 11 or higher.<br>
So I attempted to change PyTorch and Torchvision that support CUDA version 11 or higher.

## Prerequisites
Required the environment's dependencies are as below. <br>
(A -> B: A is an officially recommended version, and B is a changed version to support CUDA version 11.)

### Dependencies
* python == 3.6
* joblib==0.13.0<br>
* numpy==1.18.0<br>
* pandas==1.1.4<br>
* scikit-learn==0.20.0<br>
* scipy==1.4.1<br>
* tensorboard==1.15.0<br>
* tensorboard-logger==0.1.0<br>
* tensorflow==1.15.2<br>
* tensorflow-estimator==1.15.1<br>
* torch==1.6.0 &#8594; **1.7.1+cu110**<br>
* torchvision==0.7.0 &#8594; **0.8.2+cu110**<br>
* tqdm==4.31.1<br>

### Setup procedure
1. Create a new virtual environment with python 3.6.<br>
2. Install PyTorch and Torchvision on the environment by writing the following code.<br>
~~~
pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 -f https://download.pytorch.org/whl/torch_stable.html
~~~
3. Lastly, set up other libraries except for PyTorch and Torchvision using [requirements.txt](https://github.com/Pilhyeon/Learning-Action-Completeness-from-Points/blob/main/requirements.txt) of official LACP code<br>
---

## Reproduction results
||Paper|GTX 1080TI|RTX TITAN|RTX 3090_a|RTX 3090_b|A6000_a|A6000_b|
|----------------|----------------|----------------|----------------|----------------|----------------|----------------|----------------|
|mAP@0.1|75.7|76.1|75.8|75.4|75.4|75.2|75.2|
|mAP@0.2|71.4|71.5|71.3|70.8|70.8|70.6|70.6|
|mAP@0.3|64.6|65.6|64.8|64.7|64.7|64.4|64.4|
|mAP@0.4|56.5|56.5|56.4|56.6|56.6|56.2|56.2|
|mAP@0.5|45.3|44.4|44.9|45.4|45.4|44.8|44.8|
|mAP@0.6|34.5|33.7|34.4|34.4|34.4|34.6|34.6|
|mAP@0.7|21.8|19.6|21.4|20.4|20.4|20.3|20.3|
|average_mAP[0.1:0.5]|62.7|62.8|62.7|62.6|62.6|62.2|62.2|
|average_mAP[0.3:0.7]|44.5|43.9|44.4|44.3|44.3|44.1|44.1|

*a, b means different servers with the same graphic card.

## References
* Pilhyeon Lee and Hyeran Byun. Learning Action Completeness from Points for Weakly-supervised Temporal Action Localization. In IEEE/CVF International Conference on Computer Vision, 2021.<br>
* https://github.com/Pilhyeon/Learning-Action-Completeness-from-Points<br>
* https://docs.nvidia.com/deploy/cuda-compatibility/index.html<br>
* https://pytorch.org/get-started/previous-versions/<br>
* https://velog.io/@somnode/gpu-cuda-driver-tensorflow-pytorch-version-compatibility<br>
## Citation
@misc{lacpenvs2022,  
author = "Jaeoh Lee, Jaebin Lim, and Chanho Jung",  
title = "LACP code implementation issue about CUDA version",  
howpublished = {\url{https://github.com/workjo/LACP_implementation_compatibility}},  
year = "2022"
}

**수정중...**
