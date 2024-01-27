# Anti-disrupt

This repository contains the purification step for the paper "Coexistence of Deepfake Defenses: Addressing the Poisoning Challenge" by Jaewoo Park, Leo Hyun Park, Hong Eun Ahn, Taekyoung Kwon accepted in IEEE Access in 2024 and "Robust Training for Deepfake Detection Models Against Disruption-Induced Data Poisoning" by Jaewoo Park, Hong Eun Ahn, Leo Hyun Park, Taekyoung Kwon, accepted in WORLD CONFERENCE ON INFORMATION SECURITY APPLICATIONS(WISA) in 2023.

## Introduction

Deepfake images, which are generated by manipulating or synthesizing visual content using deep learning techniques, pose significant threats to society, economy, and politics. To combat this problem, researchers have developed deepfake detection models that can identify manipulated images. However, these models are vulnerable to data poisoning attacks, where malicious actors introduce disruptive perturbations into genuine images to degrade the accuracy of the detection model.


To address this problem, we propose a novel training framework that purifies the disruptive perturbations from genuine images using a diffusion model. Our approach enables successful deepfake image generation for training and significantly curtails accuracy loss in poisoned datasets.


Our approach differs from traditional deepfake detection model training in two ways. First, we deploy a new refinement step to deal with disturbing real images. Second, we delete disturbing images and only consider refined images in later steps.

**The code in this GitHub repository includes a purification step to deal with disturbing real-world images.**

## Results

We evaluated the performance of our framework in comparison to DiffPure and adversarially trained StarGAN. The results of our purification process yield L2 distances comparable to those of existing purification methods. Furthermore, the distribution of deepfake images produced by our method aligns more closely with the original deepfakes compared to existing methods.

## Usage

To use our purification step, follow these steps:

### 0. Clone this repository and install the required dependencies
```bash
git clone https://github.com/seclab-yonsei/Anti-disrupt.git
cd Anti-disrupt
conda env create --file environment.yaml
conda activate gan-diffusion
```

### 1. Prepare Pretrained StarGAN
First, download the pretrained stargan model and place it at `models/stargan/stargan_celeba_128/models`

The pretrained stargan model can be downloaded from https://drive.google.com/drive/folders/1xcCdfLMcoK86deKX4TzE8TC9NSBxhK9L?usp=sharing



### 2. Split dataset
First, download the CelebA dataset and set the `folder_path` and  `attr_path` of `split_dataset.py`.


`split_dataset.py` divides the CelebA dataset into 3 groups:

- A: data for defense model training
- B: data for detection model training
- C: data for detection model validation



The celebA dataset can be downloaded from https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html.


```bash
python split_dataset.py
```


### 3. Train defense model
Train by setting defense_model_type in `train_CelebA.sh`.
```bash
sh train_CelebA.sh
```

##### Base Parameters list
```bash
CUDA_VISIBLE_DEVICES: which gpu to use
defense_model_type: defense model to learn
attack_type: attack to train
train_noise_var: epsilon value of training attack noise
test_noise_var: epsilon value of validation attack noise
result_dir: result storage location
```

### 4.  Test defense model
Test by setting defense_model_type in `test_CelebA.sh`.
```bash
sh test_CelebA.sh
```

##### Base Parameters list
```bash
gan_type: deepfake generation model (currently only supports StarGAN)
defense_model_type: defense model
defense_noise: attacks used to train the defense model
attack_type: the attack you want to defend against
test_noise_var: epsilon of the attack to defend against
save_image: save image
```


## Conclusion
Our proposed purification framework provides a robust solution to the problem of data poisoning in deepfake detection models. By purifying disruptive perturbations during model training, our approach enables successful deepfake image generation for training and significantly curtails accuracy loss in poisoned datasets. We hope that our work will contribute to the development of more effective and reliable deepfake detection models.

## TODO: Citation
```
@ARTICLE{10399770,
  author={Park, Jaewoo and Park, Leo Hyun and Ahn, Hong Eun and Kwon, Taekyoung},
  journal={IEEE Access}, 
  title={Coexistence of Deepfake Defenses: Addressing the Poisoning Challenge}, 
  year={2024},
  volume={12},
  number={},
  pages={11674-11687},
  keywords={Deepfakes;Biological system modeling;Training;Perturbation methods;Security;Computer crime;Purification;Deepfake;deepfake detection;deepfake disruption;data poisoning;adversarial purification},
  doi={10.1109/ACCESS.2024.3353785}}

```
```
@inproceedings{park2023robust,
  title={Robust Training for Deepfake Detection Models Against Disruption-Induced Data Poisoning},
  author={Park, Jaewoo and Ahn, Hong Eun and Park, Leo Hyun and Kwon, Taekyoung},
  booktitle={International Conference on Information Security Applications},
  pages={175--187},
  year={2023},
  organization={Springer}
}
```


