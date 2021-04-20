
# Tutorials & FAQ

----
## Beginner Tutorials

----
### ExploreASL & ASL-BIDS

----
#### Background information

Starting with version **v1.6.0**, **ExploreASL** will support an import workflow which allows the user to convert **DICOM** and **NIFTI** data to the **ASL-BIDS** format. Since **ExploreASL** does not fully utilize the **BIDS** format internally, there will also be an automated workflow to convert from **ASL-BIDS** to the **ExploreASL** **legacy format**. In the following subsections we will explain how you can use the automated **ExploreASL** import workflow to convert your data structure to **ASL-BIDS** and how you can process it.

The `ExploreASL_Master` script will have the following format:

```matlab
[x] = ExploreASL([DataParPath, ImportModules, ProcessModules, bPause, iWorker, nWorkers])
```

Parameter descriptions:

- `DataParPath`: Path to data parameter file (`OPTIONAL`, `DEFAULT = ` prompting user input)
- `ImportModules`: `[DCM2NII, NII2BIDS, ANONYMIZE, BIDS2LEGACY]`
    - `DCM2NII`: Run the DICOM to NIFTI conversion (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 0`)
    - `NII2BIDS`: Run the NIFTI to BIDS conversion (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 0`)
    - `ANONYMIZE`: Run the defacing and full anonymization (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 0`)
    - `BIDS2LEGACY`: Run the BIDS to LEGACY conversion (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 0`)
- `ProcessModules`: `[STRUCTURAL, ASL, POPULATION]`
    - `STRUCTURAL`: Run the Structural Module (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 0`)
    - `ASL`: Run the ASL Module (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 0`)
    - `POPULATION`: Run the Population Module (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 0`)
- `bPause`: Pause workflow before ExploreASL pipeline (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 0`)
- `iWorker`: Allows parallelization when called externally  (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 1`)
- `nWorkers`: Allows parallelization when called externally  (`OPTIONAL`, `BOOLEAN`, `DEFAULT = 1`)

----
#### DICOM source data

(Description about how the DICOM source data has to look like, with regard to the corresponding sourceStructure.json file)

(Explanation about the benefit of being able to use data which basically comes from the scanner directly)

(Mention flavor library and state of development)

(Description about the first import workflow step of converting DCM2NII using dcm2niix)

```matlab
[x] = ExploreASL('sourceStructure.json', [1 0 0 0], 0, 0, 1, 1);
```


----
#### NIFTI source data

(Description about how the DICOM source data has to look like, with regard to the corresponding sourceStructure.json file)

(Description about the second import workflow step of converting NII2BIDS)

```matlab
[x] = ExploreASL('sourceStructure.json', [0 1 0 0], 0, 0, 1, 1);
```


----
#### Data anonymization

(Description about the anonymization step)

```matlab
[x] = ExploreASL('sourceStructure.json', [0 0 1 0], 0, 0, 1, 1);
```


----
#### Data in ExploreASL legacy format

(Description about BIDS2LEGACY)

```matlab
[x] = ExploreASL('sourceStructure.json', [0 0 0 1], 0, 0, 1, 1);
```


----
#### Full pipeline

(Minor example of how the full pipeline can be used)

```matlab
[x] = ExploreASL('sourceStructure.json', 1, 1, 0, 1, 1);
```


----
### How to run ExploreASL using Matlab

The first thing you have to do, to use **ExploreASL**, is to clone the **ExploreASL** repository. If you want to run **ExploreASL** from Matlab, we recommend to clone the main repository directly from the official [GitHub website](https://github.com/ExploreASL/ExploreASL). You also have the option to download the zipped version or to download an older [release](https://github.com/ExploreASL/ExploreASL/releases).

If you are new to Matlab, we recommend checking out a [Matlab tutorial](https://www.mathworks.com/support/learn-with-matlab-tutorials.html). It can be helpful to add the **ExploreASL** directory to your Matlab paths. Open Matlab, select the **Home** tab, and add the **ExploreASL** directory including its subfolders using the **Set Path** option. Now change your working directory, using the **Current Folder** tab or the **cd** command, to the **ExploreASL** directory.

To run **ExploreASL** you have to type in the following command in the **Command Window**: `ExploreASL`. If you already created an **ASL-BIDS dataset**, you can run the full default **ExploreASL** pipeline like this:

```matlab
DataParPath = 'C:\...\MY-BIDS-DATASET\DataParFile.json';
ProcessData = true;
SkipPause = true;
[x] = ExploreASL_Master(DataParPath, ProcessData, SkipPause);
```

----
### How to run a compiled ExploreASL Version


To compile ExploreASL you have to run the `xASL_adm_MakeStandalone.m` script. If necessary, you can also ask the developer team for a specific compiled version. Providing a compiled version for every operating system and corresponding Matlab version is currently not feasible for us. Please feel free to ask us for help though!
A compiled version of ExploreASL always requires the corresponding Matlab Runtime. Please checkout the official [Matlab Documentation](https://mathworks.com/products/compiler/matlab-runtime.html). Download the Matlab Runtime of the Matlab Version which was used for the compilation. Make sure to install the Matlab Runtime correctly. If you're using Windows, it is important that the path to the Matlab Runtime is added to Windows **PATH** during the installation.

#### Windows

Let's assume you want to run the compiled version of **ExploreASL v1.4.0**. Check the contents of the folder created by `xASL_adm_MakeStandalone.m`, which contains the compiled version. There should be a file called `xASL_1_4_0.exe`. We recommend using the command line interface now. For this you can go to the address bar of your file explorer. Type in `cmd` to open the command prompt in the current folder. The following command will start **ExploreASL** and process the dataset of the DataParFile:

```console
xASL_1_4_0.exe "c:\path_to_your\DataParFile.json"
```

The executable will extract all necessary data from the CTF archive within the folder. This is totally normal. Within the command window you should see that **ExploreASL** is starting to process the given dataset:

```console
xASL_1_4_0.exe "c:\path_to_your\DataParFile.json"
```

```console
==============================================================================================
ctfroot:  .\xASL_1_4_0\xASL_1_4_0_mcr
x.MyPath: .\xASL_1_4_0\xASL_1_4_0_mcr\xASL_1_4_0
==============================================================================================
 ___  ____  __  __
/ __)(  _ \(  \/  )  modified xASL version
\__ \ )___/ )    (   Statistical Parametric Mapping
(___/(__)  (_/\/\_)  SPM12 - http://www.fil.ion.ucl.ac.uk/spm/

--> Initializing ExploreASL v1.4.0...


==============================================================================================
 ________                      __                                 ______    ______   __
/        |                    /  |                               /      \  /      \ /  |
########/  __    __   ______  ## |  ______    ______    ______  /######  |/######  |## |
## |__    /  \  /  | /      \ ## | /      \  /      \  /      \ ## |__## |## \__##/ ## |
##    |   ##  \/##/ /######  |## |/######  |/######  |/######  |##    ## |##      \ ## |
#####/     ##  ##<  ## |  ## |## |## |  ## |## |  ##/ ##    ## |######## | ######  |## |
## |_____  /####  \ ## |__## |## |## \__## |## |      ########/ ## |  ## |/  \__## |## |_____
##       |/##/ ##  |##    ##/ ## |##    ##/ ## |      ##       |## |  ## |##    ##/ ##       |
########/ ##/   ##/ #######/  ##/  ######/  ##/        #######/ ##/   ##/  ######/  ########/
                    ## |
                    ## |
                    ##/
==============================================================================================


ExploreASL initialized <--
Automatically defining sessions...
-------------------------------------------
ExploreASL will run with following settings:

Root folder = .\TestDataSet\analysis

1 scans - 0 exclusions, resulting in 1 scans of:
Longitudinal timePoint 1 = 1 scans - 0 exclusions = 1 scans
ASL sessions: 1

Ancillary data, sets:
3 sets are defined for 1 "SubjectsSessions":
Set 1 = "LongitudinalTimePoint" options "TimePoint_1", codes for paired data
Set 2 = "SubjectNList" options "SubjectNList", codes for paired data
Set 3 = "Site" options "SingleSite", codes for two-sample data
x.DELETETEMP = 1 (delete temporary files)
x.Quality    = 0 (0 = fast try-out; 1 = normal high quality)

---------------------------------------------


Running xASL_module_Structural ...
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
...
```

To test if it is possible to initialize **ExploreASL** without the processing of a dataset, you could run the following command:

```console
xASL_1_4_0.exe "" "0" "1" "1" "1"
```

The usual **ExploreASL** parameters (DataParPath, ProcessData, SkipPause, iWorker, nWorkers) have to be given to the compiled **ExploreASL** version as strings. The resulting output could look like this:

```console
xASL_1_4_0.exe "" "0" "1" "1" "1"
==============================================================================================
ctfroot:  .\xASL_1_4_0\xASL_1_4_0_mcr
x.MyPath: .\xASL_1_4_0\xASL_1_4_0_mcr\xASL_1_4_0
==============================================================================================
 ___  ____  __  __
/ __)(  _ \(  \/  )  modified xASL version
\__ \ )___/ )    (   Statistical Parametric Mapping
(___/(__)  (_/\/\_)  SPM12 - http://www.fil.ion.ucl.ac.uk/spm/

--> Initializing ExploreASL v1.4.0...


==============================================================================================
 ________                      __                                 ______    ______   __
/        |                    /  |                               /      \  /      \ /  |
########/  __    __   ______  ## |  ______    ______    ______  /######  |/######  |## |
## |__    /  \  /  | /      \ ## | /      \  /      \  /      \ ## |__## |## \__##/ ## |
##    |   ##  \/##/ /######  |## |/######  |/######  |/######  |##    ## |##      \ ## |
#####/     ##  ##<  ## |  ## |## |## |  ## |## |  ##/ ##    ## |######## | ######  |## |
## |_____  /####  \ ## |__## |## |## \__## |## |      ########/ ## |  ## |/  \__## |## |_____
##       |/##/ ##  |##    ##/ ## |##    ##/ ## |      ##       |## |  ## |##    ##/ ##       |
########/ ##/   ##/ #######/  ##/  ######/  ##/        #######/ ##/   ##/  ######/  ########/
                    ## |
                    ## |
                    ##/
==============================================================================================


ExploreASL initialized <--
```

#### Linux

On Linux you can basically do the same as above. Let's assume we chose the option to create a compiled **ExploreASL** version labelled with the **latest** tag within `xASL_adm_MakeStandalone.m` script. We can run the ExploreASL shell script with a specified Matlab MCR (here we use version 96 e.g.) using the following command:

```console
./run_xASL_latest.sh /usr/local/MATLAB/MATLAB_Runtime/v96/ "" "0" "1" "1" "1"
```

Using the options `"" "0" "1" "1" "1"` we initialize **ExploreASL**, but do not process a dataset. To run a dataset, we have to switch the ProcessData parameter from 0 to 1 and pass a path for the DataParPath. This could look something like this:

```console
./run_xASL_latest.sh /usr/local/MATLAB/MATLAB_Runtime/v96/ "/home/TestDataSet/analysis/DataParFile.json" "1" "1" "1" "1"
```


----
### How to run ExploreASL using the docker image

First you have to pull an official docker image from the **ExploreASL** repository:

```console
docker pull docker.pkg.github.com/exploreasl/docker/xasl:1.x.x
```

If you want to rename the docker image, you can use the docker tag command:

```console
docker tag docker.pkg.github.com/exploreasl/docker/xasl:1.x.x xasl:1.x.x
```

To start a docker container of a dataset you can use the following command:

```console
docker run -e DATAPARFILE="DataSet/DataParFile.json" -v /home/.../incoming:/data/incoming -v /home/.../outgoing:/data/outgoing xasl:1.x.x
```

- Here DATAPARFILE is an environment variable which is a relative path to the data parameter file of your dataset.
- ```/home/.../incoming:/data/incoming``` is used to mount your dataset folder (```/home/.../incoming```) to its corresponding docker dataset folder (```/data/incoming```). 
- The same notation is used to mount the docker dataset output folder (```/data/outgoing```) to its corresponding real output folder on your drive (```/home/.../outgoing```).


----
## Basic Image Processing


----
### Basic NIFTI Input & Output

Let's assume you want to read in a NIFTI image and apply a mask on it. As a good first step we always recommend to initialize `ExploreASL` first by running the following command.

```matlab
[x] = ExploreASL_Initialize([],0);
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
maskResampled = xASL_im_ResampleLinearFair(mask, size(image));
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
### Basic File Operations

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
## Advanced Tutorials





----
## FAQ



