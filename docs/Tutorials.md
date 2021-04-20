
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

**Parameter descriptions:**

- `DataParPath`: Path to data parameter file (`OPTIONAL`)
    |                  | DataParPath           |
    | ---------------- |:---------------------:|
    | **Type**         | `CHAR ARRAY`          |
    | **Default**      | Prompting user input  |

- `ImportModules`: Multi-step import workflow (`OPTIONAL`)
    - `DCM2NII`: Run the DICOM to NIFTI conversion
    - `NII2BIDS`: Run the NIFTI to BIDS conversion
    - `ANONYMIZE`: Run the defacing and full anonymization
    - `BIDS2LEGACY`: Run the BIDS to LEGACY conversion

    | ImportModules    | DCM2NII       | NII2BIDS      | ANONYMIZE     | BIDS2LEGACY   |
    | ---------------- |:-------------:|:-------------:|:-------------:|:-------------:|
    | **Type**         | `BOOLEAN`     | `BOOLEAN`     | `BOOLEAN`     | `BOOLEAN`     |
    | **Default**      | `false`       | `false`       | `false`       | `false`       |
    
- `ProcessModules`: Multi-step processing pipeline (`OPTIONAL`)
    - `STRUCTURAL`: Run the Structural Module
    - `ASL`: Run the ASL Module
    - `POPULATION`: Run the Population Module
    
    | ProcessModules   | STRUCTURAL    | ASL           | POPULATION    |
    | ---------------- |:-------------:|:-------------:|:-------------:|
    | **Type**         | `BOOLEAN`     | `BOOLEAN`     | `BOOLEAN`     |
    | **Default**      | `false`       | `false`       | `false`       |  
    
- `bPause`: Pause workflow before ExploreASL pipeline (`OPTIONAL`)
    |                  | bPause                |
    | ---------------- |:---------------------:|
    | **Type**         | `BOOLEAN`             |
    | **Default**      | `false`               |

- `iWorker`: Allows parallelization when called externally  (`OPTIONAL`)
    |                  | iWorker               |
    | ---------------- |:---------------------:|
    | **Type**         | `INTEGER`             |
    | **Default**      | `1`                   |

- `nWorkers`: Allows parallelization when called externally  (`OPTIONAL`)
    |                  | nWorkers              |
    | ---------------- |:---------------------:|
    | **Type**         | `INTEGER`             |
    | **Default**      | `1`                   |

In the following examples, we want to show how you can use the revised import workflow and how the conventional processing is done now.

----
#### DICOM source data

Converting DICOM source data according to the [ASL BIDS](https://bids-specification.readthedocs.io/en/latest/99-appendices/12-arterial-spin-labeling.html) standard can be done using the new import workflow. For the upcoming release **v1.6.0** we're preparing an exemplary DICOM source dataset based on the [ASL DRO](https://pypi.org/project/asldro/). To run this workflow, you have to use the path to your `sourceStructure.json` file instead of the path to your `DataPar.json` file for the `DataParPath` input argument. Do not forget to set up the source dataset correctly. You'll be required to define a `studyPar.json` file as well. We're working on a flavor library, to enable the import and processing of a wide variety of different sequence, vendor, and scanner combinations.

The first step to convert your **DICOM** source data to **NIFTI** source data is to run the following command:

```matlab
[x] = ExploreASL('sourceStructure.json', [1 0 0 0], 0, 0, 1, 1);
```

Here we tell **ExploreASL** to run the `DCM2NII` import module by setting the first boolean variable of the `ImportModules` to `1`.

----
#### NIFTI source data

Let's assume we have **NIFTI** source data according to the [ASL BIDS](https://bids-specification.readthedocs.io/en/latest/99-appendices/12-arterial-spin-labeling.html) standard now. To convert this **ASL BIDS** source data to **ASL BIDS** raw data, we have to run the second part of the import workflow. To run the second step, we set the second variable of the `ImportModules` to `1`. Similar to the previous step, we pass the path to the `sourceStructure.json` file to **ExploreASL**.

```matlab
[x] = ExploreASL('sourceStructure.json', [0 1 0 0], 0, 0, 1, 1);
```


----
#### Data anonymization

There's also a new option to anonymize your data. To do this, you can run the third step of the import workflow. This is done by setting the third variable of the `ImportModules` to `1`. Similar to the previous steps, we pass the path to the `sourceStructure.json` file to **ExploreASL**.

```matlab
[x] = ExploreASL('sourceStructure.json', [0 0 1 0], 0, 0, 1, 1);
```


----
#### Data in ExploreASL legacy format

Right now, **ExploreASL** still uses the conventional data structure. To convert our **ASL BIDS** rawdata to the **ExploreASL** legacy format, we run the last step of the import workflow. This is done by setting the fourth variable of the `ImportModules` to `1`. Similar to the previous steps, we pass the path to the `sourceStructure.json` file to **ExploreASL**.

```matlab
[x] = ExploreASL('sourceStructure.json', [0 0 0 1], 0, 0, 1, 1);
```

The import workflow will also generate a `DataPar.json` file. To adapt your pipeline, you can still use the same settings as before, by changing the **JSON** fields.


----
#### ExploreASL processing pipeline

If you just want to use the conventional **ExploreASL** processing pipeline, you can simply turn off the import workflow by setting all `ImportModules` variables to `0` individually or by using a single `0` for the `ImportModules`. This results in the following notation:

```matlab
[x] = ExploreASL('sourceStructure.json', 0, 1, 0, 1, 1);
```

To run individual modules, you can set the `ProcessModules` individually. If you only want to the `Structural Module` for example, you can use a `[1 0 0]` vector. To run all modules, you can use a single `1` or a vector of ones, which should look like this `[1 1 1]`. Running the full processing pipeline can therefore be done like this as well:

```matlab
[x] = ExploreASL('sourceStructure.json', 0, [1 1 1], 0, 1, 1);
```

----
#### Full pipeline

Running the full pipeline including both import workflow and processing pipeline, can be done by setting both `ImportModules` and `ProcessModules` to `1`.

```matlab
[x] = ExploreASL('sourceStructure.json', 1, 1, 0, 1, 1);
```

----
#### Other tips and tricks

Note that we changed both name and behavior of the `SkipPause` variable. The variable is called `bPause` now. Setting it to `true` or `1`, will result in the pipeline being paused before the processing. We removed the `iModules`, but the functionality of `ProcessModules` is basically the same. We use boolean notation now, so instead of `[1 2 3]` you have to use `[1 1 1]` now.

The overall import functionality is a work in progress right now. We expect stable behavior in release `v1.6.0` though. If you plan on using the `develop` branch until then, you have to live with more or less unstable import behavior.

The examples shown below are related to versions `v1.5.1` and older. They will be updated soon. If you want to work with the current `develop` version, these examples obviously do not apply anymore.


----
### How to run ExploreASL using Matlab

The first thing you have to do, to use **ExploreASL**, is to clone the **ExploreASL** repository. If you want to run **ExploreASL** from Matlab, we recommend to clone the main repository directly from the official [GitHub website](https://github.com/ExploreASL/ExploreASL). You also have the option to download the zipped version or to download an older [release](https://github.com/ExploreASL/ExploreASL/releases).

If you are new to Matlab, we recommend checking out a [Matlab tutorial](https://www.mathworks.com/support/learn-with-matlab-tutorials.html). It can be helpful to add the **ExploreASL** directory to your Matlab paths. Open Matlab, select the **Home** tab, and add the **ExploreASL** directory including its subfolders using the **Set Path** option. Now change your working directory, using the **Current Folder** tab or the **cd** command, to the **ExploreASL** directory.

To run **ExploreASL** you have to type in the following command in the **Command Window**: `ExploreASL`. If you already created an **ASL-BIDS dataset** in sourcedata format, you can run the full default **ExploreASL** pipeline like this:

```matlab
DataParPath = 'C:\...\MY-BIDS-DATASET\DataParFile.json';
ImportModules = true;
ProcessModules = true;
bPause = false;
[x] = ExploreASL_Master(DataParPath, ImportModules, ProcessModules, bPause);
```

----
### How to run a compiled ExploreASL Version


To compile ExploreASL you have to run the `xASL_adm_MakeStandalone.m` script. If necessary, you can also ask the developer team for a specific compiled version. Providing a compiled version for every operating system and corresponding Matlab version is currently not feasible for us. Please feel free to ask us for help though!
A compiled version of ExploreASL always requires the corresponding Matlab Runtime. Please checkout the official [Matlab Documentation](https://mathworks.com/products/compiler/matlab-runtime.html). Download the Matlab Runtime of the Matlab Version which was used for the compilation. Make sure to install the Matlab Runtime correctly. If you're using Windows, it is important that the path to the Matlab Runtime is added to Windows **PATH** during the installation.

#### Windows

Let's assume you want to run the compiled version of **ExploreASL latest**. Check the contents of the folder created by `xASL_adm_MakeStandalone.m`, which contains the compiled version. There should be a file called `xASL_latest.exe`. We recommend using the command line interface now. For this you can go to the address bar of your file explorer. Type in `cmd` to open the command prompt in the current folder. The following command will start **ExploreASL**, import the **ASL-BIDS dataset** in sourcedata format, and process the dataset of your `sourceStructure.json` file:

```console
xASL_latest.exe "c:\MY-BIDS-DATASET\sourceStructure.json" "1" "1"
```

The executable will extract all necessary data from the CTF archive within the folder. This is totally normal. Within the command window you should see that **ExploreASL** is starting to process the given dataset:

```console
xASL_latest.exe "c:\MY-BIDS-DATASET\sourceStructure.json" "1" "1"
(insert example here)
```

To test if it is possible to initialize **ExploreASL** without the processing of a dataset, you could run the following command:

```console
xASL_latest.exe "" "0" "0"
```

The usual **ExploreASL** parameters (`DataParPath`, `ImportModules`, `ProcessModules`, `bPause`, `iWorker`, `nWorkers`) have to be given to the compiled **ExploreASL** version as strings. The resulting output could look like this:

```console
xASL_latest.exe "" "0" "0"
(insert example here)
```

#### Linux

On Linux you can basically do the same as above. We can run the ExploreASL shell script with a specified Matlab MCR (here we use **version 96** e.g.) using the following command:

```console
./run_xASL_latest.sh /usr/local/MATLAB/MATLAB_Runtime/v96/ "" "0" "0"
```

Using the options `"" "0" "0"` we initialize **ExploreASL**, but do not process a dataset. To run a dataset, we have to switch the `ImportModules` and/or the `ProcessModules` parameter from 0 to 1 and pass a path for the DataParPath. This could look something like this:

```console
./run_xASL_latest.sh /usr/local/MATLAB/MATLAB_Runtime/v96/ "/home/TestDataSet/analysis/DataParFile.json" "1" "1"
(insert example here)
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

To start a docker container of **ExploreASL v1.6.0** e.g., you can use the following command:

```console
docker run -e DATAPARFILE="TestDataSet/sourceStructure.json"
       -e IMPORTMODULES="1" -e PROCESSMODULES="1"
       -v /home/.../incoming:/data/incoming 
       -v /home/.../outgoing:/data/outgoing xasl:1.6.0
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
[x] = ExploreASL_Initialize;
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



