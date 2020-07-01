

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

----
### xASL_adm_CheckSPM

#### Function
```matlab
function [spm_path, spm_version] = xASL_adm_CheckSPM(modality, proposed_spm_path, check_mode)
```

#### Description
Checks if the spm function exists and if the reported version matches our development version (SPM8 or SPM12). If the spm toolbox is not available yet, it will try the PROPOSED_SPM_PATH (if specified) or the user selected directory and add it to PATH. The function will fail if SPM cannot be found or if detecting an unsupported version.

----
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

----
### xASL_adm_CompareDataSets

#### Function
```matlab
function [RMS] = xASL_adm_CompareDataSets(RefAnalysisRoot,SourceAnalysisRoot,x,type,mutexState)
```

#### Description
Compares two ExploreASL datasets for reproducibility.

----
### xASL_adm_CompareLists.m

#### Function
```matlab
function [NewList] = xASL_adm_CompareLists(list1, list2)
```

#### Description
Compare 2 single dimension lists.

----
### xASL_adm_ConvertDate2Nr.m

#### Function
```matlab
function [Nr DayInYear] = xASL_adm_ConvertDate2Nr(TempDate)
```

#### Description
Converts date to number input mmdd -> output mm (with days in fractions/floating point). Inverse from ConvertNrDate.

----
### xASL_adm_ConvertNr2Time.m

#### Function
```matlab
function Time = xASL_adm_ConvertNr2Time(Nr)
```

#### Description
Converts number to time input hh (with minutes in fractions/floating point) -> output hhmm. Inverse from xASL_adm_ConvertTime2Nr.

----
### xASL_adm_ConvertSubjSess2Subj_Sess.m

#### Function
```matlab
function [iSubj iSess] = xASL_adm_ConvertSubjSess2Subj_Sess(nSessions, iSubjSess)
```

#### Description
Converts combined SubjectSession index to subject & session indices. Useful for data lists in ExploreASL.

----
### xASL_adm_ConvertTime2Nr.m

#### Function
```matlab
function Nr = xASL_adm_ConvertTime2Nr(Time)
```

#### Description
Converts time to number input hhmm -> output hh (with minutes in fractions/floating point). Inverse from xASL_adm_ConvertNr2Time.

----
### xASL_adm_CopyMoveFileList.m

#### Function
```matlab
function [List] = xASL_adm_CopyMoveFileList(OriDir, DstDir, StrRegExp, bMove, bDir, bRecursive, bOverwrite, bVerbose)
```

#### Description
Moves a file to a file, a file to a directory, or a directory to a directory. It keeps the initial extensions, no unzipping or zipping after the move. But it makes sure that only one of .nii and .nii.gz exists in the destination directory. Useful to split a large database.

----
### xASL_adm_CorrectName.m

#### Function
```matlab
function strOut = xASL_adm_CorrectName(strIn, bOption, strExclude)
```

#### Description
Finds and replaces all non-word characters either by empty space or by an underscore. Optionally leaves in few selected special characters. Note that if '\_' is excluded from replacement, but option 2 is on, then underscores are replaced anyway.

----
### xASL_adm_CreateCSVfile.m

#### Function
```matlab
function xASL_adm_CreateCSVfile(CSVfilename,CSVdata)
```

#### Description
Creates a CSV file that can be opened with excel from your data.

----
### xASL_adm_CreateFileReport.m

#### Function
```matlab
function x = xASL_adm_CreateFileReport(x, bHasFLAIR, bHasMoCo, bHasM0, bHasLongitudinal)
```

#### Description
Prints a summary of created files or the individual modules (i.e. Structural, Longiutudinal & ASL modules).
Provides a quick check to see what has been skipped, an whether all files are present.

This script iterates across Native space 1) subject and 2) session files, resampled 3) subject and 4) session files, 5) Lock files and 6) QC Figure files.

For all we perform a A) count of the files present, summarized in fileReportSummary.csv, and we B) list the missing files in
"Missing\*.csv" files

**PM:** simplify/optimize this code, to make filename variable changing, search within subject-directories, etc. Combine the parts searching for missing & summarizing count

----
### xASL_adm_DefineASLResolution.m

#### Function
```matlab
function x = xASL_adm_DefineASLResolution(x)
```

#### Description
...

----
### xASL_adm_DeleteFilePair.m

#### Function
```matlab
function filepaths = xASL_adm_DeleteFilePair(path, varargin)
```

#### Description
Delete the file given in PATH, and also deletes files with the same name, but with extension given in EXT1, and potentially also EXT2, EXT3... 

----
### xASL_adm_Dicom2Parms.m

#### Function
```matlab
function [parms, pathDcmDictOut] = xASL_adm_Dicom2Parms(imPar, inp, parmsfile, dcmExtFilter, bUseDCMTK, pathDcmDictIn)
```

#### Description
The function goes through the INP files, reads the DICOM or PAR/REC files and parses their headers.
It extracts the DICOM parameters important for ASL, makes sure they are in the correct format, if missing then replaces with default value, it also checks if the parameters are consistent across DICOM files for a single sequence.

----
### xASL_adm_FindByRegExp.m

#### Function
```matlab
function [tree, optionalTokens] = xASL_adm_FindByRegExp(root, dirSpecs, varargin)
```

#### Description
Recursively find files in the root directory according to the dirSpecs.

----
### xASL_adm_FindStrIndex.m

#### Function
```matlab
function INDEX = xASL_adm_FindStrIndex(ARRAY, STRING)
```

#### Description
Similar to find, but then for a cell array filled with strings. Only takes 4 dimensions.

----
### xASL_adm_GetFsList.m

#### Function
```matlab
function  RES = xASL_adm_GetFsList(strDirectory, strRegEx, bGetDirNames, bExcludeHidden, bIgnoreCase, nRequired)
```

#### Description
List files or directories from a given path. And optionally uses regular expressions to filter the result with options to exclude hidden files, ignore case, and set a minimal requirement on the number of results. Sorts the results at the end.

----
### xASL_adm_GetNumFromStr.m

#### Function
```matlab
function num = xASL_adm_GetNumFromStr(str)
```

#### Description
Obtains single number from string. CAVE there should only be one number!

----
### xASL_adm_GetPhilipsScaling.m

#### Function
```matlab
function scaleFactor = xASL_adm_GetPhilipsScaling(parms,header)
```

#### Description
This script provides the correct scaling factors for a NIfTI file. It checks the header of the NIfTI that normally has the same scaling as RescaleSlope in DICOM, it checks if dcm2nii (by the info in JSON) has already converted the scale slopes to floating point. And if not, the derive the correct scaling factor to be applied.

----
### xASL_adm_GetUserName.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_Hex2Num.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_LesionResliceList.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_Load4DMemMapping.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_LoadParms.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_LoadX.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_OtherListSPM.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_Par2Parms.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_ParReadHeader.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_Remove_1_SPM.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_ReplaceSymbols.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_ResetVisualizationSlices.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_SaveJSON.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_uiGetInput.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_UnzipOrCopy.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_Voxel2RealWorldCoordinates.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_adm_ZipFileList.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_bids_Add2ParticipantsTSV.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_bids_Dicom2JSON.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_bids_InsertJSONFields.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_bids_parms2BIDS.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_bids_PARREC2JSON.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_fsl_RunFSL.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_fsl_SetFSLdir.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_fsl_TopUp.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_import_json.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_imwrite.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_AddIM2QC.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_BilateralFilter.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CenterOfMass.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CleanupWMHnoise.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_ClipExtremes.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_colorbar.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_Column2IM.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CompareNIfTIResolutionXYZ.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_ComputeDice.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CreateASLDeformationField.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CreatePseudoCBF.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CreateSliceGradient.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CreateVisualFig.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CreateVisualLongReg.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CropParmsAcquire.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_CropParmsApply.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_DecomposeAffineTransformation.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_DetermineFlip.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_DilateErodeFull.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_DilateErodeSeparable.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_DilateErodeSphere.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_dilateROI.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_DummyOrientationNIfTI.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_EstimateResolution.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_Flip.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_FlipOrientation.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_FlipOrientation2.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_IM2Column.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_joinColormap.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_JointHist.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_Lesion2CAT.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_Lesion2Mask.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_M0ErodeSmoothExtrapolate.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_MaskNegativeVascularSignal.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_MaskPeakVascularSignal.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_Modulation.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_NormalizeLabelingTerritories.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_OverlapT1_ASL.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_PCA.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_PreSmooth.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_ProcessM0Conventional.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_ProjectLabelsOverData.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_PVCbspline.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_PVCkernel.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_ResampleLinearFair.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_RescaleLinear.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_RestoreOrientation.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_rotate.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_SkullStrip.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_Smooth3D.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_TileImages.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_TransformData2View.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_Upsample.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_VisualizeROIs.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_VisualQC_TopUp.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_im_ZeroEdges.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_ConvertM2JSON.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_DefaultEffectiveResolution.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_DefineStudyData.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_FileSystem.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_InitializeMutex.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_LabelColors.mat

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_LoadMetadata.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_LongitudinalRegistration.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_init_VisualizationSettings.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_io_CreateNifti.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_io_dcm2nii.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_io_DcmtkRead.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_io_MakeNifti4DICOM.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_io_PairwiseSubtraction.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_io_ReadTheDicom.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_io_SplitASL_M0.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_Iteration.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_num2str.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_AsymmetryIndex.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_CAT12_IQR.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_CollectParameters.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_CollectQC_ASL.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_CollectQC_func.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_CollectQC_Structural.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_CollectSoftwareVersions.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_CompareTemplate.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_ComputeFoVCoverage.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_ComputeNiftiOrientation.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_CreatePDF.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_FA_Outliers.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_ObtainQCCategoriesFromJPG.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_PCPStructural.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_PrintOrientation.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_TanimotoCoeff.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_temporalSNR.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_WADQCDC.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_qc_WADQC_GenerateDescriptor.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_quant_AgeSex2Hct.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_quant_FEAST.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_quant_GetControlLabelOrder.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_quant_Hct2BloodT1.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_quant_M0.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_quant_SinglePLD.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_spm_affine.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_spm_BiasfieldCorrection.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_spm_coreg.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_spm_deface.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_spm_deformations.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_AtlasForStats.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_ComputeDifferCoV.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_ComputeMean.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_ComputeSpatialCoV.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_EqualVariancesTest.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_fcdf.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_GetROIstatistics.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_MadNan.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_MeanSSIM.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_MultipleLinReg.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_PrintStats.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_PSNR.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_QuantileNan.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_RobustMean.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_ShapiroWilk.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_StdNan.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_SumNan.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_tcdf.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_ticdf.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_ttest.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_ttest2.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_stat_VarNan.m

#### Function
```matlab
...
```

#### Description
...

----
### xASL_str2num.m

#### Function
```matlab
...
```

#### Description
...


