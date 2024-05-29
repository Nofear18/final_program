<div align="center">
<h1>Exploring Intrinsic Dimension for Vision-Language Model Pruning</h1>
</div>

Official implementation of [Exploring Intrinsic Dimension for Vision-Language Model Pruning](https://openreview.net/forum?id=xxL7CEWuxz&noteId=dIPRrajDnh). 

:gem: The following picture shows the [Intrinsic Dimension](https://proceedings.neurips.cc/paper/2019/hash/cfcce0621b49c983991ead4c3d4d3b6b-Abstract.html) of various multimodal models.
![Example Image](ID.png)



### :hammer: Installation
The code is tested on `Pytorch==1.11.0`, `cuda==11.5`, and `python==3.9.0`. The dependencies can be installed by running the following command:
 <pre/> conda install --yes --file requirements.txt </pre>

### :cat: Compute the Intrinsic Dimension
* Computing in CPU mode
    ```bash
    python ComputeID.py -n 2000 --Path ID/Blip_coco --cpu
    ```
* Computing in GPU mode
    ```bash
    python ComputeID.py -n 2000 --Path ID/Blip_coco --gpu 0
    ```

### :snowman: Image Caption on the COCO Caption Dataset with BLIP

* Dataset & Annotation

    Download the COCO2014 dataset, unzip it under the `datasets` folder, and accordingly modify the `image_root` in [config](./configs/caption_coco.yaml). Download all-in-one annotations  from [this link](https://drive.google.com/uc?export=download&id=19Vk07K3DbQYa68DipJ4dFNcF0_Br7cmD), unzip it under the `coco/annotation` folder, and accordingly modify the `annotation` in [config](./configs/caption_coco.yaml).


* Compression
  
    Download the uncompressed model from [this link](https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_caption_capfilt_large.pth), put it under the `pretrained` folder, and accordingly modify the `pretrained` in [config](./configs/caption_coco.yaml). For example, when pruning BLIP by 80% and adding Intrinsic Dimension to the importance score metric during pruning, you can run the following command:

    ```bash
    python -m torch.distributed.run  --nproc_per_node=2 --master_port=29505 train_caption.py --final_threshold 0.2 --model_dir coco/PLATON80 --pruner_name PLATON --useID
    ```
* Evaluation
  
    After obtaining the pruned model, put them under the `output` folder, and accordingly modify the `--pretrained` of the scripts . For example, to evaluate the pruned model, you can run the following command: 
    ```bash
    python -m torch.distributed.run  --nproc_per_node=2 --master_port=29505 train_caption.py  --pruner_name PLATON --pruned output/pruned_model_path --evaluate
    ```
  
### :fish: Visual Reasoning on the NLVR2 Dataset with BLIP

* Dataset & Annotation

    Download the NLVR2 dataset, unzip it under the `datasets` folder, and accordingly modify the `image_root` in [config](./configs/nlvr.yaml). Download all-in-one annotations from [this link](https://drive.google.com/uc?export=download&id=19Vk07K3DbQYa68DipJ4dFNcF0_Br7cmD), unzip it under the `nlvr/annotation` folder, and accordingly modify the `annotation` in [config](./configs/nlvr.yaml).

* Compression
  
    Download the uncompressed model from [this link](https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_nlvr.pth), put it under the `pretrained` folder, and accordingly modify the `pretrained` in [config](./configs/nlvr.yaml). For example, when pruning BLIP by 80% and adding Intrinsic Dimension to the importance score metric during pruning, you can run the following command:
    ```bash
    python -m torch.distributed.run  --nproc_per_node=2 --master_port=29505 train_nlvr.py --final_threshold 0.2 --model_dir nlvr/PLATON80 --pruner_name PLATON --useID
    ```
* Evaluation
   After obtaining the pruned model, put them under the `output`folder, and accordingly modify the `--pretrained` of the scripts. For example, to evaluate the pruned model, you can run the following command: 

    ```bash
    python -m torch.distributed.run  --nproc_per_node=2 --master_port=29505 train_nlvr.py --pruner_name PLATON --pruned output/pruned_model_path --evaluate
    ```
    
### :tulip: Acknowledgment
This code is built upon <a href="https://github.com/salesforce/BLIP">BLIP</a> and <a href="https://github.com/QingruZhang/PLATON">PLATON</a>. We thank the original authors for their open-source work.


### Citation
If you find this work useful, please consider citing the corresponding paper:
```bibtex
@inproceedings{
anonymous2024exploring,
title={Exploring Intrinsic Dimension for Vision-Language Model Pruning},
author={Anonymous},
booktitle={Forty-first International Conference on Machine Learning},
year={2024},
url={https://openreview.net/forum?id=xxL7CEWuxz}
}
```

