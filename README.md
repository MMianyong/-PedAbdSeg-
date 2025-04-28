# -PedAbdSeg-
This repository contains the trained models and training hyperparameters presented in the paper *"Deep learning-based auto-contouring of organs/structures-at-risk for pediatric abdominal radiotherapy."*  

# Deep learning-based auto-contouring of organs/structures-at-risk for pediatric abdominal radiotherapy

The repository provides:

1. **plans.json**: The nnUNetv2 plan file, including all hyperparameters based on our in-house dataset.
2. **model.tar.gz**: The final selected model archive for clinical evaluation.
3. **network_architecture.pdf**: Visualization of the model architecture for the final selected best model.
4. **Supplementary material S4.xlsx**: The grading sheet used for qualitative clinical evaluation by clinicians.

---

## Model
The model is a 3D full-resolution (`3d_fullres`) nnUNetv2 model trained for 1000 epochs. No ensemble was applied. The model  was developed based on 189 in-house pediatric CTs with renal tumors or abdominal neuroblastomas, and 189 publicly available pediatric CTs from the [TCIA Pediatric CT Segmentation dataset](https://www.cancerimagingarchive.net/collection/pediatric-ct-seg/).


### Model labels

The following labels were used during model training and prediction:

```json
"labels": {
    "background": 0,
    "spleen": 1,
    "kidney_right": 2,
    "kidney_left": 3,
    "heart": 4,
    "liver": 5,
    "lung_right": 6,
    "lung_left": 7,
    "pancreas": 8,
    "stomach_bowel": 9,
    "vertebrae": 10,
    "spinal_cord": 11,
    "aorta_abdominal": 12,  # The aorta segmentation stops at the lowest point of the lungs.
    "inferior_vena_cava": 13,
    "autochthon_left": 14,
    "autochthon_right": 15,
    "iliopsoas_left": 16,
    "iliopsoas_right": 17
}
### 1. Setup the Environment


```bash
conda create -n pedseg python=3.10
conda activate pedseg
# We used nnUNetv2 ==v2.2 for model training.
pip install nnunetv2
```

### 2. Download Files and Prepare Data

Download `model.tar.gz` into your working directory and decompress it:
```bash
tar -xzvf model.tar.gz
cd model            
find . -name "*.gz" -exec gunzip {} \;   # unzip all .gz files

```


### 3. Run Predictions

Set the necessary environment variables:
```bash
export nnUNet_raw="./nnUNet_raw"
export nnUNet_preprocessed="./nnUNet_preprocessed"
export nnUNet_results="./nnUNet_results"
```

To run inference:

```bash
nnUNetv2_predict -d Dataset002_TwoSet -i ../folder_with_nifti_files  -o output/no_postprocess -f 1 -c 3d_fullres -p PlanfromUMCU -chk checkpoint_best.pth
```

### 4. Run Postprocessing (Optional)

To apply postprocessing using the \`postprocessing.pkl\` file (based on our validation set):

```bash
nnUNetv2_apply_postprocessing -pp_pkl_file ./nnUNet_results/Dataset002_TwoSet/nnUNetTrainer__PlanfromUMCU__3d_fullres/fold_1/validation/postprocessing.pkl -i output/no_postprocess -o output/with_postprocess
```






## Citation

If you use this repository, please cite the following:


**This work**:  
Ding, M., Littooij, A. S., Maspero, M., Janssens, G. O., & van den Heuvel-Eibrink, M. M. (2025). Deep learning-based auto-contouring of organs/structures-at-risk for pediatric upper abdominal radiotherapy. *Radiotherapy and Oncology*, in press.


**nnUNet**:
Isensee, F., Jaeger, P. F., Kohl, S. A., Petersen, J., & Maier-Hein, K. H. (2021). nnU-Net: a self-configuring method for deep learning-based biomedical image segmentation. *Nature Methods, 18*(2), 203-211.

**Totalsegmentator**:
Wasserthal, J., Breit, H. C., Meyer, M. T., Pradella, M., Hinck, D., Sauter, A. W., ... & Segeroth, M. (2023). TotalSegmentator: robust segmentation of 104 anatomic structures in CT images. Radiology: Artificial Intelligence, 5(5).

**Pediatric-CT-SEG**:
Jordan, P., Adamson, P. M., Bhattbhatt, V., Beriwal, S., Shen, S., Radermecker, O., ... & Schmidt, T. G. (2022). Pediatric chest-abdomen-pelvis and abdomen-pelvis CT images with expert organ contours. *Medical Physics, 49*(5), 3523-3528.





## Discussion

We would love to hear about your experience and results using this model on your pediatric datasets.
