# ThesisCode


##  Dependencies and Installation

- Python >= 3.7 (Recommend to use [Anaconda](https://www.anaconda.com/download/#linux)
- [PyTorch >= 1.7](https://pytorch.org/)

## Installation

1. Clone repo

    ```bash
    git clone https://github.com/Adibaba1108/ThesisCode.git
    cd ThesisCode
    ```

1. Install dependent packages

    ```bash
    # Install basicsr - https://github.com/xinntao/BasicSR
    # We use BasicSR for both training and inference
    pip install basicsr
    # facexlib and gfpgan are for face enhancement
    pip install facexlib
    pip install gfpgan
    pip install -r requirements.txt
    python setup.py develop
    ```

---

## How to Train
The training has been divided into two stages. These two stages have the same data synthesis process and training pipeline, except for the loss functions. Specifically,

1. First train Real-ESRNet with L1 loss from the pre-trained model ESRGAN.
Download pre-trained model [ESRGAN] into `experiments/pretrained_models`.
    ```bash
    wget https://github.com/Adibaba1108/ThesisCode/releases/download/v1.0.0/Trained_Thesis_Model_x4plus.pth -P experiments/pretrained_models
    ```
1. Secondly use the trained Real-ESRNet model as an initialization of the generator, and train thesis model with a combination of L1 loss, perceptual loss and GAN loss.

## Dataset Preparation

I have used DF2K (DIV2K and Flickr2K) 

You can download from :

1. DIV2K: http://data.vision.ee.ethz.ch/cvl/DIV2K/DIV2K_train_HR.zip
2. Flickr2K: https://cv.snu.ac.kr/research/EDSR/Flickr2K.tar


