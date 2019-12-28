# Knowledge distillation kit for PyTorch

## Requirements
- Python 3.6
- pipenv
- [myutils](https://github.com/yoshitomo-matsubara/myutils)


## How to setup
```
git clone https://github.com/yoshitomo-matsubara/kdkit.git
cd kdkit/
git submodule init
git submodule update --recursive --remote
pipenv install
```

## Examples
### 1. ImageNet
#### 1.1 Download and unzip ImageNet datasets (ILSVRC2012 in this example)
#### 1.2 Execute the following commands
```
wget https://raw.githubusercontent.com/soumith/imagenetloader.torch/master/valprep.sh
mv valpre.sh ${VAL_DIR}
sh valpre.sh
```
#### 1.3 Distill knowledge of ResNet-152
e.g., Teacher: ResNet-152, Student: AlexNet  
a) Use one GPU
```
pipenv run python src/image_classification.py --config config/image_classification/alexnet_from_resnet152.yaml
```  
b) Use multiple GPUs
```
pipenv run python -m torch.distributed.launch --nproc_per_node=${NUM_GPUS} --use_env src/image_classification.py --world_size ${NUM_GPUS} --config config/image_classification/alexnet_from_resnet152.yaml
```
c) Use CPU
```
pipenv run python src/image_classification.py --device cpu --config config/image_classification/alexnet_from_resnet152.yaml
```  
#### 1.4 Top 1 accuracy of student models
| Teacher \\ Student    | AlexNet   | ResNet-18 |  
| :---                  | ---:      | ---:      |  
| Pretrained (no *KD*)  | 56.52     | 69.76     |  
| ResNet-152            | 57.22     | 69.86     |


## References
- [pytorch/vision/references/classification/](https://github.com/pytorch/vision/blob/master/references/classification/)