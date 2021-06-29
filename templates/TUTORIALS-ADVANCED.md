
# Tutorials (Advanced)

----
## Basic Image Processing


----
## Basic NIFTI Input & Output

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

In a future **ExploreASL** version we want to add a custom atlases directory, where you can add your own atlas & metadata file including a description of how to do this.





