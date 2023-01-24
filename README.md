# Slice Transformer and Self-supervised Learning for 6DoF Localization in 3D Point Cloud Maps

This repository is for transformer based self-supervised localization method and dataset introduced in the following paper
Muhammad Ibrahim,  Naveed Akhtar,  [Saeed Anwar](https://saeed-anwar.github.io/), Michael Wise and  Ajmal Mian, "Slice Transformer and Self-supervised Learning for 6DoF Localization
in 3D Point Cloud Maps", IEEE International Conference on Robotics and Automation (ICRA), 2023

## Contents
1. [Introduction](#introduction)
2. [Requirements](#requirements)
3. [Super-resolution](#super-resolution)
4. [Rain-Removal](#rain-removal)
5. [JPEG-Compression](#jpeg-compression)
6. [Real-Denoising](#real-denoising)
7. [Citation](#citation)
8. [Acknowledgements](#acknowledgements)

## Introduction
Precise localization is critical for autonomous vehicles. We present a self-supervised learning method that employs Transformers for the first time for the task of outdoor localization using LiDAR data. We propose a pre-text task that reorganizes the slices of a 360∘ LiDAR scan to leverage its axial properties. Our model, called Slice Transformer, employs multi-head attention while systematically processing the slices. To the best of our knowledge, this is the first instance of leveraging multi-head attention for outdoor point clouds. We additionally introduce the Perth-WA dataset, which provides a large-scale LiDAR map of Perth city in Western Australia, covering ∼4km2 area. Localization annotations are provided for Perth-WA. The proposed localization method is thoroughly evaluated on Perth-WA and Appollo-SouthBay datasets. We also establish the efficacy of our self-supervised learning approach for the common downstream task of object classification using ModelNet40 and ScanNN datasets. The code and Perth-WA data will be publicly released. 


## Requirements
- PyTorch 0.4.0, PyTorch 0.4.1 
- Tested on Ubuntu 14.04/16.04 environment 
- torchvision=0.2.1
- python 3.6
- CUDA 9.0 
- cuDNN 5.1 
- imageio
- pillow
- matplotlib
- tqdm 
- scikit-image

---

## Super-resolution
The architecture for super-resolution.

<p align="center">
  <img width="700" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-SR.png">
  <img width="700" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/EAM.png">

</p>

### SR Test
1. Download the trained models and code of our paper from [here](https://drive.google.com/file/d/1yDN4ntb1ozICBWXs24NgG9t92N1i-w_W/view?usp=sharing). The total size for all models is **240MB.**

2. cd to '/R2NetSRTestCode/code', either run **bash TestR2NET_2x.sh** or  **bash TestR2NET_3x.sh** or **bash TestR2NET_4x.sh.** 

**or run the following individual commands and find the results in directory **R2NET_SRResults**. 

    **You can use the following script to test the algorithm.**

``` #2x
CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --scale 2 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_BIX2.pt --test_only --save_results --chop --save 'R2NET_Set5' --testpath ../LR/LRBI --testset Set5

CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --scale 2 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_BIX2.pt --test_only --save_results --chop --self_ensemble --save 'R2NETplus_Set5' --testpath ../LR/LRBI --testset Set5

#3x
CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --scale 3 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_BIX3.pt --test_only --save_results --chop --save 'R2NET_Set14' --testpath ../LR/LRBI --testset Set14

CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --scale 3 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_BIX3.pt --test_only --save_results --chop --self_ensemble --save 'R2NETplus_Set14' --testpath ../LR/LRBI --testset Set14

#4x

CUDA_VISIBLE_DEVICES=5 python main.py --data_test MyImage --scale 4 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_BIX4.pt --test_only --save_results --chop --save 'R2NET_B100' --testpath ../LR/LRBI --testset BSD100

CUDA_VISIBLE_DEVICES=5 python main.py --data_test MyImage --scale 4 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_BIX4.pt --test_only --save_results --chop --self_ensemble --save 'R2NETplus_B100' --testpath ../LR/LRBI --testset BSD100

```

### SR Results
**All the results for SuperResolution R<sup>2</sup>Net can be downloaded from**  [SET5](https://drive.google.com/file/d/1-bTKUvRLHoUf8nZTbL4Hw0_-MJUGjSub/view?usp=sharing) (2MB), [SET5+](https://drive.google.com/file/d/1B4JxIV7OZyLKBle9Yg7zz0l-zIK7AOyb/view?usp=sharing) (2MB), [SET14](https://drive.google.com/file/d/10YEHHlAI1jQT-yqB8md9ta1k51bxpwrk/view?usp=sharing) (12.5MB), [SET14+](https://drive.google.com/file/d/1p34F6DsPi7dVPtASieshnpQaW7S8EnOa/view?usp=sharing) (12MB), [BSD100](https://drive.google.com/file/d/1_Oivg1pwTX8uKhjF_TG-fDfHs0weJDWy/view?usp=sharing) (60MB), [BSD100+](https://drive.google.com/file/d/1NsnpsuxJg8tsiFGOE96nVopvfM6603by/view?usp=sharing) (60MB), [Urban100](https://drive.google.com/file/d/17TRk3Gkqax70jBC8eKYNA-KVinPkEJ25/view?usp=sharing) (315MB), and [Urban100+](https://drive.google.com/file/d/17t9MVVBZDKdYCXrbxVhgLdif4Vutu88O/view?usp=sharing) (308MB). 

#### Visual Results

The visual comparisons for 4x super-resolution against several state-of-the-art algorithms on an image from Urban100 dataset. Our R<sup>2</sup>Ne results are the most accurate.
<p align="center">
  <img width="800" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-SR-visual.png">
</p>

#### Quantitative Results

Mean PSNR and SSIM of the denoising methods evaluated on the real images dataset
<p align="center">
  <img width="900" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-SR-psnr.png">
</p>
The performance of super-resolution algorithms on Set5, Set14, BSD100, and URBAN100 datasets for upscaling factors of 2, 3, and 4.
The bold highlighted results are the best on single image super-resolution.

---

## Rain Removal
The architecture for Rain Removal and the subsequent restoration tasks. There are two modifications: the change in position of long skip connection and removal of upsampling
layer.

<p align="center">
  <img width="700" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-all.png">
</p>

### RainRemoval Test
1. The trained models and code for rain removal can be downloaded from [here](https://drive.google.com/file/d/1mlQgVUA1GDTfLjYDPAf13w1YMBGrKMo5/view?usp=sharing). The total size for all models is **121.5MB.**

2. cd to '/R2NetRainRemovalTestCode/code',  either run **bash TestScripts.sh** or run the following individual commands and find the results in directory **R2NET_DeRainResults**.

    **You can use the following script to test the algorithm.**

``` #Normal
# test_a
CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --noise_g 1 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_RainRemoval.pt --test_only --save_results --save 'R2NET_test_a' --testpath ../rainy --testset test_a

# test_b
CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --noise_g 1 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_RainRemoval.pt --test_only --save_results --save 'R2NET_test_b' --testpath ../rainy --testset test_b
```

### RainRemoval Results
**All the results for  Rain Removal R<sup>2</sup>Net can be downloaded from [here](https://drive.google.com/file/d/1GUJ-2G8rBjeXGaUctHDc6OkkpYmC4T3b/view?usp=sharing) for both DeRain's test_a and test_b datasets.** 

#### Visual Results

The visual comparisons on rainy images. The first figure is showing the plate which is affected by raindrops. Our method is consistent in restoring raindrop affected areas. Similarly, in the second example of a rainy image, the cropped region is showing the road sign affected by raindrops. Our method recovers the distorted colors closer to the ground-truth.
<p align="center">
  <img width="600" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-rain-visual1.png">
  <img width="600" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-rain-visual2.png">
</p>

#### Quantitative Results

<p align="center">
  <img width="500" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-rain-psnr.png">
</p>
The average PSNR(dB)/SSIM from different methods on raindrop dataset.

---

## JPEG Compression
The architecture is same for the rest of restoration tasks.

### JPEG Compression Test
1. Download the trained models and code for JPEG Compression of R<sup>2</sup>Net from [Google Drive](https://drive.google.com/file/d/1sABy-hp60fmJdlk2HxUnat65dxftzR4a/view?usp=sharing). The total size for all models is 43MB.**

2. cd to '/R2NetJPEGTestCode/code', either run **bash TestScripts.sh** or run the following individual commands and find the results in directory **R2Net_Results**.

    **You can use the following script to test the algorithm.**

``` 
# Q10
CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --noise_g 10 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_Q10.pt --test_only --save_results --save 'R2NET_JPEGQ10' --testpath ../noisy --testset LIVE1

# Q20
CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --noise_g 20 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_Q20.pt --test_only --save_results --save 'R2NET_JPEGQ20' --testpath ../noisy --testset LIVE1

# Q30
CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --noise_g 30 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_Q30.pt --test_only --save_results --save 'R2NET_JPEGQ30' --testpath ../noisy --testset LIVE1

# Q40
CUDA_VISIBLE_DEVICES=0 python main.py --data_test MyImage --noise_g 40 --model R2NET --n_feats 64 --pre_train ../trained_models/R2Net_Q40.pt --test_only --save_results --save 'R2NET_JPEGQ40' --testpath ../noisy --testset LIVE1
```

### JPEG Compression Results
**If you don't want to re-run the models and save some computation, then all the results for JPEG Compression R<sup>2</sup>Net can be downloaded from**  [LIVE1](https://drive.google.com/file/d/1TGJgkJoJn6Jhbf0km3YRb_ITos0S3EeU/view?usp=sharing) (51.5MB). 

####  Visual Results

sample images of Monarch and parrot with the artifacts having a quality factor of 20. Our R<sup>2</sup>Net restore texture correctly, specifically the line, as shown in the zoomed version of the restored patch in Monarch images. Moreover, R<sup>2</sup>Net restores the texture accurately on the face of the parrot in the second image.
<p align="center">
  <img width="600" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-jpeg-visual1.png">
   <img width="600" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-jpeg-visual2.png">
</p>

#### Quantitative Results

Average PSNR/SSIM for JPEG image deblocking for quality factors of 10, 20, 30, and 40 on LIVE1 dataset. The best results are in bold.
<p align="center">
  <img width="900" src="https://github.com/saeed-anwar/R2Net/blob/master/Figs/R2Net-jpeg-psnr.png">
</p>

---

## Real Denoising

**The real image denoising can be found [here](https://github.com/saeed-anwar/RIDNet)**

---

## Citation
If you find the code helpful in your resarch or work, please cite the following papers.
```
@inproceedings{Ibrahim2021Slice,
    title = {Slice Transformer and Self-supervised Learning for 6DoF Localization in 3D Point Cloud Maps},
    author = {Ibrahim, Muhammad and Akhtar, Naveed and Anwar, Saeed and Wise, Michael and Mian, Ajmal},
    booktitle={International Conference on Robotics and Automation (ICRA)},
    year={2022},
    organization={IEEE}
}

```
## Acknowledgements
This code is built on [RIDNET (PyTorch)](https://github.com/saeed-anwar/RIDNet)

