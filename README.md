# Quality assement of neuroimaging data of the Hamburg City Health Study

## Introduction

This Wiki gives an overview of the quality assessment (QA) conducted for neuroimaging data obtained within the HCHS.
For each MRI modality several levels of QA - from raw data to derived quantitative measures - have been performed.

## Overview of QA Levels
* DICOM to Nifti conversion
* Raw data
* Pipeline outputs

## QA workflow
QA of MRI data was conducted both quantitatively and qualitatively. First, a trained neuroradiologists (board certified / "Facharzt") reviewed and documented all brain imaging data for pathologies (incidental findings). Next, DICOMs were converted to Nifti format with dcm2niix (https://github.com/rordenlab/dcm2niix) eliminating acquisitions with inconsistent volume data. Further, on the raw data level, quantitative QA measures were derived for T1w, DWI, ASL and fMRI data utilizing *MRIQC* (https://mriqc.readthedocs.io/en/stable/), *QSIprep* (https://qsiprep.readthedocs.io/en/latest/), *ASLprep* (https://aslprep.readthedocs.io/en/latest/) and *fMRIprep* (https://fmriprep.org/en/stable/). Qualitative QA of raw imaging data was subsequently performed for outliers defined by +/- 2 standard deviation from the mean for each quantitative measure described below. The quality of all FLAIR images was assessed visually. Last, derivatives of neuroimaging pipelines were assessed in order to ensure appropriate processing. Specifically, white matter hyperintensity segmentations were visually inspected (all outliers and 50 random subjects per quartile), perivascular space segmentations were visually inspected (all outliers and 50 random subjects per quartile), *freesurfer* segmentations of outliers were visually inspected, for the *psmd* pipeline registration quality was visually assessed for outliers, for the *tbss* pipeline registration quality was assessed for all subjects, for the *fba* pipeline all fiber orientation distribution maps were visually inspected, for the *ASLprep* pipeline all visual quality reports including cerebral blood flow maps were assessed.

## Overview of quantitative QA measures used
* T1w: Normalized product of IQR and quartic mean Z-score (*CAT12*), coefficient of joint variation (*MRIQC*), entropy-focus criterion (*MRIQC*), Dietrich signal-to-noise ratio (*MRIQC*), contrast-to-noise ratio (*MRIQC*), residual partial volume effects of gray matter (*MRIQC*)
   * *CAT12* (segmentations and surfaces): Normalized product of IQR and quartic mean Z-score   
   * *freesurfer*: BrainSegVolNotVentSurf, lh/rh.aparc.thickness, Left/RightCerebellumWhiteMatter, SupraTentorialVolNotVent, lh/rh_lateraloccipital_thickness
* DWI: frame-wise displacement (*QSIprep*) and number of bad slices (*DSI Studio*)
   * *psmd*: global psmd
   * *freewater*: FW, FA-t
* ASL: frame-wise displacement (*ASLprep*), mean gray matter cerebral blood flow
* fMRI: frame-wise displacement (*fMRIprep*)

## Overview of qualitative QA
* All sequences: neuroradiological reading by trained neuroradiologist
* FLAIR: visual inspection of raw data
   * *wmh*: visual inspection
* DWI: visual inspection of QSIprep visual reports
   * *freewater*/*tbss*: visual inspection of registration to template/MNI space
   * *psmd*: visual inspection of registration to template/MNI space
   * *fba*: visual inspection of fiber orientation distributions
* T1w: visual inspection of raw data, visual inspection of registration to MNI space
   * *pvs*: visual inspection
* ASL: visual inspection of *ASLprep* visual reports

## HCHS export comparison: benchmark metrics for overlapping subjects (current 17k vs. previous 10k)
The release of the most recent HCHS data export marks a significant update, as all underlying data has been reprocessed using our latest analytical pipelines. This comprehensive reprocessing was undertaken to incorporate newly released participant data and substantial optimizations to the neuroimaging pipelines we employ. These updates include version updates, bug fixes, and performance enhancements, ensuring that the current data benefits from the most advanced and reliable processing techniques available.

To transparently assess the impact of these software and pipeline modifications, this section provides a detailed comparative analysis. We focus specifically on the cohort of subjects who are present in both the current HCHS export (representing approximately 17,000 individuals, 4,400 with MRIs) and the immediately preceding export (encompassing around 10,000 individuals, 2,600 with MRIs). The shown regression plots compare benchmark metrics we frequently analyze in our projects. In sum, the metrics were highly consistent between exports.


![estimated_total_intracranial_volume_scatter_plot](https://github.com/user-attachments/assets/0cc6aeff-71db-4d81-8f22-20ebcfd7a876)

![brain_volume_scatter_plot](https://github.com/user-attachments/assets/bdf98020-8860-4303-baee-cd129a7f2e0f)

![brain_volume_normalized_scatter_plot](https://github.com/user-attachments/assets/bfcdf346-4c11-481b-aaf9-987dcce27c1b)

![mean_cortical_thickness_scatter_plot](https://github.com/user-attachments/assets/dfa44710-3e20-45ab-ac04-5ed3f81e7154)

![psmd_scatter_plot](https://github.com/user-attachments/assets/9f1e74d2-c302-4ee3-ae0d-55b02e2fa0f9)

![wmh_volume_scatter_plot](https://github.com/user-attachments/assets/944bdc13-35da-44ef-9c9e-f1f7f52961ae)

![wmh_load_scatter_plot](https://github.com/user-attachments/assets/4333c8a7-bd56-4c28-b64a-eb905dd4659c)

![pvs_count_scatter_plot](https://github.com/user-attachments/assets/6ceeed98-5bdf-4b1d-b5fa-3373d7278c75)





## QA Evaluation WMH sgementation accuracy
In a subcohort of randomly selected 100 participants covering the full spectrum of WMH extend on FLAIR, WMH were segemented manually by two raters (neurologists) experienced in MRI data analysis in CSVD. Quality and accuracy of manual segmentation was checked visually by an additional junior neurologist and senior neurologist (> 10 years experience in neuroimging in cerebrovascular diseases) and manuall corrections performed if neccessary after consensus between all raters. We then calculatd the overlap with the N=100 manual segmentations using the DICE and JACCARD index and calculated the absolute difference in wmh volume. 

### WMH volumes [ml] for N=100 participants with manual WMH segmentations, grouped by quartiles.

![image](https://user-images.githubusercontent.com/50658271/179002525-24c573fd-5de0-4086-8028-4aef437857b1.png)

### Accuracy evaluation of automated segmentations for N=100 participants with manual segmentations grouped by quartiles of WMH volumes

![image](https://user-images.githubusercontent.com/50658271/179017520-7023703f-d877-47f7-979d-fcdba6dfe6a9.png)

In accordance with the plots above, these are the dice indices per quartile:

![image](https://user-images.githubusercontent.com/50658271/179003051-d951000c-c906-4b40-b4db-3eab45420161.png)

... and the jaccard indices per quartile:

![image](https://user-images.githubusercontent.com/50658271/179003176-bc83abc0-571d-4da0-90c1-c4b3fc5a8e66.png)

... and the absolute volume difference per quartile:

![image](https://user-images.githubusercontent.com/50658271/179017625-cbde36e6-3616-4fd2-894c-5a77a84b838b.png)


##### OVERALL Accuracy statistics
DICE in total:

![image](https://user-images.githubusercontent.com/50658271/181270803-97dc2d2b-c2c9-4321-9fad-430784b2b10b.png)

JACCARD in total:

![image](https://user-images.githubusercontent.com/50658271/181270443-d46ebd21-f731-4f4b-9884-b5fa57b91840.png)

absolute volume difference in total:

![image](https://user-images.githubusercontent.com/50658271/181270335-37072c18-a0de-4eea-98c7-3762b93dffdd.png)

