# Reproducing Keep your Eyes on the Lane: Real-time Attention-guided Lane Detection


This code is tested with following system configuration
### System Configuration
- CPU : i7-10750H 
- CUDA Version: 11.4
- GPU : RTX 2080 (8Gb)
- Python=3.8

### Installation
Install Anaconda (if it does not already exist). Create conda environment and follow these installation instructions.



```bash
conda create -n EEE511 python=3.8
conda activate EEE511
```
```bash
pip install -r requirements.txt
cd lib/nms
python setup.py install
cd ../..
```
### Download Datasets
```bash
cd datasets
```
#### Tempe Dataset
```bash
wget https://www.dropbox.com/s/9ywz0yx5unt1ab7/tempe.zip
unzip tempe.zip && rm tempe.zip
```
#### TuSimple Dataset
```bash
# train & validation data (~10 GB)
wget "https://s3.us-east-2.amazonaws.com/benchmark-frontend/datasets/1/train_set.zip"
unzip train_set.zip -d tusimple
# test images (~10 GB)
mkdir tusimple-test
wget "https://s3.us-east-2.amazonaws.com/benchmark-frontend/datasets/1/test_set.zip"
unzip test_set.zip -d tusimple-test
# test annotations
wget "https://s3.us-east-2.amazonaws.com/benchmark-frontend/truth/1/test_label.json" -P tusimple-test/
cd ..
```

#### CuLane Dataset
```bash
# train & validation images (~30 GB)
gdown "https://drive.google.com/uc?id=1AQjQZwOAkeBTSG_1I9fYn8KBcxBBbYyk"
gdown "https://drive.google.com/uc?id=1PH7UdmtZOK3Qi3SBqtYOkWSH2dpbfmkL"
gdown "https://drive.google.com/uc?id=14Gi1AXbgkqvSysuoLyq1CsjFSypvoLVL"
tar xf driver_23_30frame.tar.gz
tar xf driver_161_90frame.tar.gz
tar xf driver_182_30frame.tar.gz
# test images (~10 GB)
gdown "https://drive.google.com/uc?id=1LTdUXzUWcnHuEEAiMoG42oAGuJggPQs8"
gdown "https://drive.google.com/uc?id=1daWl7XVzH06GwcZtF4WD8Xpvci5SZiUV"
gdown "https://drive.google.com/uc?id=1Z6a463FQ3pfP54HMwF3QS5h9p2Ch3An7"
tar xf driver_37_30frame.tar.gz
tar xf driver_100_30frame.tar.gz
tar xf driver_193_90frame.tar.gzt
# all annotations (train, val and test)
gdown "https://drive.google.com/uc?id=1QbB1TOk9Fy6Sk0CoOsR3V8R56_eG6Xnu"
tar xf annotations_new.tar.gz
gdown "https://drive.google.com/uc?id=18alVEPAMBA9Hpr3RDAAchqSj5IxZNRKd"
tar xf list.tar.gz
```

#### Download Pretrained Models by Authors
```bash
gdown "https://drive.google.com/uc?id=1R638ou1AMncTCRvrkQY6I-11CPwZy23T" # main experiments on TuSimple, CULane and LLAMAS (1.3 GB)
unzip laneatt_experiments.zip && rm laneatt_experiments.zip
```
#### Download Trained Models by Team 13
```bash
wget https://www.dropbox.com/sh/6zt0li4juw4ki5v/AAA8IDBlU_JOPBgqzVJRTxtQa?dl=0
unzip our_experiments.zip && rm our_experiments.zip
```
#### Training
```
python main.py train --exp_dir example --exp_name example --cfg example.yml
```
For example, to train LaneATT with the ResNet-34 backbone on TuSimple, run:
```
python main.py train --exp_dir our_experiments --exp_name laneatt_r34_tusimple --cfg cfgs/laneatt_tusimple_resnet34.yml
```
#### Testing:

```
python main.py test --exp_dir example --exp_name example --view all
```
For example, to evaluate our trained model for LaneATT with the ResNet-34 backbone on TuSimple, run:
```
python main.py train --exp_dir our_experiments --exp_name laneatt_r34_tusimple --view all
```
This command will evaluate the model saved in the last checkpoint of the experiment `example` (inside `our_experiments`).
Set `--exp_dir` to `experiments` for evaluating pre-trained models by authors.
#### Testing on Tempe Dataset:

For example, to evaluate LaneATT with the ResNet-34 backbone on Tempe Dataset, run:
```
python main.py train --exp_dir our_experiments --exp_name laneatt_r34_culane  --cfg cfgs/laneatt_tempe_resnet34.yml --view all
```
This command will load the model weights trained with ResNet-34 backbone on CuLane Dataset.

#### Ablation Study with No Attention Mechanism:

rename the file `lib/models/laneatt_noatt.py` >> `lib/models/laneatt.py`
```
python main.py train --exp_dir our_experiments --exp_name lanenoatt_r18_culane  --cfg cfgs/laneatt_culane_resnet18.yml
```
This command will train the no-attention model with ResNet-18 backbone on CuLane Dataset.