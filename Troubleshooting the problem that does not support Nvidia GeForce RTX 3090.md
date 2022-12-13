

# Troubleshooting the problem that does not support Nvidia GeForce RTX 3090

**I tried to run the official [LACP](https://github.com/Pilhyeon/Learning-Action-Completeness-from-Points) code on Nvidia GeForce RTX 3090 but faced a CUDA compatibility issue.**<br>
해당 논문의 저자가 배포한 소스코드를 Nvidia GeForce RTX 3090 기반 시스템에서 구동시키고자 하였으나 다음과 같은 에러가 발생

![에러이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbubhWo%2FbtrTltOPyFf%2FuDqUqarRJ1iCEotmOwlvOk%2Fimg.png)
(Error message: NVIDIA GeForce RTX 3090 with CUDA capability sm_86 is not compatible with the current PyTorch installation.)<br><br>
**The issue is that recommended each CUDA and PyTorch version of the official LACP code is 10.2 and 1.6, but RTX 3090 is only compatible with CUDA version 11 or higher.<br>
So I attempted to change PyTorch and Torchvision version that supports CUDA version 11 or higher.<br>**
RTX 3090 을 사용하는 경우 최대 CUDA 11.0 까지 지원하는Pytorch v1.7.0 이상을 이용해야 함<br>
저자가 배포한 소스코드 구동을 위한 requirements.txt 를 확인한 결과 Pytorch v1.6.0 을 이용하였음<br>

**Required the environment's dependencies are as below.<br>**
저자가 배포한 소스코드를 Nvidia GeForce RTX 3090 기반 시스템에서 구동시키기 위해 PyTorch와 Torchvision의 버전을 다음과 같이 변경하였음<br><br>
~~torch==1.6.0~~<br>
-> **torch==1.7.1+cu110**<br>
~~torchvision==0.7.0~~<br>
-> **torchvision==0.8.2+cu110**<br><br>
**Install PyTorch and Torchvision on the environment by writing the following command.<br>**
Install하기 위해 다음과 같은 command를 이용할 수 있음<br>
```
pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio==0.7.2 -f https://download.pytorch.org/whl/torch_stable.html
```
**Through the install command, the official LACP code was executed on a system based on RTX 3090. <br>
To study the impact of a PyTorch version update, I ran the official LACP code on TITAN RTX and RTX 3090.<br>
And I compared performances reported on paper and two experimental results.<br>**
Pytorch 버전 변화에 따른 영향을 스터디하기 위해 저자가 배포한 소스코드를 1) TITAN RTX 및 2) RTX 3090 기반 시스템에서 구동시키고 성능 측정<br>
스터디 과정에서 THUMOS'14 데이터셋을 이용하여 성능 재현을 하였고, 논문 성능은 ICCV 2021 논문에서 reporting된 성능을 의미<br>

## Reproduction results
||average_mAP(0.1:0.5)|average_mAP(0.3:0.7)|
|----------------|----------------|----------------|
|Paper|62.7|44.5|
|TITAN RTX|62.5|43.8|
|RTX 3090|62.4|43.9|
**You can download my reproduction medels -> (Link)[https://drive.google.com/drive/folders/1Y0BaRwbALN6-VlHfqfCoPdmeSeEOveIc?usp=sharing]**
## **Conclusion**
1. **I could check to reproduce similar results compared to the performance reported in ICCV 2021 paper.**<br>
ICCV  2021 논문에서 reporting된 성능이 비교적 잘 재현되는 것을 확인<br>
2. **Changing the PyTorch version doesn't significantly influence the results on THUMOS'14 dataset.**<br>
THUMOS'14 데이터셋에 대한 실험 결과 PyTorch 변화에 따른 영향이 크지 않음을 확인<br>
## References
* Pilhyeon Lee and Hyeran Byun. Learning Action Completeness from Points for Weakly-supervised Temporal Action Localization. In IEEE/CVF International Conference on Computer Vision, 2021.<br>
* https://github.com/Pilhyeon/Learning-Action-Completeness-from-Points<br>
* https://docs.nvidia.com/deploy/cuda-compatibility/index.html<br>
* https://pytorch.org/get-started/previous-versions/<br>
* https://velog.io/@somnode/gpu-cuda-driver-tensorflow-pytorch-version-compatibility<br>
