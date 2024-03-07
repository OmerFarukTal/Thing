# Thing

## Directories

### PAP/utils

Where "Per instance" and "UAP" attacks created.

### PAP/supervised

Supervised finetuned models are stored in there.

### PAP/perturbations

100ep model checkpoint UAP perturbations stored in there.

### PAP/perturbations_best

Best model checkpoint UAP perturbations stored in there.

### PAP/finetuning

Finetuning python files are there.

### PAP/datasetplus

Dataset classes are defined.

### PAP/best_model_checkpoints

Checkpoints for SSL models. (BEST)

### PAP/100ep_model_checkpoints

Checkpoints for SSL models (100ep)

## Python Files

### PAP/tools.py

Models are loaded here. 

get_pretrained_imagenet_model() loads the asked model from the checkpoint. 

get_finetuned..() loads the asked model finetuned on given dataset as parameter.

### PAP/eval.py

The file where "Per instance" and "UAP" attacks are tested. Also does the linear evalution on Imagenet. It calls functions from "PAP/tools.py". (Look basic_attack_script.sh)

### PAP/attacks.py

The file which "UAP" attacks are created. It calls functions from "PAP/tools.py". 

### PAP/finetuning/SimCLR/100ep_models_finetune.py

In this file 100ep models are loaded. Here I load them seperately, did not use the functions from the "PAP/tools.py". So, if you fix "PAP/tools.py", we also need to adjust this file.

### PAP/finetuning/SimCLR/best_models_finetune.py

In this file best models are loaded. Same issue as in the "PAP/finetuning/SimCLR/best_models_finetune.py".

## Script Files

They actually have examples in them, to be more explicit:

### PAP/attack_script.sh

To generate "UAP" example. Ex:

python3 ./attacks.py --mode L4A_base --model_name r50_1x_sk0_mocov3 --model_arch mocov3 --save_path ./save_model_attack_ep100/

--mode => attack name. Thhey are written in the arguments


### PAP/basic_attack_script.sh

To generate and assess the "Per Instance" attacks. Ex:

python eval.py --imagenet_test basic_attack --model_name r50_1x_sk0_simclr --model_arch simclr --epsilon 0.00392156862 --basic_attack_name pgd --batch_size 64 --dataset_name imagenet --data_path ../data/

--basic_attack => "yes" or "cross" (Use this as yes)
--attack_name => "fgsm" (You can look at the attacks from the "PAP/utils/basic_attack2.py)

To do linear eval on Imagenet:

python eval.py --imagenet_test raw --model_name r50_1x_sk0_simclr --model_arch simclr --batch_size 64 --dataset_name imagenet --data_path ../data/


### PAP/finetune_script.sh

python3 ./finetuning/SimCLR/best_models_finetune.py --save_path ./best_checkpoints/ --model_name r50_1x_sk0_simclr --dataset pascal_voc --data_path ./data/pascal_voc/ 


--dataset_name (You can find the dataset names in the "./finetuning/SimCLR/best_models_finetune.py")

data_dict = {'cifar100':100,'cifar10':10,'imagenet':1000,'flowers':102,'cars':196,'food':101,'DTD':47,'pets':37,'stl10':10,'svhn':10,'cub':200,'fgvc':100}



### PAP/eval_script.sh

To test the "UAP"

python3 eval.py --model_name r50_1x_sk0_simclr --model_arch simclr --uap_path ./perturbations_best/vicreg/r50_1x_sk0_vicreg/asv/save_model_attack_ep100/k=3125.pt

--uap_path (They are stored in one of "PAP/perturbations" (100ep) or "PAP/perturbations_best" (Best))

You can just change the "simclr" parts to any model you want. Then it is good to go.



