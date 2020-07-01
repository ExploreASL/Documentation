

## List of ExploreASL Functions

----
### xASL_adm_CatchNumbersFromString

#### Function
```matlab
function [OutputNumber] = xASL_adm_CatchNumbersFromString(InputString)
```

#### Description
...

----
### xASL_adm_CheckFileCount

#### Function
```matlab
function [result, files] = xASL_adm_CheckFileCount(path, expr, mincount, failifmissing)
```

#### Description
Checks the given PATH for files corresponding to the SPM_SELECT regular expression EXPR. Returns if the number of files is equal to or higher than MINCOUNT. If FAILIFMISSING is true and not enough files, then throw and error. If everything goes ok and second output argument is specified then return also the list of files.

-----
### xASL_adm_CheckPermissions

#### Function
```matlab
function [FilesList, FilesExeList, FoldersList] = xASL_adm_CheckPermissions(InputPath, FilesExecutable)
```

#### Description
This function does a recursive search through the root folder & makes a list of the attributes of all files and folders.
It tries to reset the attributes to what we desire, which is by default:

* 664 for files (meaning only reading & writing for users & group, & read-only for others) 
* 775 for folders (meaning reading, writing & opening for current user & current group, & for others only reading & opening) 

For executable files we also want 775. Note that the permission to 'execute a folder' means opening them


### xASL_adm_CheckSPM

#### Function
```matlab
function [spm_path, spm_version] = xASL_adm_CheckSPM(modality, proposed_spm_path, check_mode)
```

#### Description
Checks if the spm function exists and if the reported version matches our development version (SPM8 or SPM12). If the spm toolbox is not available yet, it will try the PROPOSED_SPM_PATH (if specified) or the user selected directory and add it to PATH. The function will fail if SPM cannot be found or if detecting an unsupported version.

### xASL_adm_CleanUpBeforeRerun

#### Function
```matlab
function xASL_adm_CleanUpBeforeRerun(AnalysisDir, iModule, bRemoveWMH, bAllSubjects, SubjectID, SessionID)
```

#### Description
This function (partly) reverts previous ExploreASL runs, deleting derivatives, while keeping raw data intact. if bAllSubjects==true, then all subjects and all module derivatives will be removed. This function performs the following steps:

1. If a Population folder doesn't exist yet but dartel does, rename it
2. Remove whole-study data files in AnalysisDir if bAllSubjects
3. Remove lock files/folders for reprocessing
4. Restore backupped _ORI (original) files
5. Delete native space CAT12 temporary folders (always, independent of iModule)
6. Remove native space files for iModule
7. Remove standard space files for iModule
8. Remove population module files
9. Remove or clean up stored x-struct & QC file -> THIS HAS NO SESSION SUPPORT YET

### xASL_adm_CompareDataSets

#### Function
```matlab
function [RMS] = xASL_adm_CompareDataSets(RefAnalysisRoot,SourceAnalysisRoot,x,type,mutexState)
```

#### Description
Compares two ExploreASL datasets for reproducibility.

### xASL_adm_CompareLists.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_ConvertDate2Nr.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_ConvertNr2Time.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_ConvertSubjSess2Subj_Sess.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_ConvertTime2Nr.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_CopyMoveFileList.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_CorrectName.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_CreateCSVfile.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_CreateFileReport.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_DefineASLResolution.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_DeleteFilePair.m

#### Function
```matlab
...
```

#### Description
...

### xASL_adm_Dicom2Parms.m

### xASL_adm_FindByRegExp.m

### xASL_adm_FindStrIndex.m

### xASL_adm_GetFsList.m

### xASL_adm_GetNumFromStr.m

### xASL_adm_GetPhilipsScaling.m

### xASL_adm_GetUserName.m

### xASL_adm_Hex2Num.m

### xASL_adm_LesionResliceList.m

### xASL_adm_Load4DMemMapping.m

### xASL_adm_LoadParms.m

### xASL_adm_LoadX.m

### xASL_adm_OtherListSPM.m

### xASL_adm_Par2Parms.m

### xASL_adm_ParReadHeader.m

### xASL_adm_Remove_1_SPM.m

### xASL_adm_ReplaceSymbols.m

### xASL_adm_ResetVisualizationSlices.m

### xASL_adm_SaveJSON.m

### xASL_adm_uiGetInput.m

### xASL_adm_UnzipOrCopy.m

### xASL_adm_Voxel2RealWorldCoordinates.m

### xASL_adm_ZipFileList.m

### xASL_bids_Add2ParticipantsTSV.m

### xASL_bids_Dicom2JSON.m

### xASL_bids_InsertJSONFields.m

### xASL_bids_parms2BIDS.m

### xASL_bids_PARREC2JSON.m

### xASL_fsl_RunFSL.m

### xASL_fsl_SetFSLdir.m

### xASL_fsl_TopUp.m

### xASL_import_json.m

### xASL_imwrite.m

### xASL_im_AddIM2QC.m

### xASL_im_BilateralFilter.m

### xASL_im_CenterOfMass.m

### xASL_im_CleanupWMHnoise.m

### xASL_im_ClipExtremes.m

### xASL_im_colorbar.m

### xASL_im_Column2IM.m

### xASL_im_CompareNIfTIResolutionXYZ.m

### xASL_im_ComputeDice.m

### xASL_im_CreateASLDeformationField.m

### xASL_im_CreatePseudoCBF.m

### xASL_im_CreateSliceGradient.m

### xASL_im_CreateVisualFig.m

### xASL_im_CreateVisualLongReg.m

### xASL_im_CropParmsAcquire.m

### xASL_im_CropParmsApply.m

### xASL_im_DecomposeAffineTransformation.m

### xASL_im_DetermineFlip.m

### xASL_im_DilateErodeFull.m

### xASL_im_DilateErodeSeparable.m

### xASL_im_DilateErodeSphere.m

### xASL_im_dilateROI.m

### xASL_im_DummyOrientationNIfTI.m

### xASL_im_EstimateResolution.m

### xASL_im_Flip.m

### xASL_im_FlipOrientation.m

### xASL_im_FlipOrientation2.m

### xASL_im_IM2Column.m

### xASL_im_joinColormap.m

### xASL_im_JointHist.m

### xASL_im_Lesion2CAT.m

### xASL_im_Lesion2Mask.m

### xASL_im_M0ErodeSmoothExtrapolate.m

### xASL_im_MaskNegativeVascularSignal.m

### xASL_im_MaskPeakVascularSignal.m

### xASL_im_Modulation.m

### xASL_im_NormalizeLabelingTerritories.m

### xASL_im_OverlapT1_ASL.m

### xASL_im_PCA.m

### xASL_im_PreSmooth.m

### xASL_im_ProcessM0Conventional.m

### xASL_im_ProjectLabelsOverData.m

### xASL_im_PVCbspline.m

### xASL_im_PVCkernel.m

### xASL_im_ResampleLinearFair.m

### xASL_im_RescaleLinear.m

### xASL_im_RestoreOrientation.m

### xASL_im_rotate.m

### xASL_im_SkullStrip.m

### xASL_im_Smooth3D.m

### xASL_im_TileImages.m

### xASL_im_TransformData2View.m

### xASL_im_Upsample.m

### xASL_im_VisualizeROIs.m

### xASL_im_VisualQC_TopUp.m

### xASL_im_ZeroEdges.m

### xASL_init_ConvertM2JSON.m

### xASL_init_DefaultEffectiveResolution.m

### xASL_init_DefineStudyData.m

### xASL_init_FileSystem.m

### xASL_init_InitializeMutex.m

### xASL_init_LabelColors.mat

### xASL_init_LoadMetadata.m

### xASL_init_LongitudinalRegistration.m

### xASL_init_VisualizationSettings.m

### xASL_io_CreateNifti.m

### xASL_io_dcm2nii.m

### xASL_io_DcmtkRead.m

### xASL_io_MakeNifti4DICOM.m

### xASL_io_PairwiseSubtraction.m

### xASL_io_ReadTheDicom.m

### xASL_io_SplitASL_M0.m

### xASL_Iteration.m

### xASL_num2str.m

### xASL_qc_AsymmetryIndex.m

### xASL_qc_CAT12_IQR.m

### xASL_qc_CollectParameters.m

### xASL_qc_CollectQC_ASL.m

### xASL_qc_CollectQC_func.m

### xASL_qc_CollectQC_Structural.m

### xASL_qc_CollectSoftwareVersions.m

### xASL_qc_CompareTemplate.m

### xASL_qc_ComputeFoVCoverage.m

### xASL_qc_ComputeNiftiOrientation.m

### xASL_qc_CreatePDF.m

### xASL_qc_FA_Outliers.m

### xASL_qc_ObtainQCCategoriesFromJPG.m

### xASL_qc_PCPStructural.m

### xASL_qc_PrintOrientation.m

### xASL_qc_TanimotoCoeff.m

### xASL_qc_temporalSNR.m

### xASL_qc_WADQCDC.m

### xASL_qc_WADQC_GenerateDescriptor.m

### xASL_quant_AgeSex2Hct.m

### xASL_quant_FEAST.m

### xASL_quant_GetControlLabelOrder.m

### xASL_quant_Hct2BloodT1.m

### xASL_quant_M0.m

### xASL_quant_SinglePLD.m

### xASL_spm_affine.m

### xASL_spm_BiasfieldCorrection.m

### xASL_spm_coreg.m

### xASL_spm_deface.m

### xASL_spm_deformations.m

### xASL_stat_AtlasForStats.m

### xASL_stat_ComputeDifferCoV.m

### xASL_stat_ComputeMean.m

### xASL_stat_ComputeSpatialCoV.m

### xASL_stat_EqualVariancesTest.m

### xASL_stat_fcdf.m

### xASL_stat_GetROIstatistics.m

### xASL_stat_MadNan.m

### xASL_stat_MeanSSIM.m

### xASL_stat_MultipleLinReg.m

### xASL_stat_PrintStats.m

### xASL_stat_PSNR.m

### xASL_stat_QuantileNan.m

### xASL_stat_RobustMean.m

### xASL_stat_ShapiroWilk.m

### xASL_stat_StdNan.m

### xASL_stat_SumNan.m

### xASL_stat_tcdf.m

### xASL_stat_ticdf.m

### xASL_stat_ttest.m

### xASL_stat_ttest2.m

### xASL_stat_VarNan.m

### xASL_str2num.m


