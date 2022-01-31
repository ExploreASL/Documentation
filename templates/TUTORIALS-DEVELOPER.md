
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

The ExploreASL `x` structure is the main object used to define pipeline settings. Besides settings you can also find processing and meta data there. Note that developer settings are listed here. Basic and advanced settings are listed in the [Processing Tutorials](./Tutorials-Processing/).

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

The `x.S` subfield contains **masking & atlas** related parameters. Full list of parameters is provided here.

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
