# SU-FOCALID: Sequence-to-Sequence Unsupervised Domain Adpatation with Focal On Imbalance Distribution in Scene Text Recognition.
Author: Hung Tran Tien, Thanh Duc Ngo

Scene Text Recognition has always been a popular interest in Computer Vision field. However, due to the lack of real-world data and labelling process is time-consuming, training a recognition model is difficult. With the help of synthetic data, many proposals have been introduced with promissing results. 
On the other hand, recognizing scene text images still faces challenges due to domain shift between synthetic and real-world domains. Moreover, heavily imbalanced character-level distribution can damages the training process and affects on the model performance. To tackle this problem, we introduced an unsupervised domain adaptation method by minimizing latent representation entropy using focal mechanism. Our proposal outperform other UDA methods with SOTA results on official scene text recognition benchmarks.

## Overview
The proposed SU-FOCALID (Sequence-to-Sequence Unsupervised Domain Adpatation with Focal On Imbalance Distribution in Scene Text Recognition.) is an UDA method with minimizing latent representation entropy for scene text recognition.

## Installation
- building environment: ```cuda==11.0, python==3.7.10```

- install requirements: ```pip3 install torch==1.2.0 pillow==6.2.1 torchvision==0.4.0 opencv-python scipy lmdb nltk natsort```
## Dataset
Training datasets is [here](https://drive.google.com/drive/folders/192UfE9agQUMNq6AgU3_E05_FcPZK4hyt), comprised of both synthetic and real-world datasets:
* Synthetic data is an union of: **MJSynth(MJ)** and **SynthText(ST)**.
* Real-world data is an union of: **IC13, IC15, IIIT5k and SVT**.
* Offical benchmarks evaluation: including **IIIT5k, SVT, IC03, IC13, IC15, SVTP, CUTE**.

## Training
**Pretrained STR framework:**

SU-FOCALID is based on TPS-ResNet-BiLSTM-Attn structure, pretrained model can be obtained [here](https://drive.google.com/drive/folders/15WPsuPJDCzhp2SvYZLRj8mAlT3zmoAMW).

**SU-FOCALID pretrained model:**

Will be updated soon.

* Add pretrained model to ```pretrained/```.
```
CUDA_VISIBLE_DEVICES=1 python train_focalid.py 
      --Transformation TPS --FeatureExtraction ResNet \ --SequenceModeling BiLSTM --Prediction Attn \
	--src_train_data data_lmdb_release/training \
	--src_select_data MJ-ST \
	--tar_train_data data_lmdb_release/validation \
	--tar_select_data real_data \
	--tar_batch_ratio 1.0 \
	--valid_data data_lmdb_release/evaluation/IC15_1811 \
	--batch_size 64 --lr 1 \
	--workers 16 \
	--num_iter 30000 \
	--continue_model pretrained/pretrained.pth \
	--init_portion 0.0 --add_portion 0.00005 \
	--loss_type FocalLoss --entropy_type FE \
	--src_lambda 1.0 --tar_lambda 1.0 \
	--focal_alpha 0.2 --focal_gamma 2 --loss_focal_gamma 2
```
## Evaluation
```
CUDA_VISIBLE_DEVICES=1 python test.py 
      --Transformation TPS --FeatureExtraction ResNet --SequenceModeling BiLSTM --Prediction Attn \
      --eval_data data_lmdb_release/evaluation \
      --benchmark_all_eval \
      --batch_size 128 \
      --workers 16 \
      --saved_model pretrained/pretrained.pth
```
## Acknowledgement
The project is based on [deep-text-recognition-benchmark](https://github.com/clovaai/deep-text-recognition-benchmark), [Seq2SeqAdapt](https://github.com/AprilYapingZhang/Seq2SeqAdapt) and [SMILE](https://github.com/timtimchang/SMILE)
