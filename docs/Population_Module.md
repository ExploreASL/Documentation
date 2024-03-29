# Submodules of the Population Module

----
### xASL\_adm\_DeleteManyTempFiles.m

**Format:**

```matlab
xASL_adm_DeleteManyTempFiles(x)
```

**Description:**

This function removes as many files as possible.



----
### xASL\_adm\_GetPopulationSessions.m

**Format:**

```matlab
[nSessions, bSessionsMissing] = xASL_adm_GetPopulationSessions(x)
```

**Description:**

This function looks for the maximum amount of sessions that
are present in selected processed files present in the Population folder.

1. Determine which files to look for in the Population folder
2. Obtain list of session files in the Population folder
3. Determine unique amount of session numbers present in list
4. Set nSessions as highest unique session number
5. Check and provide warning of number of sesssions differs per subject


----
### xASL\_adm\_GzipAllFiles.m

**Format:**

```matlab
xASL_adm_GzipAllFiles(ROOT, bFolder, bUseLinux, pathExternal)
```

**Description:**

This function zips NIfTI files or folders recursively and deletes
the original file/folder after zipping.


----
### xASL\_im\_CreateAnalysisMask.m

**Format:**

```matlab
[x] = xASL_im_CreateAnalysisMask(x, Threshold)
```

**Description:**

This function takes the mean population-based probability
maps of masks, thresholds and combines them:

A. Creation GM, WM & WholeBrain masks by p>0.5
B. Create, combine & save vascular, susceptibity & FoV
masks:
- MaskVascular
- MaskSusceptibility = MaskSusceptibility & MaskFoV
C. Create & save VBA mask
- MaskAnalysis = MaskVascular & MaskSusceptibility
- x.S.VBAmask = MaskAnalysis & GMmask
D. Visualization: Creates a figure with columns being
following maps/masks overlaid over mean population T1w:
1. FoV probability 0-50% missing voxels
2. Vascular 0-7.5% missing voxels
3. Susceptibility 0-50% missing voxels
4. Analysis mask



----
### xASL\_qc\_SortBySpatialCoV.m

**Format:**

```matlab
xASL_qc_SortBySpatialCoV(x, Threshold1, Threshold2)
```

**Description:**

This function organizes the ASL QC images in //analysis/Population/ASLCheck
into CBF, vascular and artifactual contrast per the
spatial CoV thresholds defined above, in folders:
//analysis/Population/ASLCheck/1\_CBFContrast
//analysis/Population/ASLCheck/2\_VascularContrast
//analysis/Population/ASLCheck/3\_ArtifactualContrast
Invalid spatial CoV numbers (e.g. NaN) will go to:
//analysis/Population/ASLCheck/4\_Unknown\_sCoV
Note: these outputs need to be visually checked; but
pre-sorting them by spatial CoV puts them already in
categories that are quick to skim through and manually
correct by moving the images

The idea is then that only category 1 images are used
for perfusion (CBF) analyses, both categories 1 & 2 for the
vascular (sCoV) analyses, and the category 3 images excluded
from analysis.

PM: this code does not include multiple sessions per subject yet!
NB: this code uses the //analysis/Population/Stats/CoV\_qCBF\*TotalGM\*.csv file,
make sure that this file isn't edited!
-------------------------------------------------------------------------------------------------------------------------

----
### xASL\_stat\_ComputeWsCV.m

**Format:**

```matlab
xASL_stat_ComputeWsCV(x)
```

**Description:**

Calculates the within and between-subject
coefficient of variance (wsCV and bsCV respectively), to estimate the
power to detect effects.

This requires 4D images that have been split.



----
### xASL\_stat\_GetAcquisitionTime.m

**Format:**

```matlab
[x] = xASL_stat_GetAcquisitionTime(x)
```

**Description:**

This functions collects the DICOM field AcquisitionTime from
each json sidecar (& parms.mat for backward compatibility) and saves them
in the participants.tsv. Additionally, it creates a AcquisitionTime histogram of the
full study, which can be useful to check time of scanning -> can
influence physiological CBF variability.

1. Collect times
2. Save times
3. Create time histogram



----
### xASL\_stat\_GetDICOMStatistics.m

**Format:**

```matlab
xASL_stat_GetDICOMStatistics(x, ScanType, HasSessions)
```

**Description:**

This functions prints DICOM metadata (e.g. parameters used
for quantification) and collects them in a single tsv (per BIDS).
Summarizes this for the total population.
Can be useful to detect software upgrades, where only slight
parameter changes can hint on quantification changes.
This function carries out the following steps:

1. Load & save individual parameter files
2. Print summary
3. Write TSV file
------------------------------------------------------------------------------------------------

----
### xASL\_stat\_GetMotionStatistics.m

**Format:**

```matlab
xASL_stat_GetMotionStatistics(x)
```

**Description:**

This functions collects motion stats, with the following steps:

1. Collect motion data
2. If no data, skip this function
3. Print motion vs exclusion overview
4. Add motion data to participants.tsv



----
### xASL\_stat\_GetROIstatistics.m

**Format:**

```matlab
[x] = xASL_stat_GetROIstatistics(x)
```

**Description:**

This function computes mean and spatial CoV for each ROI,
in a [1.5 1.5 1.5] mm MNI space,
with several ASL-specific adaptions:

0. Administration
0.a Manage masking
0.b Obtain ASL sequence
0.c Determine whether group mask exists
1. Skip ROI masks that are smaller than 1 mL
as this would be too noisy for ASL (ignored when x.S.IsASL==false)
2. Expand each ROI mask such that it has sufficient WM
content (skipped when IsASL==false)
3. Create for each ROI mask a left, right and bilateral copy
4. Iterate over all subjects:
- a) Load partial volume maps
- b) Correct for WMH SEGM -> IS THIS STILL REQUIRED???
- c) Load data
- d) Show ROIs projected on ASL image
- e) Actual data computations
Partial Volume Correction (PVC) options:
PVC==0 -> perform masking only, no regression
PVC==1 -> single compartment regression, for pGM
PVC==2 -> dual compartment regression for pGM & pWM (i.e. normal
PVC)
Here we mask out susceptibility artifacts (including
outside FoV) for all ASL computations, and also mask
out vascular artifacts for computing the mean.
5. Housekeeping

While other artifacts/FoV can be masked out on population
level (i.e. >0.95 subjects need to have a valid signal in a
certain voxel), vascular artifacts differ too much in their
location between subjects, so we mask this on subject-level.

Note that the words "mask" and "ROI" are used
interchangeably throughout this function, where they can
have a different or the same meaning

PM: WE COULD CHANGE THIS, INTO MASK BEING USED TO EXCLUDE
VOXELS AND ROI FOR INCLUDING VOXELS



----
### xASL\_stat\_GetRegistrationStatistics.m

**Format:**

```matlab
xASL_stat_GetRegistrationStatistics(x)
```

**Description:**

Loads the data from the study given in the QC\_collection\*.json files. Goes through all subjects and
sessions and prints the Tanimoto coefficients that define the quality of the registrations. Steps:

1. Load & extract parameters from individual parameter files
2. Write TSV file

------------------------------------------------------------------------------------------------

----
### xASL\_stat\_GetVolumeStatistics.m

**Format:**

```matlab
xASL_stat_GetVolumeStatistics(x)
```

**Description:**

This functions collects motion stats, with the following. Steps:

1. Collect structural volume data
2. Collect WMH data
3. Add stats in participants.tsv



----
### xASL\_wrp\_CreateBiasfield.m

**Format:**

```matlab
xASL_wrp_CreateBiasfield(x)
```

**Description:**

This function creates a smooth biasfield as intensity map to
normalize intensities between different
sequences/scanner/sites within a single study.
This is a simple pragmatic approach and is not validated,
but is the best we can do.

First acquires average additive & multiplicative factors for total GM,
then does a smooth voxel-wise rescaling. This doesn't make an assumption
whether site or sequence differences are additive or multiplicative,
but rather fits them both. Global scaling it performed to GM CBF == 60 mL/100g/min

NB: make sure that sequence resolution differences have been
taken in account before creating these biasfields
PM: add normalization of between-subjects SD as well.
PM: are there other things we can normalize?

Note that NaNs in the images are ignored (so for each voxel
only the subjects without a NaN in the voxel are considered)

This function has the following sections:

0. Admin
1. Load images & store site-average CBF images
2. Smooth the site-average CBF images to biasfields
3. Create average biasfield
4. Create new bias-fields, masked & expand
5. Save biasfields
6. Create backup of each CBF image before applying biasfields
7. Print QC reference images
8. Rescale CBF images (apply the biasfield correction)
9. Re-load & store site-average CBF images
10. Perform site-wise SD correction & save new CBF images



----
### xASL\_wrp\_CreatePopulationTemplates.m

**Format:**

```matlab
xASL_wrp_CreatePopulationTemplates(x[, bSaveUnmasked, Compute4Sets, SpecificScantype, bSkipWhenMissingScans, bRemoveOutliers, FunctionsAre])
```

**Description:**

This function creates simple parametric images, a.k.a. templates, for
different image/scan types, on population level, as well as for different
sets (e.g. sites/scanners/cohorts, etc) if specified. By default these
images are masked, and transformed into a single column, for quick
computations with low memory usage. The default parametric images that
are created are the mean, between-subject SD, and the maximal intensity
projection (MIP). The latter can e.g. identify intra-vascular signal that
is similar between different subjects. Other parametric maps can be
decommented (now commented out for speed).

Any new addition to participants.tsv will be recognized and loaded, for
the generation of new parametric maps for groups specifically
(needs to be set in input argument bCompute4Sets)

If a set only includes a combination of the following SetOptions:
left, right, l, r, n/a, NaN (irrespective of capitals)
each image with option right/r, will be flipped in the left-right
direction, and left/right will not be treated as separate groups.
This function performs the following steps:

1. Define images/scantypes (if they are not defined by input argument SpecificScantype)
2. Iterate over scan types & sessions
3. Check availability images
4. Load images
5. Remove outliers
6. Compute templates for all subjects together (only for bilateral images)
7. Compute templates for individual sets


----
### xASL\_wrp\_GetROIstatistics.m

**Format:**

```matlab
xASL_wrp_GetROIstatistics(x)
```

**Description:**

This wrapper organizes the computation of statistics for different ROIs
in a [1.5 1.5 1.5] mm MNI space:

1. Load the atlas: xASL\_stat\_AtlasForStats
2. Organize TSV output name: using x.S.output\_ID
3. Obtain the ROI statistics: xASL\_stat\_GetROIstatistics
4. Print statistics in TSV files: xASL\_stat\_PrintStats



