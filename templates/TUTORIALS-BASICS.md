
# Tutorials (Basics)

----
## How to run ExploreASL using Matlab

The first thing you have to do, to use **ExploreASL**, is to clone the **ExploreASL** repository. If you want to run **ExploreASL** from Matlab, we recommend to clone the main repository directly from the official [GitHub website](https://github.com/ExploreASL/ExploreASL). You also have the option to download the zipped version or to download an older [release](https://github.com/ExploreASL/ExploreASL/releases).

If you are new to Matlab, we recommend checking out a [Matlab tutorial](https://www.mathworks.com/support/learn-with-matlab-tutorials.html). It can be helpful to add the **ExploreASL** directory to your Matlab paths. Open Matlab, select the **Home** tab, and add the **ExploreASL** directory including its subfolders using the **Set Path** option. Now change your working directory, using the **Current Folder** tab or the **cd** command, to the **ExploreASL** directory.

To run **ExploreASL** you have to type in the following command in the **Command Window**: `ExploreASL`. If you already created an **ASL-BIDS dataset** in sourcedata format, you can run the full default **ExploreASL** pipeline like this:

```matlab
DatasetRoot = 'C:\...\MY-BIDS-DATASET';
ImportModules = true;
ProcessModules = true;
bPause = false;
[x] = ExploreASL_Master(DatasetRoot, ImportModules, ProcessModules, bPause);
```

----
## How to run a compiled ExploreASL Version


To compile ExploreASL you have to run the `xASL_adm_MakeStandalone.m` script. If necessary, you can also ask the developer team for a specific compiled version. Providing a compiled version for every operating system and corresponding Matlab version is currently not feasible for us. Please feel free to ask us for help though!
A compiled version of ExploreASL always requires the corresponding Matlab Runtime. Please checkout the official [Matlab Documentation](https://mathworks.com/products/compiler/matlab-runtime.html). Download the Matlab Runtime of the Matlab Version which was used for the compilation. Make sure to install the Matlab Runtime correctly. If you're using Windows, it is important that the path to the Matlab Runtime is added to Windows **PATH** during the installation.

### Windows

Let's assume you want to run the compiled version of **ExploreASL latest**. Check the contents of the folder created by `xASL_adm_MakeStandalone.m`, which contains the compiled version. There should be a file called `xASL_latest.exe`. We recommend using the command line interface now. For this you can go to the address bar of your file explorer. Type in `cmd` to open the command prompt in the current folder. The following command will start **ExploreASL**, import the **ASL-BIDS dataset** in sourcedata format, and process the dataset corresponding to your `DatasetRoot` directory:

```console
xASL_latest.exe "c:\MY-BIDS-DATASET" "1" "1"
```

The executable will extract all necessary data from the CTF archive within the folder. This is totally normal. Within the command window you should see that **ExploreASL** is starting to process the given dataset:

```console
xASL_latest.exe "c:\MY-BIDS-DATASET" "1" "1"
(insert example here)
```

To test if it is possible to initialize **ExploreASL** without the processing of a dataset, you could run the following command:

```console
xASL_latest.exe "" "0" "0"
```

The usual **ExploreASL** parameters (`DatasetRoot`, `ImportModules`, `ProcessModules`, `bPause`, `iWorker`, `nWorkers`) have to be given to the compiled **ExploreASL** version as strings. The resulting output could look like this:

```console
xASL_latest.exe "" "0" "0"
(insert example here)
```

### Linux

On Linux you can basically do the same as above. We can run the ExploreASL shell script with a specified Matlab MCR (here we use **version 96** e.g.) using the following command:

```console
./run_xASL_latest.sh /usr/local/MATLAB/MATLAB_Runtime/v96/ "" "0" "0"
```

Using the options `"" "0" "0"` we initialize **ExploreASL**, but do not process a dataset. To run a dataset, we have to switch the `ImportModules` and/or the `ProcessModules` parameter from `0` to `1` and pass a path for the `DatasetRoot` directory. This could look something like this:

```console
./run_xASL_latest.sh /usr/local/MATLAB/MATLAB_Runtime/v96/ "/home/MY-BIDS-DATASET" "1" "1"
(insert example here)
```


----
## How to run ExploreASL using the docker image

First you have to pull an official docker image from the **ExploreASL** repository:

```console
docker pull exploreasl/xasl:latest
```

Check out your local images using `docker images`. If you want to rename the docker image, tag your image using the docker tag command:

```console
docker tag exploreasl/xasl:latest xasl:my-version
```

To start a docker container of **ExploreASL v1.7.0** e.g., you can use the following command:

```console
docker run -e DATASETROOT=MY-BIDS-DATASET
       -e IMPORTMODULES=1 -e PROCESSMODULES=1
       -v /home/.../incoming:/data/incoming 
       -v /home/.../outgoing:/data/outgoing xasl:1.7.0
```

- Here `DATASETROOT` is an environment variable which is a relative path to the `DATASETROOT` directory of your dataset.
- The `IMPORTMODULES` and `PROCESSMODULES` are the parameters of ExploreASL_Master
- ```/home/.../incoming:/data/incoming``` is used to mount your dataset folder (```/home/.../incoming```) to its corresponding docker dataset folder (```/data/incoming```). 
- The same notation is used to mount the docker dataset output folder (```/data/outgoing```) to its corresponding real output folder on your drive (```/home/.../outgoing```).


----
## The ExploreASL x structure

The ExploreASL `x` structure is the main object used to define pipeline settings. Besides settings you can also find processing and meta data there.

### x.opts (Options)

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


### x.settings (Settings)

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


### x.dataset (Dataset)

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.dataset.name                       | String for the name of the study. |
| x.dataset.subjectRegexp              | String with regular expression for ExploreASL to find subjects by foldername. |
| x.dataset.exclusion                  | Cell with list of subjects to exclude. |
| x.dataset.ForceInclusionList         | Use this field if you want to use a selection of subjects rather than taking all available subjects from directories. |


### x.dir, x.P & x.D (Paths)

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.dir.sourceStructure                | Path to the sourceStructure.json file. |
| x.dir.studyPar                       | Path to the studyPar.json file. |
| x.dir.dataset_description            | Path to the dataset_description.json file. |
| x.dir.dataPar                        | Path to the dataPar.json file. |
| x.D.ROOT                             | Path to the root directory. |


### x.Q (Sequence & quantification)

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

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.bRunLongReg                | Run longitudinal registration. |
| x.modules.bRunDARTEL                 | Run between-subject registration/create templates. |


**Import module**: `x.modules.import`

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| ...                                  | n/a |


**Structural module**: `x.modules.structural`

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.structural.bSegmentSPM12   | Boolean to specify if SPM12 segmentation is run instead of CAT12. |
| x.modules.structural.bHammersCAT12   | Boolean specifying if CAT12 should provide Hammers volumetric ROI results. |
| x.modules.structural.bFixResolution  | Resample to a resolution that CAT12 accepts. |


**ASL module**: `x.modules.asl`

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


**Population module**: `x.modules.population`

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| ...                                  | n/a |


### x.S (Masking & atlases)

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



### x.external (External tools)

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.external.SPMVERSION                | String describing the version of SPM. |
| x.external.bAutomaticallyDetectFSL   | Boolean to automatically detect the FSL version if disabled, this function will try to use the system-initialized FSL and throw an error if FSL is not initialized. |








