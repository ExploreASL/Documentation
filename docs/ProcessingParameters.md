
# Processing parameters (dataPar.json)

This document describes the ExploreASL internal x-structure that contains processing settings and also sequence parameters. Normally, you do not have to modifies the processing paramaters and the default values are recommended for most studies. Also, you do not have to insert of modify the sequence parameters as these are normally all obtained from the JSON-files for data saved in ASL-BIDS. However, you can change both the processing settings and sequence parameters by providing them in a `dataPar.json` file in the `ROOT\derivatives\ExploreASL\dataPar.json`. Alternatively, you can provide the file at `ROOT\dataPar.json` during the import and it will be automatically copied to the `ROOT\derivatives\ExploreASL\dataPar.json` location. Note that for missing parameters, default will be used. Provided sequence parameters will be ignored if they will be already contained in individual subject's JSON sidecars, and will only be considered when missing for each particular subject.

## List of parameters
Below, we list all useful parameters that can be set by users for both basic and advanced processing. See a full list of parameters in [Developer Tutorial](./../Tutorials-Developer/). Note that the parameters are grouped to several groups by their function - do not forget to add the full hierarchy when creating the `DataPar.json` - see the example at the end of this page. Note that the tables below use **Matlab** notation and the example file is in **JSON** notation

### ENVIRONMENT PARAMETERS
Within `x.external` you can find parameters related to **external tools** and the coding **environment**.

| `x.external.[...]`         | Description                                                                                                                                                         | Defaults                     |
| -------------------------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------:|
| `bAutomaticallyDetectFSL`  | Boolean to automatically detect the FSL version if disabled, this function will try to use the system-initialized FSL and throw an error if FSL is not initialized. | OPTIONAL, DEFAULT = false |

### STUDY PARAMETERS
Basic information about the session names within a study.

| `x.[...]`                            | Description                                                                                 | Defaults           |
| ------------------------------------ |:-------------------------------------------------------------------------------------------:|:------------------:|
| `SESSIONS`                           | Use this to define sessions. Example (`'.json' file): ["ASL_1","ASL_2"]` | OPTIONAL, DEFAULT = `{'ASL_1'}` |
| `session.options`                    | This is how the sessions will be called, example: `{'baseline' 'drug'}`. For FEAST, this should be `{'crushed' 'non-crushed'}`. | OPTIONAL |

### DATASET PARAMETERS
**Dataset** related parameters can be found in this subfield.

| `x.dataset.[...]`            | Description                                                                                                              | Defaults                  |
| ---------------------------- |:------------------------------------------------------------------------------------------------------------------------:|:-------------------------:|
| `subjectRegexp`              | String with regular expression for ExploreASL to find subjects by foldername, example: `^\d{3}$` for three digits        | REQUIRED                  |
| `exclusion`                  | Cell with list of subjects to exclude, example: `{'005' '018'}`                                                          | OPTIONAL, DEFAULT = empty |
| `ForceInclusionList`         | Use this field if you want to use a selection of subjects rather than taking all available subjects from directories. Example: `load(fullfile(x.D.ROOT,'LongitudinalList.mat')`). | OPTIONAL, DEFAULT = use all subjects |

### M0 PARAMETERS and OPTIONS
Parameters to define M0-processing choices and also extra information about M0-scan position within the ASL series if not specified in the ASLcontext.tsv already. Note that these parameters can be added either to the general `dataPar.json` but also to the `ASL4D.json` of each subject to affect the processing of this subject only.

| `x.modules.asl.[...]`                                       | Description                                   | Defaults           |
| -------------------------------------- |:---------------------------------------------:|:------------------:|
| `M0_conventionalProcessing`            | Boolean - use the conventional M0 processing (per consensus paper), options: 1 = standard processing, 0 = new image processing (improved masking & smoothing). | OPTIONAL, DEFAULT = 0 |
| `M0_GMScaleFactor`                     | Add additional scale factor to multiply the M0 image by This can be useful when you have background suppression but no control/M0 image without background suppression. If you then know the M0 scalefactor for the GM, you can use the control image as M0 and use this parameter to scale back what was suppressed by background suppression. Note that there is no option for separate tissue scaling (e.g. WM & GM), because ExploreASL pragmatically smooths the M0 a lot, assuming that head motion and registration between M0 & ASL4D will differ between patients and controls. | OPTIONAL, default = 1 |
| `M0PositionInASL4D`                    | A vector of integers that indicates the position of M0 in TimeSeries, if it is integrated by the Vendor in the DICOM export. Will move this from ASL4D.nii to M0.nii Note that the x.modules.asl.M0PositionInASL4D parameter is independent from the x.Q.M0 parameter choice. Example for Philips 3D GRASE = '[1 2]' (first control-label pair). Example for Siemens 3D GRASE = 1 first image. Example for GE 3D spiral = 2 where first image is PWI & last = M0. Empty vector should be given (= [] or = null (in JSON)) if no action is to be taken and nothing is removed. | OPTIONAL, DEFAULT = `[] (no M0 in timeseries)` |
| `DummyScanPositionInASL4D`             | A vector of integers that indicates the position of Dummy scans in TimeSeries if they are integrated by the Vendor in the DICOM export. This allows to remove the dummy scans or noise scans that are part of the Timeseries. A new ASL4D.nii is saved with dummy scans removed and the original is backed-up. Works in a similar way as M0PositionInASL4D, both can be entered at the same time and both indicate the original position in the Timeseries independend of each other. Example for Siemens 2D EPI = `[79 80]` Skip the control-label pair used for noise measurements. Example for certain Siemens 3D GRASE = 2 Skip the first dummy control image. Empty vector should be given (= [] or = null (in JSON)) if no action is to be taken and nothing is removed. | OPTIONAL, DEFAULT = `[] (no M0 in timeseries)` |

### QUANTIFICATION PARAMETERS
Please use these fields to modify the quantification model parameters for the entire study. Note that normally, the default values are used for each field-strength and these values do not have to be provided. Note that these parameters can be added either to the general `dataPar.json` but also to the `ASL4D.json` of each subject to affect the processing of this subject only.

| `x.Q.[...]`                       | Description                                   | Defaults           |
| ------------------------------------- |:---------------------------------------------:|:------------------:|
| `Lambda`                            | Brain/blood water coefficient (mL 1H/ mL blood). Example: `0.32` (for GSP phantom). | OPTIONAL, DEFAULT = 0.9 |
| `T2art`                             | `T2*` of arterial blood, only used when no M0 image (ms). | OPTIONAL, DEFAULT = 50 @ 3T|
| `BloodT1`                           | T1 relaxation time of arterial blood (ms). Defaults (Alsop MRM 2014), 1800 for GSP phantom. | OPTIONAL, DEFAULT = 1650 @ 3T |
| `TissueT1`                          | T1 relaxation time of GM tissue (ms). Defaults (Alsop MRM 2014). | OPTIONAL, DEFAULT=1240 @ 3T |
| `nCompartments`                     | Number of modeled compartments for quantification. Options: 1 = a single-compartment quantification model (default by concensus paper), 2 = a dual-compartment quantification model. | OPTIONAL, DEFAULT = 1) |

### ASL PROCESSING PARAMETERS
All **ASL module** related parameters are stored within this subfield. Use these for extra ASL-processing options or to turn OFF/ON certain steps. Note that these parameters can be added either to the general `dataPar.json` but also to the `ASL4D.json` of each subject to affect the processing of this subject only.

| `x.modules.asl.[...]`                  | Description                                   | Defaults           |
| -------------------------------------- |:---------------------------------------------:|:------------------:|
| `SaveCBF4D`                         | Boolean, true to also save 4D CBF timeseries, if ASL4D had timeseries. | OPTIONAL, DEFAULT=false |
| `bUseBasilQuantification`           | True for using BASIL quantification in addition to ExploreASL's quantification. |  |
| `bMaskingBASIL`                    | For for BASIL/FABBER to quantify only over a mask provided by ExploreASL | OPTIONAL, DEFAULT = true |
| `bSpatialBASIL`                      | True for BASIL to use automated spatial smoothing |  OPTIONAL, DEFAULT = false |
| `bInferT1BASIL`                      | True for BASIL to infer variable T1 values| OPTIONAL, DEFAULT = false |
| `bInferATTBASIL`                     | True for BASIL to infer arterial component and quantify ATT | OPTIONAL, DEFAULT = true | 
| `ExchBASIL`                         | True for BASIL to model of the exchange of labeled water in the capilliary bed . Options : mix = Well-mixed single compartment, simple = Simple single compartment of T1 of blood, 2cpt = A two compartment exchange model following Parkes & Tofts, spa = Single pass approximation from St. Lawrence | OPTIONAL, DEFAULT = simple |
| `DispBASIL`                         | True for BASIL to model label dispersion. Options : none = No dispersion, gamma = Gamma dispersion kernal (vascular transport function), gauss = Temporal gaussian dispersion kernal, sgauss = Spatial gaussian dispersion kernal | OPTIONAL, DEFAULT = none |
| `ATTSDBASIL`                     | Set parameter standard deviation of ATT for BASIL fitting | OPTIONAL, DEFAULT = 1.0 | 
| `bCleanUpBASIL`                     | Delete temporary files creates for FSL quantification | OPTIONAL, DEFAULT = true | 
| `motionCorrection`                     | Boolean to perform motion correction in case of timeseries. Options: `1` = on, `0` = off. | OPTIONAL, DEFAULT = 1 |
| `SpikeRemovalThreshold`                | Minimal t-stat improval needed to remove motion spikes. Examples: `1` = effectively disabling spike removal. | OPTIONAL, DEFAULT = 0.01 |
| `SpikeRemovalAbsoluteThreshold`        | Motion threshold in mm for removing motion spike volumes. Examples: `0.05`, `0` = disabling spike removal. | OPTIONAL, DEFAULT = 0 |
| `bRegistrationContrast`                | Specifies the image contrast used for registration: `0` = Control->T1w, `1` = CBF->pseudoCBF from template/pGM+pWM (skip if sCoV>0.667), `2` = automatic (mix of both), `3` = option 2 & force CBF->pseudoCBF irrespective of sCoV. | OPTIONAL, DEFAULT = 2 |
| `bAffineRegistration`                  | Specifies if the ASL-T1w rigid-body registration is followed up by an affine registration: `0` = affine registration disabled, `1` = affine registration enabled, `2` = affine registration automatically chosen based on spatial CoV of PWI. | OPTIONAL, DEFAULT = 0 |
| `bDCTRegistration`                     | Specifies if to include the DCT registration on top of Affine, all other requirements for affine are thus also taken into account the x.modules.asl.bAffineRegistration must be >0 for DCT to run: `0` = DCT registration disabled `1` = DCT registration enabled if affine enabled and conditions for affine passed, `2` = DCT enabled as above, but use PVC on top of it to get the local intensity scaling right. | OPTIONAL, DEFAULT = 0 |
| `bRegisterM02ASL`                      | Boolean specifying whether M0 is registered to mean_control image (or T1w if no control image exists). It can be useful to disable M0 registration if the ASL registration is done based on the M0, and little motion is expected between the M0 and ASL acquisition. If no separate M0 image is available, this parameter will have no effect. This option is disabled automatically for 3D spiral: `0` = M0 registration disabled, `1` = M0 registration enabled (DEFAULT). | OPTIONAL, DEFAULT = 0 |
| `bUseMNIasDummyStructural`             | When structural (e.g. T1w) data is missing, copy population-average MNI templates as dummy structural templates. With this option, the ASL module copies the structural templates to fool the pipeline, resulting in ASL registration to these templates. While the rigid-body parameters might still be found somewhat correctly, with this option it is advised to enable affine registration for ASL as well, since ASL and these dummy structural images will differ geometrically. When disabled, an error will be issued instead when the structural image are missing. `1` = enabled, `0` = disabled. | OPTIONAL, DEFAULT = 0 |
| `bPVCNativeSpace`                      | Performs partial volume correction (PVC) in ASL native space using the GM and WM maps obtained from previously segmented T1-weighted images. Skipped with warning when those maps do not exist and are not resampled to the ASL space. PVC can take several minutes for larger scans (e.g. 128x128x30), so it is deactivated by default. `1` = enabled, `0` = disabled. | OPTIONAL, DEFAULT = 0 |
| `PVCNativeSpaceKernel`                 | Kernel size for the ASL native space PVC. This is ignored when x.modules.asl.bPVCNativeSpace is set to 0. Equal weighting of all voxels within the kernel is assumed. 3D kernel can be used, but any of the dimension can be also set to 1. Only odd number of voxels can be used in each dimension (e.g. `[3 7 5]` not `[2 3 1]`). | OPTIONAL, DEFAULT = `[5 5 1]` for bPVCGaussianMM==0, `[10 10 4]` for bPVCGaussianMM==1 |
| `bPVCGaussianMM`                       | If set to 1, PV-correction with a Gaussian weighting is used instead of the equal weights of all voxels in the kernel ('flat' kernel) as per Asllani's original method. Ignored when x.modules.asl.bPVCNativeSpace is set to 0. Unlike with the flat kernel when the size is defined in voxels, here the FWHM of the Gaussian in mm is defined in each dimension. The advantage is twofold - continuous values can be added and a single value can be entered which is valid for datasets with different voxel-sizes without having a kernel of different effective size.`1` = enabled, use Gaussian kernel with FWHM in mm given in PVCNativeSpaceKernel, `0` = disabled, use 'flat' kernel with voxels given in PVCNativeSpaceKernel. | OPTIONAL, DEFAULT = 0 |
| `bMakeNIfTI4DICOM`                     | Boolean to output CBF native space maps resampled and/or registered to the original T1w/ASL, and contrast adapted and in 12 bit range allowing to convert the NIfTI to a DICOM file, e.g. for implementation in PACS or other DICOM archives. If set to true, an additional CBF image will be created with modifications that allow it to be easily implemented back into a DICOM for e.g. PACS: 1. Remove peak & valley signal, remove NaNs, rescale to 12 bit integers, apply original orientation (2 copies saved, with original ASL and T1w orientation). |  
| `ApplyQuantification`                  | A vector of 1x6 logical values specifying which types on quantified images should be calculated and saved. Fields: **1)** Apply ScaleSlopes ASL4D (xASL_wrp_Quantify, future at dcm2niiX stage), **2)** Apply ScaleSlopes M0 (xASL_quant_M0, future at dcm2niiX stage), **3)** Convert PWI a.u. to label (xASL_wrp_Quantify, future at xASL_wrp_Reslice?), **4)** Quantify M0 a.u. (xASL_quant_M0, corrects for incomplete T1 relaxation), **5)** Perform division by M0. **6)** Apply all scaling. Examples: ASL4D is an already quantified CBF image, disable all quantification `'[0 0 0 0 0 0]'`. To compare label but not CBF (e.g. label in vessels or sinus vs tissue): `[1 1 1 1 0 1]'`. Note that the output always goes to CBF.nii. | OPTIONAL, DEFAULT = `'[1 1 1 1 1 1]'` = all enabled |
| `sessionMergingList`                  | A list of lists of sessions to merge. One or several lists can be provided. Each of these lists contain sessions that are concatenated in the given order and quantified together. All individual sessions from that list are pre-processed separately. When processing the last session in the list, all previous sessions including the current one are concatenated and then quantified together and the result is written for this current session. Example: [["ASL_1", "ASL_2"],["ASL_1","ASL_3"]] merges session 1 and 2 and provides the output in session 2, and then merges session 1 and 3 and provides them in the session 3. | OPTIONAL, DEFAULT = empty |
| `bQuantifyMultiTE`                  | If set to 1, multi-TE quantification is enabled in case that multi-TE data are present. It is enabled by default when mutli-TE data are detected. | OPTIONAL, DEFAULT = true|


### STRUCTURAL PROCESSING PARAMETERS
All **structural module** related parameters are stored within this subfield. Use these to configure processing options of the structural module. Note that some of these settings are directly in the `modules` and some are in `modules.structural`.

| `x.modules.[...]`             | Description                                                                                              | Defaults                |
| ----------------------------- |:--------------------------------------------------------------------------------------------------------:|:-----------------------:|
| `bRunLongReg`                 | Run longitudinal registration.                                                                           | OPTIONAL, DEFAULT = 0   |
| `bRunDARTEL`                  | Run between-subject registration/create templates.                                                       | OPTIONAL, DEFAULT = 0   |
| `WMHsegmAlg`                  | Choose the LST algorithm 'LGA'/'LPA' for WMH segmentation                                        | OPTIONAL, DEFAULT = 'LPA'   |
| `structural.bSegmentSPM12`    | Boolean to specify if SPM12 segmentation is run instead of CAT12. Options: 1 = run SPM12, 0 = run CAT12. | OPTIONAL, DEFAULT = 0   |
| `structural.bHammersCAT12`    | Boolean specifying if CAT12 should provide Hammers volumetric ROI results.                               | OPTIONAL, DEFAULT = 0   |
| `structural.bFixResolution`   | Resample to a resolution that CAT12 accepts.                                                             | OPTIONAL, DEFAULT=false |
| `population.bNativeSpaceAnalysis`   | Output final results calculated also in the subject's native space.                                                            | OPTIONAL, DEFAULT=false |

### GENERAL PROCESSING PARAMETERS
General **settings** can be found in this subfield.

| `x.settings.[...]`                    | Description                                   | Defaults           |
| ------------------------------------- |:---------------------------------------------:|:------------------:|
| `Quality`                             | Boolean specifying on which quality the pipeline should be run, options: `1` = normal quality, `0` = lower quality, fewer iterations and lower resolution of processing for a fast try-out. | OPTIONAL, DEFAULT = 1 |
| `DELETETEMP`                          | Boolean for removing the temporary files. Options: `0` = keeping all files, `1` = delete temporary files created by the pipeline. | OPTIONAL, DEFAULT = 1 |
| `SkipIfNoFlair`                       | Boolean to skip processing of subjects that do not have a FLAIR image. These parameters can be useful when some data is still complete, but one would like to start image processing already. Options: `1` = skip processing of a subject that does not have a FLAIR image `0` = do not skip anything. | OPTIONAL, DEFAULT = 0 |
| `SkipIfNoASL`                         | Boolean to skip processing of subjects that do not have a ASL image. Options: `1` = skip processing of a subject that does not have a ASL image, `0` = do not skip anything. | OPTIONAL, DEFAULT = 0 |
| `SkipIfNoM0`                          | Boolean to skip processing of subjects that do not have a M0 image. Options:  `1` = skip processing of a subject that does not have a M0 image, `0` = do not skip anything. | OPTIONAL, DEFAULT = 0 |
| `stopAfterErrors`           | Number of allowed errors before job iteration is stopped | (OPTIONAL, DEFAULT = inf) |
| `bLesionFilling`            | Boolean for lesion filling in structural module (submodule 5). | (OPTIONAL, DEFAULT = true) |
| `bAutoACPC`                 | Boolean whether center of mass alignment should be performed before SPM registration. | (OPTIONAL, DEFAULT = true) |

### MASKING & ATLAS PARAMETERS
The `x.S` subfield contains **masking & atlas** related parameters. 

| `x.S.[...]`                           | Description                                   | Defaults           |
| ------------------------------------- |:---------------------------------------------:|:------------------:|
| `bMasking`                            | Vector specifying if we should mask a ROI with a subject-specific mask (1 = yes, 0 = no): `[1 0 0 0]` = susceptibility mask (either population-or subject-wise), `[0 1 0 0]` = vascular mask (only subject-wise), `[0 0 1 0]` = subject-specific tissue-masking (e.g. pGM>0.5), `[0 0 0 1]` = WholeBrain masking (used as memory compression) `[0 0 0 0]` = no masking at all, `[1 1 1 1]` = apply all masks, Can also be used as boolean, where 1 = `[1 1 1 1]`, 0 = `[0 0 0 0]`. Can be useful for e.g. loading lesion masks outside the GM. | OPTIONAL, DEFAULT=1 |
| `DataTypes`                        | Vector of cells specifying which images to take data from, in the ROI analysis. E.g., {'Tex’, 'ATT' 'SD' 'M0' 'rc1T1'} | OPTIONAL, DEFAULT = {'qCBF'} |
| `Atlases`                             | Vector specifying the atlases which should be used within the population module. Default definition within the Population Module: `x.S.Atlases = {'TotalGM','DeepWM'}`. Available atlases (please check the atlas NIfTI and accompanying files for more information): **Free atlases**: `TotalGM`: Mask of the entire GM `'./External/SPMmodified/MapsAdded/TotalGM.nii'`, `TotalWM`: Mask of the entire WM `'./External/SPMmodified/MapsAdded/TotalWM.nii.gz'`, `DeepWM`: Mask of the deep WM `'./External/SPMmodified/MapsAdded/DeepWM.nii.gz'`, `WholeBrain`: Mask of the entire brain `'./External/SPMmodified/MapsAdded/WholeBrain.nii.gz'`, `MNI_Structural`: MNI cortical atlas '`./External/Atlases/MapsAdded/MNI_Structural.nii.gz'`, `Tatu_ACA_MCA_PCA`: Original vascular territories by Tatu et al. `'./External/SPMmodified/MapsAdded/VascularTerritories/Tatu_ACA_MCA_PCA.nii.gz'`, `Tatu_ICA_PCA`: Tatu - only ICA and PCA `'./External/SPMmodified/MapsAdded/VascularTerritories/Tatu_ICA_PCA.nii'`, `Tatu_ICA_L_ICA_R_PCA`: `'./External/SPMmodified/MapsAdded/VascularTerritories/Tatu_ICA_L_ICA_R_PCA.nii.gz'`, `Tatu_ACA_MCA_PCA_Prox_Med_Dist`: Tatu separated to distal/medial/proximal of ACA/MCA/PCA `'./External/SPMmodified/MapsAdded/VascularTerritories/Tatu_ACA_MCA_PCA_Prox_Med_Dist.nii.gz'`, `Mindboggle_OASIS_DKT31_CMA`: Mindboggle-101 cortical atlas `'./External/Atlases/Mindboggle_OASIS_DKT31_CMA.nii.gz'`. **Free for non-commercial use only**: `DKT`: Desikan-Killiany atlas `'./External/Atlases/Desikan_Killiany_MNI_SPM12.nii.gz'`, `HOcort_CONN`: Harvard-Oxford cortical atlas `'./External/Atlases/HOcort_CONN.nii.gz'`, `HOsub_CONN`: Harvard-Oxford subcortical atlas `'./External/Atlases/HOsub_CONN.nii.gz'`, `Hammers`: Alexander Hammers's brain atlas `'./External/Atlases/Hammers.nii.gz'`, `HammersCAT12`: Hammers atlas adapted to DARTEL template of IXI550 space `'./External/Atlases/HammersCAT12.nii'`, `Thalamus`: Harvad-Oxford thalamus atlas `'./External/Atlases/Thalamus.nii.gz'`. | OPTIONAL, DEFAULT=`{'TotalGM','DeepWM'}` |

## DataPar.json example
An example configuration file is given below. Note that we include a large number of options and sequence parameters with the purpose of showing the correct formatting of the file and in the praxis no `dataPar.json` or only a couple of parameters are typically provided.

```json
{"x": {
    "dataset": {
	"subjectRegexp": "^\\d{3}$",
	"exclusion": ""},
    "SESSIONS": ["ASL_1","ASL_2"],
    "S":{"Atlases": ["Thalamus", "HOcort_CONN", "TotalGM"]},
    "session":{
	"options": ["baseline","drug"]},
    "external":{
	"bAutomaticallyDetectFSL": 1},
    "Q":{
	"SliceReadoutTime": 30,
	"T2art": 50,
	"BloodT1": 1650},
    "settings":{
	"Quality": 1,
	"DELETETEMP": 1,
	"bAutoACPC": false},
    "modules":{
	"bRunLongReg": true,
	"structural": {
		"bSegmentSPM12": 1},
	"asl":{
		"M0PositionInASL4D": [1, 2],
		"bUseMNIasDummyStructural": 1,
		"bPVCNativeSpace": 1,
		"PVCNativeSpaceKernel": [10 10 4],
		"bPVCGaussianMM": 1}}}
}
```

## List of additional sequence parameters
Note that sequence parameters should always be defined during the Import through `studyPar.json`. Only in rare cases when DICOM files are not available for the initial import, extra sequence parameters can be supplemented just for the processing. This should only be done in rare cases and otherwise is this practice discouraged.

### SEQUENCE PARAMETERS
Parameters of the ASL sequence. Note that these parameters should have been defined in the ASL-BIDS format for each subject and saved in its JSON-sidecar. These fields, when provided in a `dataPar.json` file are only used if the corresponding ASL-BIDS field is missing. Use of these parameters is thus discouraged unless really needed. 

| `x.Q.[...]`                           | Description                                   | Defaults           |
| ------------------------------------- |:---------------------------------------------:|:------------------:|
| `M0`                                  | Choose which M0 option to use: `'separate_scan'` = for a separate M0 NIfTI (needs to be in the same folder called `M0.nii`), `3.7394*10^6` = (or any other value) single M0 value to use, `'UseControlAsM0'` = will copy the mean control image as M0.nii and process as if it was a separately acquired M0 image (taking TR etc from the `ASL4D.nii`). In case the Background Suppression is used, it will automatically try to compensate for it, `'Absent'` = will skip any M0 processing; this requires an already quantified CBF image | REQUIRED |
| `BackgroundSuppressionNumberPulses`   | Used to estimate decrease of labeling efficiency. Options: 0 = (no background suppression), 2 = labeling efficiency factor `0.83` (e.g. Philips 2D EPI & Siemens 3D GRASE), 4 = labeling efficiency factor `0.81` (e.g. Philips 3D GRASE), 5 = labeling efficiency factor `0.75` (e.g. GE 3D spiral). | REQUIRED |
| `BackgroundSuppressionPulseTime`    | Vector containing timing, in ms, of the background suppression pulses before the start of the readout (per BIDS). REQUIRED when x.Q.UseControlAsM0 & x.Q.BackgroundSuppressionNumberPulses>0. | Check description |
| `PresaturationTime`                 | Time in ms before the start of the readout, scalar, when the slice has been saturated (90 degree flip) this has to come before all the bSup pulses, but doesn't need to be always specified. Defaults to PLD (PASL) or PLD+LabDur ((P)CASL). | OPTIONAL |
| `readoutDim`                        | String specifying the readout type. Options: `'2D'` for slice-wise readout, `'3D'` for volumetric readout. | REQUIRED |
| `Vendor`                            | String containing the Vendor used. This parameter is used to apply the Vendor-specific scale factors, options: 'GE_product', 'GE_WIP', 'Philips', 'Siemens'. | REQUIRED for ASL |
| `Sequence`                          | String containing the sequence used. Options: `'3D_spiral', '3D_GRASE', '2D_EPI'`. | REQUIRED for ASL |
| `LabelingType`                      | String containing the labeling strategy used. Options: `'PASL'` (pulsed Q2-TIPS), `'CASL'` (CASL/PCASL). Note: pulsed without Q2TIPS cannot be reliably quantified because the bolus width cannot be identified CASL & PCASL are both continuous ASL methods, identical quantification. | REQUIRED for ASL |
| `Initial_PLD`                       | Value of PLD (ms), for 3D this is fixed for whole brain, for 2D this is the PLD of first acquired slice, example: 1800. | REQUIRED for ASL |
| `LabelingDuration`                  | Value of labeling duration (ms), example: 1800. | REQUIRED for ASL |
| `SliceReadoutTime`                  | Value (ms) of time added to the PLD after reading out each slice, example: 31. Other option = `'shortestTR'`; shortest TR enabled gives each sequence the minimal TR. This enables calculating slice delay per subject. | REQUIRED for 2D ASL |

### DataPar.json example for sequence parameters
An example configuration file is given below for the sequence parameters. Please combine these with the configuration file above.

```json
{"x": {
    "Q":{
	"BackgroundSuppressionNumberPulses": 2,
	"LabelingType": "CASL",
	"Initial_PLD": 1800,
	"LabelingDuration": 1800,
	"SliceReadoutTime": 30,
	"T2art": 50,
        "BloodT1": 1650},
}
}
```
