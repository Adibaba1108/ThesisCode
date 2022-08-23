# ThesisCode


##  Dependencies and Installation

- Python >= 3.7 
- PyTorch >= 1.7
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

     Then modify the content in the option file `options/train_realesrnet_x4plus.yml` accordingly:
    ```yml
    train:
        name: DF2K
        type: Dataset
        dataroot_gt: datasets/DF2K 
        meta_info: ThesisCode/meta_info/meta_info_DF2Kmultiscale+OST_sub.txt
        io_backend:
            type: disk
    ```
   1. For Validation: 1. 
   Uncomment those lines and modify accordingly:
    ```yml
      # Uncomment these for validation
      # val:
      #   name: validation
      #   type: PairedImageDataset
      #   dataroot_gt: path_to_gt
      #   dataroot_lq: path_to_lq
      #   io_backend:
      #     type: disk

    ...

      # Uncomment these for validation
      # validation settings
      # val:
      #   val_freq: !!float 5e3
      #   save_img: True

      #   metrics:
      #     psnr: # metric name, can be arbitrary
      #       type: calculate_psnr
      #       crop_border: 4
      #       test_y_channel: false
    ```
    
2.Secondly use the trained Real-ESRNet model as an initialization of the generator, and I have train thesis model with a combination of L1 loss, perceptual loss and GAN loss.
     After the training of Real-ESRNet, there will be a file `experiments/train_RealESRNetx4plus_1000k_B12G4_fromESRGAN/model/net_g_1000000.pth`. If one need to specify the pre-trained path to other files, modify the `pretrain_network_g` value in the option file `train_realesrgan_x4plus.yml`.
2. Modify the option file `train_realesrgan_x4plus.yml` accordingly. Most modifications are similar to those listed above.

## Dataset Preparation

I have used DF2K (DIV2K and Flickr2K) 

You can download from :

1. DIV2K: http://data.vision.ee.ethz.ch/cvl/DIV2K/DIV2K_train_HR.zip
2. Flickr2K: https://cv.snu.ac.kr/research/EDSR/Flickr2K.tar

One of the things done is to generate multi-scale images

For the DF2K dataset, I have use a multi-scale strategy, *i.e.*, we downsample HR images to obtain several Ground-Truth images with different scales. <br>
We can use the [scripts/generate_multiscale_DF2K.py](scripts/generate_multiscale_DF2K.py) script to generate multi-scale images. <br>

## Trained Model

We can use the trained model that I have put it out in the release section,to see the performance of the model.
[Trained Model] (https://github.com/Adibaba1108/ThesisCode/releases/download/v1.0.0/Trained_Thesis_Model_x4plus.pth)



