
# Pediatric Abdominal Radiotherapy Auto-Contouring

This repository contains resources for deep learning-based auto-contouring of organs/structures-at-risk for pediatric abdominal radiotherapy. The repository includes: 

1. **nnUNet Plan**: Configuration files for training the models.
2. **Best Model Weights**: Pre-trained model weights for inference.
3. **Postprocess Files**: Scripts and settings for post-processing model predictions.
4. **Example of Inference Testing**: Scripts and examples for running inference on a public dataset.






## How to Use

### 1. Setup the Environment

First, create a Conda environment and install the necessary dependencies, including nnUNetv2. Please follow the [nnUNetv2 guidelines](https://github.com/MIC-DKFZ/nnUNet).

```bash
conda create -n pedseg python=3.10
conda activate pedseg
# We used nnUNetv2 ==v2.2 for model training. If you want to use a newer version of nnUNetv2, it is also okay.
pip install nnunetv2
```

### 2. Download Files and Prepare Data

Download all files in this repository to your working directory. Prepare your data by converting it into a suitable format, such as NIfTI file format (\`.nii.gz\`).

To convert DICOM files to NIfTI, you can use the provided script:

```bash
pip install SimpleITK
python code/dicom2nii.py -i example_data/example_data_dicom -o example_data/example_data_nifti
```

### 3. Run Predictions

Set the necessary environment variables:
```bash
export nnUNet_raw="./nnUNet/nnUNet_new/nnUNet_raw"
export nnUNet_preprocessed="./nnUNet/nnUNet_new/nnUNet_preprocessed"
export nnUNet_results="./nnUNet/nnUNet_new/nnUNet_results"
```bash

To run inference:

```bash
nnUNetv2_predict -d Dataset001_CombinedData -i example_data/example_data_nifti -o nnUNet_prediction/example_data -f 1 -c 3d_fullres -p PlanfromUMCU -chk checkpoint_best.pth
```bash

### 4. Run Postprocessing (Optional)

To apply postprocessing using the \`postprocessing.pkl\` file (based on our validation set):

```bash
nnUNetv2_apply_postprocessing -pp_pkl_file ./nnUNet/nnUNet_results/Dataset001_CombinedData/nnUNetTrainer__nnUNetPlans__3d_fullres/fold_0/validation/postprocessing.pkl -i nnUNet_prediction/no_postprocess -o nnUNet_prediction/with_postprocess
```






## Citation

If you use this repository, please cite the following:

**Our Paper**:
[Include your paper's citation here]

**nnUNet**:
Isensee, F., Jaeger, P. F., Kohl, S. A., Petersen, J., & Maier-Hein, K. H. (2021). nnU-Net: a self-configuring method for deep learning-based biomedical image segmentation. *Nature Methods, 18*(2), 203-211.

**Pediatric-CT-SEG**:
Jordan, P., Adamson, P. M., Bhattbhatt, V., Beriwal, S., Shen, S., Radermecker, O., ... & Schmidt, T. G. (2022). Pediatric chest-abdomen-pelvis and abdomen-pelvis CT images with expert organ contours. *Medical Physics, 49*(5), 3523-3528.





## Discussion

We'd love to hear about your experience and results using this model. Please share your findings in the discussion section.
