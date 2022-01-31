
# Tutorials (Processing)


----
## Atlas Options

The atlases of the **ExploreASL** population module can be defined in the `x.S` sub-structure. If you are interested in the `TotalGM`, `TotalWM`, `DeepWM`, `Hammers`, `HOcort_CONN`, `HOsub_CONN`, and `Mindboggle_OASIS_DKT31_CMA` atlases e.g., you can add the following lines to your `dataPar.json` file.


```json
{
	"x": [{
			"name": "Example",
			"subject_regexp": "Example",
			"S": {"Atlases": ["TotalGM","TotalWM","DeepWM","Hammers","HOcort_CONN","HOsub_CONN","Mindboggle_OASIS_DKT31_CMA"]}
		}]
}
```

Available atlases are:

Free atlases: 

- `TotalGM`: Mask of the entire GM
- `TotalWM`: Mask of the entire WM
- `DeepWM`: Mask of the deep WM
- `WholeBrain`: Mask of the entire brain
- `MNI_Structural`: MNI cortical atlas
- `Tatu_ACA_MCA_PCA`: Original vascular territories by Tatu et al.
- `Tatu_ICA_PCA`: Tatu (only ICA and PCA)
- `Tatu_ICA_L_ICA_R_PCA`: Tatu (L ICA and R PCA)
- `Tatu_ACA_MCA_PCA_Prox_Med_Dist`: Tatu separated to distal/medial/proximal of ACA/MCA/PCA
- `Mindboggle_OASIS_DKT31_CMA`: Mindboggle-101 cortical atlas

Free for non-commercial use only:

- `HOcort_CONN`: Harvard-Oxford cortical atlas
- `HOsub_CONN`: Harvard-Oxford subcortical atlas
- `Hammers`: Alexander Hammers's brain atlas
- `HammersCAT12`: Hammers atlas adapted to DARTEL template of IXI550 space
- `Thalamus`: Harvad-Oxford thalamus atlas

If you want **ExploreASL** to include your favorite atlas in a future version, please get in contact. Only a label NIfTI, a descriptive TSV file, and the corresponding license are required.




### x.settings

General **settings** can be found in this subfield.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.settings.SelectParFile             | Variable which tells the import workflow if we have to ask the user for the study root directory a second time. |
| x.settings.stopAfterErrors           | Number of allowed errors before job iteration is stopped (default=inf). |
| x.settings.dryRun                    | Dry run does not execute the module (default=0). |
| x.settings.bOverwrite                | Re-running makes no sense if you're not overwriting existing files. |
| x.settings.BILAT_FILTER              | Bilateral filter by Matthan Caan (original=1, more recent=2). |
| x.settings.DELETETEMP                | Boolean for removing the temporary files. |
| x.settings.Quality                   | Quality setting for `ExploreASL` processing. Set to 1 for normal high-quality processing or to 0 for low-quality test runs. |
| x.settings.bReproTesting             | n/a |
| x.settings.Pediatric_Template        | n/a |
| x.settings.bLesionFilling            | Boolean for lesion filling in structural module (submodule 5). |
| x.settings.bAutoACPC                 | Boolean whether center of mass alignment should be performed before SPM registration. |
| x.settings.bGetControlLabelOrder     | n/a |
| x.settings.SkipIfNoFlair             | Boolean to skip processing of subjects that do not have a FLAIR image. |
| x.settings.SkipIfNoASL               | Boolean to skip processing of subjects that do not have a ASL image. |
| x.settings.SkipIfNoM0                | Boolean to skip processing of subjects that do not have a M0 image. |


### x.dataset

**Dataset** related parameters can be found in this subfield.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.dataset.name                       | String for the name of the study. |
| x.dataset.subjectRegexp              | String with regular expression for ExploreASL to find subjects by foldername. |
| x.dataset.exclusion                  | Cell with list of subjects to exclude. |
| x.dataset.ForceInclusionList         | Use this field if you want to use a selection of subjects rather than taking all available subjects from directories. |


### x.dir, x.P & x.D

In these subfields we store **path** related values.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.dir.sourceStructure                | Path to the sourceStructure.json file. |
| x.dir.studyPar                       | Path to the studyPar.json file. |
| x.dir.dataset_description            | Path to the dataset_description.json file. |
| x.dir.dataPar                        | Path to the dataPar.json file. |
| x.D.ROOT                             | Path to the root directory. |


### x.Q

In `x.Q` you can find **sequence** and **quantification** related parameters.

| Fieldname                             | Description                                   |
| ------------------------------------- |:---------------------------------------------:|
| x.Q.M0                                | Choose which M0 option to use. |
| x.Q.BackgroundSuppressionNumberPulses | Used to estimate decrease of labeling efficiency. |
| x.Q.BackgroundSuppressionPulseTime    | Vector containing timing, in ms, of the background suppression pulses before the start of the readout (per BIDS). |
| x.Q.PresaturationTime                 | Time in ms before the start of the readout, scalar, when the slice has been saturated (90 degree flip) this has to come before all the bSup pulses, but doesn't need to be always specified. |
| x.Q.readoutDim                        | String specifying the readout type. |
| x.Q.Vendor                            | String containing the Vendor used. |
| x.Q.Sequence                          | String containing the sequence used. |
| x.Q.LabelingType                      | String containing the labeling strategy used. |
| x.Q.Initial_PLD                       | Value of PLD (ms), for 3D this is fixed for whole brain, for 2D this is the PLD of first acquired slice. |
| x.Q.LabelingDuration                  | Value of labeling duration (ms). |
| x.Q.SliceReadoutTime                  | Value (ms) of time added to the PLD after reading out each slice. |
| x.Q.bUseBasilQuantification           | True for using BASIL quantification in addition to ExploreASL's quantification. |
| x.Q.Lambda                            | Brain/blood water coefficient (mL 1H/ mL blood). |
| x.Q.T2art                             | `T2*` of arterial blood at 3T, only used when no M0 image (ms). |
| x.Q.BloodT1                           | T1 relaxation time of arterial blood (ms). Defaults (Alsop MRM 2014), 1800 for GSP phantom. |
| x.Q.TissueT1                          | T1 relaxation time of GM tissue (ms). Defaults (Alsop MRM 2014). |
| x.Q.nCompartments                     | Number of modeled compartments for quantification. |
| x.Q.ApplyQuantification               | A vector of 1x5 logical values specifying which types on quantified images should be calculated and saved. |
| x.Q.SaveCBF4D                         | Boolean, true to also save 4D CBF timeseries, if ASL4D had timeseries. |


### x.modules (Modules)

Parameters for additional **modules** are stored within the main level of `x.modules`.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.bRunLongReg                | Run longitudinal registration. |
| x.modules.bRunDARTEL                 | Run between-subject registration/create templates. |


### x.modules.structural

All **structural module** related parameters are stored within this subfield.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.structural.bSegmentSPM12   | Boolean to specify if SPM12 segmentation is run instead of CAT12. |
| x.modules.structural.bHammersCAT12   | Boolean specifying if CAT12 should provide Hammers volumetric ROI results. |
| x.modules.structural.bFixResolution  | Resample to a resolution that CAT12 accepts. |


### x.modules.asl

All **ASL module** related parameters are stored within this subfield.

| Fieldname                              | Description                                   |
| -------------------------------------- |:---------------------------------------------:|
| x.modules.asl.motionCorrection         | Boolean to perform motion correction in case of timeseries. |
| x.modules.asl.SpikeRemovalThreshold    | Minimal t-stat improval needed to remove motion spikes. |
| x.modules.asl.bRegistrationContrast    | Specifies the image contrast used for registration. |
| x.modules.asl.bAffineRegistration      | Specifies if the ASL-T1w rigid-body registration is followed up by an affine registration. |
| x.modules.asl.bDCTRegistration         | Specifies if to include the DCT registration on top of Affine. |
| x.modules.asl.bRegisterM02ASL          | Boolean specifying whether M0 is registered to mean_control image (or T1w if no control image exists). |
| x.modules.asl.bUseMNIasDummyStructural | When structural (e.g. T1w) data is missing, copy population-average MNI templates as dummy structural templates. |
| x.modules.asl.bPVCNativeSpace          | Performs partial volume correction (PVC) in ASL native space using the GM and WM maps obtained from previously segmented T1-weighted images. |
| x.modules.asl.PVCNativeSpaceKernel     | Kernel size for the ASL native space PVC. |
| x.modules.asl.bPVCGaussianMM           | If set to 1, PV-correction with a Gaussian weighting is used instead of the equal weights of all voxels in the kernel ('flat' kernel) as per Asllani's original method. |
| x.modules.asl.bMakeNIfTI4DICOM         | Boolean to output CBF native space maps resampled and/or registered to the original T1w/ASL, and contrast adapted and in 12 bit range allowing to convert the NIfTI to a DICOM file. |


### x.modules.population

All **population module** related parameters are stored within this subfield.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| ...                                  | n/a |


### x.S

The `x.S` subfield contains **masking & atlas** related parameters. Only a selection of parameters is provided here. A full account is given in the [Developer Tutorial](./Tutorials-Developer/).

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.S.bMasking                         | Vector specifying if we should mask a ROI with a subject-specific mask. |
| x.S.Atlases                          | Vector specifying the atlases which should be used within the population module. |
| x.S.VoxelSize                        | Voxel-size in mm of reslicing & DARTEL (default=1.5mm). |
| x.S.masks                            | Contains skull and WBmask. |




### x.external

Within `x.external` you can find parameters related to **external tools** and the coding **environment**.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.external.SPMVERSION                | String describing the version of SPM. |
| x.external.bAutomaticallyDetectFSL   | Boolean to automatically detect the FSL version if disabled, this function will try to use the system-initialized FSL and throw an error if FSL is not initialized. |




