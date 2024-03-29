# Submodules of the ASL Module

----
### xASL\_wrp\_CreateAnalysisMask.m

**Format:**

```matlab
xASL_wrp_CreateAnalysisMask(x)
```

**Description:**

This function creates an analysis mask with the following steps:
0. Create FoV mask (native & MNI spaces)
1. Detect negative vascular signal (native & MNI spaces, within pGM>0.5)
2. Detect peak vascular signal (native & MNI spaces, within pGM==80% percentile on ASL image)
3A. Brainmasking & FoV-masking native space
3B. Brainmasking & FoV-masking standard space
3C. Save brain mask for image processing (e.g., BASIL)
4. Save vascular masks
- Add WM vascular parts back to the mask (defined as pWM>0.8) & remove extracranial signal
In the WM, negative or peak signal is more expected from
noise rather than from intra-vascular signal, not many
big vessels exist in the WM
5. Create susceptibility mask (standard space only)
Here, we combine manually segmented susceptibility artifact regions in which
a population-based susceptibility probability map is created
This map is combined (i.e. taking the product) with the mean control & PWI
intensity distribution in these regions. This product
is thresholded with the average of the 75th percentile &
15% of the intensity (for a bit more robustness against individual variability in sinus sizes).



----
### xASL\_wrp\_PVC.m

**Format:**

```matlab
xASL_wrp_PVC(x)
```

**Description:**

This submodule performs partial volume correction (PVC) in native ASL space. It runs the Asllani's method
for partial volume correction by linear regression. It has two main extensions - first it uses a 3D kernel.
Second, it can use a Gaussian weights instead of the default flat kernel.

0. Admin and checking values and files
1. Getting the resolution and preparing parameters
2. Running PV-correction
3. Saving files and cleaning



----
### xASL\_wrp\_PreparePV.m

**Format:**

```matlab
xASL_wrp_PreparePV(x, bStandardSpace)
```

**Description:**

This submodule prepares partial volume correction (PVC) by creating correct PV maps in ASL resolution, in native space,
as well as in standard space if requested (to perform PVC in
standard space):

If bStandardSpace:

1. Create dummy upsampled ASL scan, for registration
2. Reslice pGM & pWM to hi-res ASL
3. Estimate effective spatial resolution of ASL
4. Smooth pGM & pWM to this spatial resolution
5. Move smoothed tissue posteriors to MNI space

else: run step 3 only, which will use the effective spatial resolution
that is default for the respective sequence:
2D EPI: [1 1 1] \* VoxelSize
3D GRASE: [1.1 1.1 1.38] \* VoxelSize
3D spiral: [4.3 4.4 10.1] \* VoxelSize (assuming GE uses the upsampled 2x2x4 mm
& run steps 1&2, but in native space these entail presmoothing &
downsampling.


----
### xASL\_wrp\_ProcessM0.m

**Format:**

```matlab
xASL_wrp_ProcessM0(x)
```

**Description:**

This submodule performs the image processing and
quantification of M0 maps (if they exist), with the following steps:

1. Register M0 to mean control if it exists
Before registration, contrast is equalized between the
images & biasfields are removed
2. Quantify M0 (correction scale slope & incomplete T1 recovery)
3. Masking & smoothing of M0 image, either using:
A) traditional technique (very sharp masking & little smoothing)
B) new ExploreASL-specific technique:
\* extrapolating outside mask (avoiding artifacts from too
much or too little masking)
\* smooth very extensively, to create a biasfield
(increases robustness & comparison of M0 between
sequences/patients)

Any M0 will be processed here. Even if part of the subjects does not
have an M0, since this can be later imputed, or an average population
M0 image could be used. Also, without background suppression and
without an M0, the MeanControl image is before saved as M0, and will
be processed here as well.

Note that any voxel-size differences between M0 and ASL are allowed
here: step 0B below rescales the PD inside an M0 voxel to the same as
the ASL resolution (assuming a voxel with half volume contains half
the amount of protons). The M0 is further processed in standard
space, and reduced to a biasfield. For the quantification in standard
space, the PWI and M0 are now by definition in the same space.
Also, the standard space M0 biasfield is resampled to the native PWI
space (at the end of step 3B), ensuring that both are also in the
same native space.


----
### xASL\_wrp\_Quantify.m

**Format:**

```matlab
xASL_wrp_Quantify(x [, PWI_Path, pathOutputCBF, M0Path, SliceGradientPath])
```

**Description:**

This submodule converts PWIs to quantified CBF maps (or
related derivatives). Note that we don't delete x.P.Path\_PWI4D here, as
this NIfTI file may be needed by xASL\_wrp\_VisualQC\_ASL.m

1. Load PWI
2. Prepare M0
3. Hematocrit & blood T1 correction
4. ASL & M0 parameters comparisons
5. Load SliceGradient
6. Initialize & define quantification parameters
7. Define labeling efficiency
8. Perform quantification
9. Save files
10. Perform FEAST quantification (if exist)
11. Create standard space masked image to visualize masking effect


----
### xASL\_wrp\_RealignASL.m

**Format:**

```matlab
xASL_wrp_RealignASL(x[, bASL])
```

**Description:**

This submodule estimates motion by spm\_realign, which uses a
rigid-body registration (3 translations, 3 rotations). It runs ENABLE to
reject outliers and provides a visualization. ENABLE, QC and visualizations
are based on the Net Displacement Vector (NDV) (in mm):
according to Pythagorean/Euclydian RMS
https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=ind1211&L=fsl&P=R34458&1=fsl&9=A&J=on&d=No+Match%3BMatch%3BMatches&z=4
view this link for image of rotation roll, pitch and yaw https://www.google.nl/search?q=rotation+pitch+yaw+roll&espv=2&tbm=isch&imgil=LW3Nn1K-L6Oc7M%253A%253B-aSyykkRityJoM%253Bhttp%25253A%25252F%25252Fwww.grc.nasa.gov%25252FWWW%25252Fk-12%25252Fairplane%25252Frotations.html&source=iu&usg=\_\_MlLQ5VuyRbm6kZP0vBJlPxmfbkw%3D&sa=X&ei=TWfjU4WcK4bqyQPqu4Fo&ved=0CD8Q9QEwBQ&biw=1680&bih=946#facrc=\_&imgdii=\_&imgrc=LW3Nn1K-L6Oc7M%253A%3B-aSyykkRityJoM%3Bhttp%253A%252F%252Fwww.grc.nasa.gov%252FWWW%252Fk-12%252Fairplane%252FImages%252Frotations.gif%3Bhttp%253A%252F%252Fwww.grc.nasa.gov%252FWWW%252Fk-12%252Fairplane%252Frotations.html%3B709%3B533

This submodule performs the following steps:

1. Estimate motion
2. Calculate and plot position and motion parameters
3. Threshold-free spike definition (based on ENABLE, but with t-stats rather than the threshold p<0.05)
4. Remove spike frames from nifti


----
### xASL\_wrp\_RegisterASL.m

**Format:**

```matlab
xASL_wrp_RegisterASL(x)
```

**Description:**

This submodule registers ASL images to T1w space, by using a
combination of the registration techniques below. Note that in the
absence of raw structural files (i.e. T1.nii[.gz] or T1\_ORI.nii[.gz],
it will recreate dummy files from standard space to do this registration

M0-T1w rigid-body  -> this works well in 2D EPI sequences
PWI-pGM rigid-body -> this is robust across sequences with different
readouts and consequently different effective spatial resolutions. With
low spatial resolution (e.g. GE 3D spiral product sequence), M0-T1w
registration may not work, but PWI-pGM will work.
PWI-pGM registration fails with large (vascular) artifacts, therefore
this is performed only with relatively low spatial CoV.
PWI-pGM affine     -> If the spatial CoV is sufficiently low, this can
improve the registration

These images are registered to ASL templates that were inversely
transformed from MNI to the T1w space (& resampled to the ASL space)
As this would have an ever higher similarity with the M0 & PWI

This submodule performs the following steps:
0.    Administration:
- A. ASL4D is dealth with, if motion peaks were removed this is called
despiked\_ASL4D
- B. a default "OtherList" is specified. This is used every
registration instance, except for removing the ref and src NIfTIs
used in the registration instance. Also, inside the registration
function the unexisting OtherList NIfTIs are skipped
- C. Define paths to the ASL templates
- D. Previous registration output files are removed
- E. Allow registration without structural data
- F. native->MNI transformation flow field y\_T1.nii is smoothed to the
effective ASL resolution y\_ASL.nii
- G. Registration contrasts are dealth with:
x.modules.asl.bRegistrationContrast - specifies the image contrast used for
registration (OPTIONAL, DEFAULT = 2):
- 0 = Control->T1w
- 1 = CBF->pseudoCBF from template/pGM+pWM
(skip if sCoV>0.667)
- 2 = automatic (mix of both)
- 3 = option 2 & force CBF->pseudoCBF irrespective of sCoV or Tanimoto coefficient
- H. Dummy src NIfTIs are created:
mean\_control.nii to register with T1w
mean\_PWI\_Clipped.nii to register with pseudoCBF
- I. Create reference images, downsampled pseudoTissue

1.    Registration Center of Mass
2.    Registration ASL -> anat (Control->T1w)
(this step is only applied if it improves the Tanimoto coefficient)
3.    Registration CBF->pseudoCBF
(this step is only applied if it improves the Tanimoto coefficient). Also, this step is only
applied if the spatial CoV<0.67. Note that this is usually the case
for 3D scans because of their lower effective spatial resolution.

x.modules.asl.bAffineRegistration - specifies the ASL-T1w rigid-body registration is followed up by an affine
registration (OPTIONAL, DEFAULT = 0)
- 0 = affine registration disabled
- 1 = affine registration enabled
- 2 = affine registration automatically chosen based on
spatial CoV of PWI
x.modules.asl.bDCTRegistration - Specifies if to include the DCT registration on top of Affine, all other requirements for
affine are thus also taken into account (OPTIONAL, DEFAULT = 0)
- 0 = DCT registration disabled
- 1 = DCT registration enabled if affine enabled and conditions for affine passed
- 2 = DCT enabled as above, but use PVC on top of it to get the local intensity scaling right


----
### xASL\_wrp\_ResampleASL.m

**Format:**

```matlab
xASL_wrp_ResampleASL(x)
```

**Description:**

This submodule resamples native space NIfTIs to standard space, using the deformation fields computed in the structural module
after smoothing these transformation fields to the ASL resolution.
The applied interpolation is a combination of all transformations (e.g. motion correction, registration to
T1w, and transformation of T1w to standard space. This submodule performs the following steps:

0. Administration
1. Warp TopUp QC files
2. Create slice gradient image for quantification reference, in case of 2D ASL
3. Resample ASL time series to MNI space
4. Resample to native space (applying any motion correction or registration)
5. Bilateral filter (currently disabled)
6. Create mean control image, if available, in native & standard space
7. Clone mean control image to be used as pseudo-M0 (if x.Q.M0==UseControlAsM0)
8. Pair-wise subtraction & saving PWI & PWI4D in both spaces
9. Save PWI NIfTI & time-series-related maps (SD, SNR)
10. Delete temporary files
11. Report spatial CoV as QC


----
### xASL\_wrp\_VisualQC\_ASL.m

**Format:**

```matlab
xASL_wrp_VisualQC_ASL(x)
```

**Description:**

This submodule performs several visualizations for visual & quantitative QC.

1. After initial admin
2. It starts with making ASL NIfTIs ready for visualization
& conversion to DICOM (though skipped by default)
3. Then it performs a collection of visualizations
4. Visualizes results of the TopUp geometric distortion correction
5. Visualization of slice gradient
6. Visualization & calculation of temporal QC parameters
7. Compute DICE overlap/intersection of ASL brain in FoV & T1w, to calculate coverage
8. Summarize orientation & check left-right flips
9. Collect several other parameters & store in PDF overview


