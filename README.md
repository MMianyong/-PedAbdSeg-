
# Deep learning-based auto-contouring of organs/structures-at-risk for pediatric abdominal radiotherapy 

This repository contains resources for deep learning-based auto-contouring of organs/structures-at-risk for pediatric abdominal radiotherapy. The repository includes: 

1. **nnUNet Plan**: nnUNetplan with all hyperparameters based on our data.
2. **Best Model Weights**: The final selected best model for clinical evaluation.
3. **Postprocess Files**: The generated postprocess file to apply postprocess based on our validation data. 
4. **Example data of Inference Testing**: Example of data for inference from Pediatric-CT-SEG




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
```

To run inference:

```bash
nnUNetv2_predict -d Dataset001_CombinedData -i example_data/example_data_nifti -o nnUNet_prediction/example_data -f 1 -c 3d_fullres -p PlanfromUMCU -chk checkpoint_best.pth
```

### 4. Run Postprocessing (Optional)

To apply postprocessing using the \`postprocessing.pkl\` file (based on our validation set):

```bash
nnUNetv2_apply_postprocessing -pp_pkl_file ./nnUNet/nnUNet_results/Dataset001_CombinedData/nnUNetTrainer__nnUNetPlans__3d_fullres/fold_0/validation/postprocessing.pkl -i nnUNet_prediction/no_postprocess -o nnUNet_prediction/with_postprocess
```






## Citation

If you use this repository, please cite the following:

**Our Paper**:
[Add later]

**nnUNet**:
Isensee, F., Jaeger, P. F., Kohl, S. A., Petersen, J., & Maier-Hein, K. H. (2021). nnU-Net: a self-configuring method for deep learning-based biomedical image segmentation. *Nature Methods, 18*(2), 203-211.

**Pediatric-CT-SEG**:
Jordan, P., Adamson, P. M., Bhattbhatt, V., Beriwal, S., Shen, S., Radermecker, O., ... & Schmidt, T. G. (2022). Pediatric chest-abdomen-pelvis and abdomen-pelvis CT images with expert organ contours. *Medical Physics, 49*(5), 3523-3528.





## Discussion

We'd love to hear about your experience and results using this model. Please share your findings in the discussion section.
