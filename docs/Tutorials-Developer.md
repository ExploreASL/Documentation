
# Tutorials (Developer)

This tutorial gives an example of useful functions that can be used outside of the pipeline for certain image operations, evaluations, and statistics. More information can be found in the reference manual.

Moreover, we list here the internal parameters in ExploreASL that can be used either to supply further parameters or to modify certain advanced parameters during ExploreASL runtime.

----
## Basic NIFTI Input & Output

Let's assume you want to read in a NIFTI image and apply a mask on it. As a good first step we always recommend to initialize `ExploreASL` first by running the following command.

```matlab
[x] = ExploreASL();
```

Now let's read in the image by defining the image path and using the `xASL_io_Nifti2Im` function.

```matlab
pathNIFTI = fullfile(x.MyPath,'External','TestDataSet','analysis','Sub-001','T1.nii');
image = xASL_io_Nifti2Im(pathNIFTI);
```

Maybe we want to mask the image. The image and the mask do not have the same resolution though, which means we need to resample either the mask or the image. The following commands will resample the mask to the image size.

```matlab
pathMask = fullfile(x.MyPath,'External','SPMmodified','MapsAdded','brainmask.nii');
mask = xASL_io_Nifti2Im(pathMask);
maskResampled = xASL_im_ResampleLinear(mask, size(image));
```

To mask the image we multiply the image matrix with the mask matrix. This way all voxels outside of the mask are set to 0.

```matlab
image = image.*maskResampled;
```

Then we can save the image as a NIFTI again and open it with our favorite NIFTI viewer.

```matlab
xASL_io_CreateNifti(fullfile(x.MyPath,'export.nii'), image);
```

`xASL_io_Nifti2Im` does unzip your image, so make sure to revert those changes within the ExploreASL directory later using `git reset` or `git revert`.

----
## Basic File Operations

Matlab offers two functions to copy or move files and folders from a `source` to a `destination` within your file system. These functions are `copyfile` and `movefile`. To improve speed and multi OS compatibility, we wrote two similar functions, called `xASL_Copy` and `xASL_Move`. In the following example we copy a file called `fileA` from the folder `C:\Users\Test_One` to `C:\Users\Test_Two`. We then move the file back to `C:\Users\Test_One` and overwrite the original `fileA`. Finally, we rename `fileA` to `fileB`.

```matlab
% Copy fileA from Test_One to Test_Two
xASL_Copy('C:\Users\Test_One\fileA.txt', 'C:\Users\Test_Two\fileA.txt');

% Move fileA back to Test_One and overwrite the original fileA
xASL_Move('C:\Users\Test_Two\fileA.txt', 'C:\Users\Test_One\fileA.txt', 1);

% Rename fileA to fileB
xASL_Move('C:\Users\Test_One\fileA.txt', 'C:\Users\Test_One\fileB.txt', 1);
```

Often we need to find a list of files in a certain directory. To do this, we can use `xASL_adm_GetFileList`. Let's assume there are five files called `fileA`, `fileB`, `fileC`, `fileD` and `fileE` in `C:\Users\Test_One`.  We know that all names start with `file`, so we can use this for our regular expression. Check out the example below on how you can get a cell array containing the paths of all these files.

```
strDirectory = 'C:\Users\Test_One';
strRegEx = '^file.+$';
filepaths = xASL_adm_GetFileList(strDirectory, strRegEx);
```

----
## The ExploreASL x structure

The ExploreASL `x` structure is the main object used to define pipeline settings. Besides pipeline settings you can also find processing parameters and metadata there. This manual contains the complete list of all parameters including those that can only be modified during runtime and by developers. Basic and advanced settings for the users that are modifiable in configuration files are listed in the [Processing Tutorials](./Tutorials-Processing/).

### x.opts

General **options** can be found in this subfield.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.opts.DatasetRoot                   | Dataset root directory of the current BIDS dataset. This is also the first input argument of `ExploreASL`. |
| x.opts.ImportModules                 | Vector to define which import modules should be executed. This is also the second input argument of `ExploreASL`. |
| x.opts.ProcessModules                | Vector to define which processing modules should be executed. This is also the third input argument of `ExploreASL`. |
| x.opts.bPause                        | Boolean to set if you want to pause the pipeline before the processing. This is also the fourth input argument of `ExploreASL`. |
| x.opts.iWorker                       | This variable defines which of the parallel ExploreASL calls we are. This is also the fifth input argument of `ExploreASL`. |
| x.opts.nWorkers                      | This variable defines how many ExploreASL calls are made in parallel. This is also the sixth input argument of `ExploreASL`. |
| x.opts.bImportData                   | Boolean that is true if at least one import module is going to be executed. |
| x.opts.bProcessData                  | Boolean that is true if at least one processing module is going to be executed. |
| x.opts.bLoadData                     | Boolean that is true if the current BIDS dataset is going to be loaded. |
| x.opts.MyPath                        | Path to the `ExploreASL` program. |
| x.opts.dataParType                   | String describing the type of input argument that was given for the `DatasetRoot`. This parameter is mainly supposed to help with backwards compatibility. |

### x.modules.import

All **import module** related parameters are stored within this subfield.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.import.settings.bCopySingleDicoms | Boolean to define if single DICOMs should be copied during the import. |
| x.modules.import.settings.bUseDCMTK | Boolean to define if DCMTK should be used for the import workflow. |
| x.modules.import.settings.bCheckPermissions | Boolean to define if the workflow should check for permission issues. |

### x.S

The `x.S` subfield contains **masking & atlas** related parameters. 

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.S.bMasking                         | Vector specifying if we should mask a ROI with a subject-specific mask. |
| x.S.Atlases                          | Vector specifying the atlases which should be used within the population module. |
| x.S.slices                           | Slice numbers: defines which transversal slices to use by default. |
| x.S.slicesLarge                      | Slice numbers: defines which transversal slices to use by default. |
| x.S.slicesExtraLarge                 | Slice numbers: defines which transversal slices to use by default. |
| x.S.nSlices                          | Length of x.S.slices. |
| x.S.nSlicesLarge                     | Length of x.S.slicesLarge. |
| x.S.nSlicesExtraLarge                | Length of x.S.slicesExtraLarge. |
| x.S.TransCrop                        | Cropping settings: defines default transversal cropping settings. |
| x.S.jet256                           | Jet 256 colormap. |
| x.S.gray                             | Grayscale colormap. |
| x.S.red                              | Red colormap. |
| x.S.yellow                           | Yellow colormap. |
| x.S.green                            | Green colormap. |
| x.S.blue                             | Blue colormap. |
| x.S.purple                           | Purple colormap. |
| x.S.turqoise                         | Turqoise colormap. |
| x.S.orange                           | Orange colormap. |
| x.S.colors_ROI                       | Cell array containing the colormaps from above. |
| x.S.cool                             | Cool colorbar. |
| x.S.hot                              | Hot colorbar. |
| x.S.VoxelSize                        | Voxel-size in mm of reslicing & DARTEL (default=1.5mm). |
| x.S.masks                            | Contains skull and WBmask. |
| x.S.LabelClr                         | 64 label colors. |

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

### x.external

Within `x.external` you can find parameters related to **external tools** and the coding **environment**.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.external.SPMVERSION                | String describing the version of SPM. |
| x.external.bAutomaticallyDetectFSL   | Boolean to automatically detect the FSL version if disabled, this function will try to use the system-initialized FSL and throw an error if FSL is not initialized. |

