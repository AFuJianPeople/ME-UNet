# ME-UNet

Official repository for "ME-UNet: Enhancing Mamba for Myocardial Pathology Segmentation in Multi-Center Multi-Sequence CMR Images".

## Release
-  **News**: ```2024/11/27```: ME-UNet released.

## Get Start 

Requirements: `CUDA ≥ 11.6`

1. Create a virtual environment: `conda create -n meunet python=3.10 -y` and `conda activate meunet `
2. Install [Pytorch](https://pytorch.org/get-started/previous-versions/#linux-and-windows-4) 2.0.1: `pip install torch==2.0.1 torchvision==0.15.2`
3. Install [Mamba](https://github.com/state-spaces/mamba): `pip install causal-conv1d==1.1.1` and `pip install mamba-ssm`
4. Download code: `git clone https://github.com/AFuJianPeople/ME-UNet`
5. `cd ME-UNet/me-unet` 
6. run `pip install -e .`

## Data Preparation

Download MyoPS++2024 dataset, then put them into the `ME-Unet/data/nnUNet_raw` folder. 
ME-UNet is built on the popular [nnU-Net](https://github.com/MIC-DKFZ/nnUNet) framework. If you want to train ME-UNet on your own dataset, please follow this [guideline](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/dataset_format.md) to prepare the dataset. 

Please organize the dataset as follows:

```
data/
├── nnUNet_raw/
│   ├── Dataset001_Myops/
│   │   ├── imagesTr
│   │   │   ├── Myops_0001_0000.nii.gz
│   │   │   ├── Myops_0002_0000.nii.gz
│   │   │   ├── ...
│   │   ├── labelsTr
│   │   │   ├── Myops_0001.nii.gz
│   │   │   ├── Myops_0002.nii.gz
│   │   │   ├── ...
│   │   ├── dataset.json
│   ├── ...
```

Based on nnUNet, preprocess the data and generate the corresponding configuration files (the generated results can be found in the `ME-Unet/data/nnUNet_preprocessed` folder).

```bash
nnUNetv2_plan_and_preprocess -d DATASET_ID --verify_dataset_integrity
```

## Model Training


### Train 2D models

- Train 2D `ME-Unet` model

```bash
nnUNetv2_train DATASET_ID 2d all -tr nnUNetTrainerMEUNet
```

### Train 3D models

- Train 3D `ME-Unet` model

```bash
nnUNetv2_train DATASET_ID 3d_fullres all -tr nnUNetTrainerMEUNet
```

## Inference

### Inference 2D models

- Inference 2D `ME-Unet` model

```bash
nnUNetv2_predict -i INPUT_FOLDER -o OUTPUT_FOLDER -d DATASET_ID -c 2d -tr nnUNetTrainerMEUNet --disable_tta
```

### Inference 3D models

- Inference 3D `ME-Unet` model

```bash
nnUNetv2_predict -i INPUT_FOLDER -o OUTPUT_FOLDER -d DATASET_ID -c 3d_fullres -tr nnUNetTrainerMEUNet --disable_tta
```

## Citation
If you find our work helpful, please consider citing the papers

## Acknowledgements

We acknowledge all the authors of the employed public datasets, allowing the community to use these valuable resources for research purposes. 
We also thank the authors of [nnU-Net](https://github.com/MIC-DKFZ/nnUNet), [Mamba](https://github.com/state-spaces/mamba) and [U-Mamba](https://github.com/bowang-lab/U-Mamba) for making their valuable code publicly available.

