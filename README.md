

This is the official repository for fine-tuning SAM2 on medical images. 

## Installation

Environment Requirements: `Ubuntu 20.04` | Python `3.10` | `CUDA 12.1+` | `Pytorch 2.3.1+`

1. Create a virtual environment `conda create -n sam2 python=3.10 -y` and activate it `conda activate sam2`
2. Install [Pytorch 2.3.1+](https://pytorch.org/get-started/locally/)
3. git clone -b SAM2-In-Med https://github.com/bowang-lab/MedSAM/`
4. Enter the SAM2-In-Med folder `cd SAM2-In-Med` and run `pip install -e .`


## Get Started

### 2D Image Segmentation



### 3D Image Segmentation



### Video Segmentation


## Model fine-tuning for medical image segmentation

### Data preprocessing

Download [SAM checkpoint](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth) and place it at `work_dir/SAM/sam_vit_b_01ec64.pth` .

Download the demo [dataset](https://zenodo.org/record/7860267) and unzip it to `data/FLARE22Train/`.

This dataset contains 50 abdomen CT scans and each scan contains an annotation mask with 13 organs. The names of the organ label are available at [MICCAI FLARE2022](https://flare22.grand-challenge.org/).

Run pre-processing

Install `cc3d`: `pip install connected-components-3d`

```bash
python pre_CT_MR.py
```

- split dataset: 80% for training and 20% for testing
- adjust CT scans to [soft tissue](https://radiopaedia.org/articles/windowing-ct) window level (40) and width (400)
- max-min normalization
- resample image size to `1024x1024`
- save the pre-processed images and labels as `npy` files


### Fine-tune SAM2 on multiple GPUs (Recommend)

The model was trained on five A100 nodes and each node has four GPUs (80G) (20 A100 GPUs in total). Please use the slurm script to start the training process.

```bash
sbatch train_multi_gpus.sh
```

When the training process is done, please convert the checkpoint to SAM's format for convenient inference.

```bash
python utils/ckpt_convert.py # Please set the corresponding checkpoint path first
```

### Fine-tune SAM2 on one GPU

```bash
python train_one_gpu.py
```


## Acknowledgements
- We highly appreciate all the challenge organizers and dataset owners for providing the public dataset to the community.
- We thank Meta AI for making the source code of [SAM2](https://github.com/facebookresearch/segment-anything-2) publicly available.



## Reference

```
@article{MedSAM,
  title={Segment Anything in Medical Images},
  author={Ma, Jun and He, Yuting and Li, Feifei and Han, Lin and You, Chenyu and Wang, Bo},
  journal={Nature Communications},
  volume={15},
  pages={1--9},
  year={2024}
}
```
