# FLDetector_pytorch
Unofficial implementation for paper FLDetector: Defending Federated Learning Against Model Poisoning Attacks via Detecting Malicious Clients (KDD2022). Official implementation is [here](https://github.com/zaixizhang/FLDetector) with MXNet framework. 

The code cannot work well with local SGD updates. So run this code with local epoch set to 1 (local_ep = 1) and local batch size set to the same nember of samples in local dataset (local_bs = 500, 600). 

paper FLDetector: Defending Federated Learning Against Model Poisoning Attacks via Detecting Malicious Clients is from [KDD2022](https://dl.acm.org/doi/abs/10.1145/3534678.3539231)

Feel free to contact me if you have any difficulty to run the code in issue.

# Results
The results in this version are a bit different with the results reported in the original paper, especially in Non-iid settings. Please use it discriminately and let me know if there is any problem. Here ASR indicates attack success rate also called backdoor success rate, and Acc indicates accuracy of the main tasks.
|Dataset|Model|Attack|Defence|ASR|Acc|iid|
|  ---- |  ----  |  ----  |  ----  |  ----  | ----  | ---- |
|CIFAR-10|ResNet18|Badnet|No Defence|70.2|80.38|IID|
|CIFAR-10|ResNet18|Badnet|FLDector|4.43|68.54|IID|
|CIFAR-10|ResNet18|Badnet|No Defence|70.53|77.58|Non-IID|
|CIFAR-10|ResNet18|Badnet|FLDector|5.21|64.39|Non-IID|

# Requirement
Python=3.9

pytorch=1.10.1

scikit-learn=1.0.2

opencv-python=4.5.5.64

Scikit-Image=0.19.2

matplotlib=3.4.3

hdbscan=0.8.28

jupyterlab=3.3.2

Install instruction are recorded in install_requirements.sh

# Run
VGG and ResNet18 can only be trained on CIFAR-10 dataset, while CNN can only be trained on fashion-MNIST dataset.

Quick start:
```
python main_fed.py --defence fld --model resnet --dataset cifar --local_ep 1 --local_bs 500 --attack badnet --triggerX 27 --triggerY 27 --epochs 500 --poison_frac 0.5
```
It costs more than 10 GPU hours to run this program.

Detailed settings:

```
python main_fed.py      --dataset cifar,fashion_mnist \
                        --model VGG,resnet,cnn \
                        --attack baseline,dba \
                        --lr 0.1 \
                        --malicious 0.1 \
                        --poison_frac 0.5 \
                        --local_ep 1 \
                        --local_bs 500, 600 \
                        --attack_begin 0 \
                        --defence avg, fldetector, fltrust, flame, krum, RLR \
                        --epochs 500 \
                        --attack_label 5 \
                        --attack_goal -1 \
                        --trigger 'square','pattern','watermark','apple' \
                        --triggerX 27 \
                        --triggerY 27 \
                        --gpu 0 \
                        --save save/your_experiments \
                        --iid 0,1 
```
Images with triggers on attack process and test process are shown in './save' when running. Results files are saved in './save' by default, including a figure and a accuracy record. More default parameters on different defense strategies or attack can be seen in './utils/options'.
