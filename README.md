# Multimodal_Emotion_Recognition

MultiModal Emotion Recognition using Cross modal Attention module and Contrastive loss

- Data: [KEMDy19](https://nanum.etri.re.kr/share/kjnoh/KEMDy19?lang=ko_KR)
- Modality: Audio, Text

<img src="result/model_figure.png" width=900> 


# Installation
## Experiment setting

- Linux
- Python 3.8.16
- PyTorch 1.13.1 and CUDA 11.7

a. Create a conda virtual environment and activate it.

```shell
conda create -n MER python=3.8
conda activate MER
```

b. Install PyTorch and torchvision following the [official instructions](https://pytorch.org/)

c. Clone this repository.

d. Install requirments.

```shell
pip install -r requirements.txt
```

e. Install DeepSpeed


Please check for install [DeepSpeed official github](https://github.com/microsoft/DeepSpeed)

First you need libaio-dev. please install by

```shell
sudo apt-get install libaio-dev
```

After this, install deepspeed by 

```shell
bash install.sh
```


# Prepare for training

a. Prepare data 

- root_path: original KEMD19 path Ex) /home/ubuntu/data/KEMD_19/
- save_path: save folder, default: ./data/

```shell
python preprocess.py --root_path your_KEMD_19_path --save_path ./data/
```

Here is the preprocess flow chart.

<img src="result/preprocessing.png" width=600> 

Note that, wav_length cliping is conducted in train_hf.sh or inference.py 


# Train 

Run Training code

```shell
bash train_hf.sh
```

Check your GPU, and change train_hf.sh and configs properly.

# Tensorboard 

You can run tensorboard 

```shell
tensorboard --logdir ./output/log/tensorboard_what_you_want/version_0/
```

# Inference

First you need to collate the model weights using

```shell
python make_model_weights.py
```

Because this repository use deepspeed stage 2, model weights sharded between gpus. So you need to make sharded checkpoints as one.
After this,

```shell
CUDA_VISIBLE_DEVICES=0 python inference.py
```

# Result

In table, CE means cross entropy and contra means contrastive loss repectively.
Multimodal(concat) represents using concatenate for multimodal modeling and Multimodal(CMA) represents using cross modal attention respectively.

<img src="result/metric.png" width=900> 
