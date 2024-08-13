# -PedAbdSeg-


This repository contains resources for deep learning-based auto-contouring of organs/structures-at-risk for pediatric abdominal radiotherapy. The repository includes:

1. **nnUNet Plan**: Configuration files for training the models.
2. **Best Model Weights**: Pre-trained model weights for inference.
3. **Postprocess Files**: Scripts and settings for post-processing model predictions.
4. **Example of Inference Testing**: Scripts and examples for running inference on a public dataset.

## How to Use

### 1. Setup the Environment

First, create a Conda environment and install the necessary dependencies, including nnUNetv2. Please follow the [nnUNetv2 guidelines](https://github.com/MIC-DKFZ/nnUNet).

\`\`\`bash
conda create -n pedseg python=3.10
conda activate pedseg
# We used nnUNetv2 ==v2.2 for model training. If you want to use a newer version of nnUNetv2, it is also okay.
pip install nnunetv2
\`\`\`

### 2. Download Files and Prepare Data

Download all files in this repository to your working directory. Prepare your data by converting it into a suitable format, such as NIfTI file format (\`.nii.gz\`).
