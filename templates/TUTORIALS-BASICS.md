
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

### Options: x.opts

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


### General settings: x.settings

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.settings.SelectParFile             |  |
| x.settings.stopAfterErrors           |  |
| x.settings.dryRun                    |  |
| x.settings.bOverwrite                |  |
| x.settings.BILAT_FILTER              |  |
| x.settings.DELETETEMP                |  |
| x.settings.Quality                   | Quality setting for `ExploreASL` processing. Set to 1 for normal high-quality processing or to 0 for low-quality test runs. |
| x.settings.bReproTesting             |  |
| x.settings.Pediatric_Template        |  |
| x.settings.bLesionFilling            |  |
| x.settings.bAutoACPC                 |  |
| x.settings.bGetControlLabelOrder     |  |
| x.settings.SkipIfNoFlair             |  |
| x.settings.SkipIfNoASL               |  |
| x.settings.SkipIfNoM0                |  |


### General dataset parameters: x.dataset

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.dataset.name                       |  |
| x.dataset.subjectRegexp              |  |
| x.dataset.exclusion                  |  |
| x.dataset.ForceInclusionList         |  |


### Paths & directories: x.dir, x.P & x.D

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.dir.sourceStructure                |  |
| x.dir.studyPar                       |  |
| x.dir.dataset_description            |  |
| x.dir.dataPar                        |  |
| x.D.ROOT                             |  |


### Sequence & quantification parameters: x.Q

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.Q.M0                               |  |
| x.Q.BackgroundSuppressionNumberPulses |  |
| x.Q.BackgroundSuppressionPulseTime   |  |
| x.Q.PresaturationTime                |  |
| x.Q.readoutDim                       |  |
| x.Q.Vendor                           |  |
| x.Q.Sequence                         |  |
| x.Q.LabelingType                     |  |
| x.Q.Initial_PLD                      |  |
| x.Q.LabelingDuration                 |  |
| x.Q.SliceReadoutTime                 |  |
| x.Q.bUseBasilQuantification          |  |
| x.Q.Lambda                           |  |
| x.Q.T2art                            |  |
| x.Q.BloodT1                          |  |
| x.Q.TissueT1                         |  |
| x.Q.nCompartments                    |  |
| x.Q.ApplyQuantification              |  |
| x.Q.SaveCBF4D                        |  |


### Processing parameters: x.modules

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.bRunLongReg                |  |
| x.modules.bRunDARTEL                 |  |


**Import module**: `x.modules.import`

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| ...                                  |  |


**Structural module**: `x.modules.structural`

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.structural.bSegmentSPM12   |  |
| x.modules.structural.bHammersCAT12   |  |
| x.modules.structural.bFixResolution  |  |


**ASL module**: `x.modules.asl`

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.asl.motionCorrection       |  |
| x.modules.asl.SpikeRemovalThreshold  |  |
| x.modules.asl.bRegistrationContrast  |  |
| x.modules.asl.bAffineRegistration    |  |
| x.modules.asl.bDCTRegistration       |  |
| x.modules.asl.bRegisterM02ASL        |  |
| x.modules.asl.bUseMNIasDummyStructural |  |
| x.modules.asl.bPVCNativeSpace          |  |
| x.modules.asl.PVCNativeSpaceKernel     |  |
| x.modules.asl.bPVCGaussianMM           |  |
| x.modules.asl.bMakeNIfTI4DICOM         |  |


**Population module**: `x.modules.population`

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| ...                                  |  |


### Masking & atlas parameters: x.S

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.S.bMasking                         |  |
| x.S.Atlases                          |  |
| x.S.slices                           |  |
| x.S.slicesLarge                      |  |
| x.S.slicesExtraLarge                 |  |
| x.S.nSlices                          |  |
| x.S.nSlicesLarge                     |  |
| x.S.nSlicesExtraLarge                |  |
| x.S.TransCrop                        |  |
| x.S.jet256                           |  |
| x.S.gray                             |  |
| x.S.red                              |  |
| x.S.yellow                           |  |
| x.S.green                            |  |
| x.S.blue                             |  |
| x.S.purple                           |  |
| x.S.turqoise                         |  |
| x.S.orange                           |  |
| x.S.colors_ROI                       |  |
| x.S.cool                             |  |
| x.S.hot                              |  |
| x.S.VoxelSize                        |  |
| x.S.masks                            |  |
| x.S.LabelClr                         |  |



### External toolboxes & environment parameters: x.external

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.external.SPMVERSION                | String describing the version of SPM. |
| x.external.bAutomaticallyDetectFSL   |  |








