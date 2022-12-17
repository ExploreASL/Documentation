# Functions

## Administration

----
### xASL\_adm\_BreakString.m

**Format:**

```matlab
[resultText] = xASL_adm_BreakString(textToPrint,SymbolToFill, bColor, bNewLines, bPrintImmediately)
```

**Description:**

Pads symbols left and right of a string. By default it adds new lines, and colors, and prints the string, with
a possibility to turn each of these options off.



----
### xASL\_adm\_CatchNumbersFromString.m

**Format:**

```matlab
[OutputNumber] = xASL_adm_CatchNumbersFromString(InputString)
```

**Description:**

Extracts a number from a char array.


----
### xASL\_adm\_CheckFileCount.m

**Format:**

```matlab
[result, files] = xASL_adm_CheckFileCount(path, expr[, mincount, failifmissing])
[result]        = xASL_adm_CheckFileCount(...)
```

**Description:**

Checks the given **PATH** for files corresponding to the **SPM\_SELECT** regular expression **EXPR**.
Returns if the number of files is equal to or higher than **MINCOUNT**. If **FAILIFMISSING** is true
and not enough files, then throw and error. If everything goes ok and second output argument is specified
then return also the list of files.

----
### xASL\_adm\_CheckPermissions.m

**Format:**

```matlab
[FilesList, FilesExeList, FoldersList] = xASL_adm_CheckPermissions(InputPath[, FilesExecutable])
```

**Description:**

This function does a recursive search through the root
folder & makes a list of the attributes of all files and folders.
It tries to reset the attributes to what we desire, which is by default:

- 664 for files (meaning only reading & writing for users & group, & read-only for others)
- 775 for folders (meaning reading, writing & opening for current user & current group, & for others only reading & opening)

For executable files we also want 775.
Note that the permission to 'execute a folder' means opening them.

- DataOK checks data permissions.
- ExeOK checks executable permissions.
- DataOK also includes executable permissions for folders.
- This runs recursively (but currently skips the contents of the root-folder) .


----
### xASL\_adm\_CheckSPM.m

**Format:**

```matlab
[spm_path, spm_version] = xASL_adm_CheckSPM([modality, proposed_spm_path, check_mode])
[spm_path]              = xASL_adm_CheckSPM(...)
xASL_adm_CheckSPM(...)
```

**Description:**

Checks if the spm function exists and if the reported version matches our development
version (**SPM8** or **SPM12**). If the spm toolbox is not available yet, it will try the
**PROPOSED\_SPM\_PATH** (if specified) or the user selected directory and add it to **PATH**.
The function will fail if **SPM** cannot be found or if detecting an unsupported version.

----
### xASL\_adm\_CleanUpBeforeRerun.m

**Format:**

```matlab
xASL_adm_CleanupBeforeCompleteRerun(AnalysisDir, iModule, bRemoveWMH, bAllSubjects, SubjectID)
```

**Description:**

This function (partly) reverts previous ExploreASL runs,
deleting derivatives, while keeping raw data intact.
if bAllSubjects==true, then all subjects and all module
derivatives will be removed. This function performs the
following steps:

1. If a Population folder doesn't exist yet but dartel does, rename it
2. Remove whole-study data files in AnalysisDir if bAllSubjects
3. Remove lock files/folders for reprocessing
4. Restore backupped \_ORI (original) files
5. Delete native space CAT12 temporary folders (always, independent of iModule)
6. Remove native space files for iModule
7. Remove standard space files for iModule
8. Remove population module files
9. Remove or clean up stored x-struct & QC file -> THIS HAS NO SESSION SUPPORT YET

NB: still need to add xASL\_module\_func & xASL\_module\_dwi for EPAD


----
### xASL\_adm\_CleanUpX.m

**Format:**

```matlab
x = xASL_adm_CleanUpX(x)
```

**Description:**

Clean-Up before processing pipeline.



----
### xASL\_adm\_CompareDataSets.m

**Format:**

```matlab
[RMS] = xASL_adm_CompareDataSets(RefAnalysisRoot,SourceAnalysisRoot,x,type,mutexState)
```

**Description:**

Compare data sets is used to ...

- type 0: Only save
- type 1: Save and evaluate
- type 2: Only evaluate



----
### xASL\_adm\_CompareLists.m

**Format:**

```matlab
[NewList] = xASL_adm_CompareLists(list1, list2)
```

**Description:**

This script compares two single dimension lists.



----
### xASL\_adm\_ConvertDate2Nr.m

**Format:**

```matlab
[Nr DayInYear] = xASL_adm_ConvertDate2Nr(TempDate)
```

**Description:**

Converts date to number input mmdd -> output mm (with days in fractions/floating point).
Inverse from ConvertNrDate.



----
### xASL\_adm\_ConvertNr2Time.m

**Format:**

```matlab
Time = xASL_adm_ConvertNr2Time(Nr)
```

**Description:**

Converts number to time input hh (with minutes in fractions/floating point) -> output hhmm.
Inverse from xASL\_adm\_ConvertTime2Nr.



----
### xASL\_adm\_ConvertSubjSess2Subj\_Sess.m

**Format:**

```matlab
[iSubj iSess] = xASL_adm_ConvertSubjSess2Subj_Sess(nSessions, iSubjSess)
```

**Description:**

Converts combined SubjectSession index to subject & session
indices. Useful for data lists in ExploreASL.



----
### xASL\_adm\_ConvertTime2Nr.m

**Format:**

```matlab
Nr = xASL_adm_ConvertTime2Nr(Time)
```

**Description:**

Converts time to number input hhmm -> output hh (with
minutes in fractions/floating point).
Inverse from xASL\_adm\_ConvertNr2Time.



----
### xASL\_adm\_CopyMoveFileList.m

**Format:**

```matlab
[List] = xASL_adm_CopyMoveFileList(OriDir, DstDir, StrRegExp, bMove[, bDir, bRecursive, bOverwrite, bVerbose])
```

**Description:**

Moves a file to a file, a file to a directory, or a directory to a directory.
It keeps the initial extensions, no unzipping or zipping after the move.
But it makes sure that only one of **.nii** and **.nii.gz** exists in the destination directory.
Useful to split a large database.


----
### xASL\_adm\_CorrectName.m

**Format:**

```matlab
strOut = xASL_adm_CorrectName(strIn[, bOption, strExclude])
```

**Description:**

Finds and replaces all non-word characters either by empty space or by an underscore.
Optionally leaves in few selected special characters. Note that if '\_' is excluded from
replacement, but option 2 is on, then underscores are replaced anyway.


----
### xASL\_adm\_CreateFileReport.m

**Format:**

```matlab
x = xASL_adm_CreateFileReport(x, bHasFLAIR, bHasMoCo, bHasM0, bHasLongitudinal)
```

**Description:**

Prints a summary of created files or the individual modules
(i.e. Structural, Longiutudinal & ASL modules). Provides a quick check to
see what has been skipped, an whether all files are present.

This script iterates across:
Native space 1) subject and 2) session files,
Resampled 3) subject and 4) session files,
5) Lock files and 6) QC Figure files.

For all we perform a:

- A) Count of the files present, summarized in FileReportSummary.csv
- B) List of the missing files in "Missing\*.csv" files

PM: Simplify/optimize this code, to make filename variable changing,
search within subject-directories, etc. Combine the parts searching for
missing & summarizing count.



----
### xASL\_adm\_DefineASLResolution.m

**Format:**

```matlab
x = xASL_adm_DefineASLResolution(x)
```

**Description:**

If the parameters x.ResolutionEstimation == 1, it initializes the resolution with expected values
per sequence type and then runs the procedure xASL\_im\_ResolutionEstim to estimate the resolution from the
mismatch between ASL and structural data. For x.ResolutionEstimation == 0, xASL\_init\_DefaultEffectiveResolution
the educated guess is used for the estimated resolution using previous data and analyzis.



----
### xASL\_adm\_DefineASLSequence.m

**Format:**

```matlab
[x] = xASL_adm_DefineASLSequence(x)
```

**Description:**

This ExploreASL function tries to check what ASL sequence is
being processed, if this was not already defined in x.Q.Sequence.
It does so by checking known combinations of readout dimensionality
(x.Q.readoutDim) and Vendor, knowing the product sequences of the Vendors.


----
### xASL\_adm\_DeleteFilePair.m

**Format:**

```matlab
filepaths = xASL_adm_DeleteFilePair(path, ext1[, ext2 [, ext3 ...]])
xASL_adm_DeleteFilePair(path, ext1[, ext2 [, ext3 ...]])
```

**Description:**

Delete the file given in PATH, and also deletes files with the same name, but with extension
given in EXT1, and potentially also EXT2, EXT3...

----
### xASL\_adm\_Dicom2Parms.m

**Format:**

```matlab
[parms pathDcmDictOut] = xASL_adm_Dicom2Parms(imPar, inp[, parmsfile, dcmExtFilter, bUseDCMTK, pathDcmDictIn])
```

**Description:**

The function goes through the **INP** files, reads the **DICOM** or **PAR/REC** files and parses their headers.
It extracts the **DICOM** parameters important for ASL, makes sure they are in the correct format, if missing then
replaces with default value, it also checks if the parameters are consistent across **DICOM** files for a single sequence.


----
### xASL\_adm\_FindByRegExp.m

**Format:**

```matlab
xasl_adm_FindByRegExp(root, dirSpecs[, varargin])
```

**Description:**

Recursively find files in the root directory according to the dirSpecs.



----
### xASL\_adm\_FindStrIndex.m

**Format:**

```matlab
INDEX = xASL_adm_FindStrIndex(ARRAY, STRING)
```

**Description:**

Similar to find, but then for a cell array filled with strings.
Only takes 4 dimensions.



----
### xASL\_adm\_GetDeprecatedFields.m

**Format:**

```matlab
nameConversionTable = xASL_adm_GetDeprecatedFields()
```

**Description:**

This script is mainly used to improve backwards
compatibility. Check the usage in both xASL\_adm\_LoadX
and xASL\_io\_ReadDataPar.


----
### xASL\_adm\_GetFsList.m

**Format:**

```matlab
RES = xASL_adm_GetFsList([strDirectory, strRegEx, bGetDirNames, bExcludeHidden, bIgnoreCase, nRequired])
```

**Description:**

List files or directories from a given path. And optionally uses regular expressions to filter the result
with options to exclude hidden files, ignore case, and set a minimal requirement on the number of results.
Sorts the results at the end.

----
### xASL\_adm\_GetNumFromStr.m

**Format:**

```matlab
num = xASL_adm_GetNumFromStr(str)
```

**Description:**

Obtains single number from string.
**CAVE** there should only be one number!



----
### xASL\_adm\_GetPhilipsScaling.m

**Format:**

```matlab
scaleFactor = xASL_adm_GetPhilipsScaling(parms, header)
```

**Description:**

This script provides the correct scaling factors for a NIfTI file. It checks the header of the NIfTI
that normally has the same scaling as RescaleSlope in DICOM, it checks if dcm2nii (by the info in JSON)
has already converted the scale slopes to floating point. And if not, the derive the correct
scaling factor to be applied.
The function works with Philips-specific scale-slopes:
RWVSlope = Real world value slope (0040,9225)
MRScaleSlope (2005,100e), (2005,110e), (2005,120e)
RescaleSlopeOriginal (2005,0x140a), (2005,110a)
These are different from the standard slopes in DICOM that are used as display slopes in Philips:
RescaleSlope (0028,1053)
With new version of dcm2niix-20220720 and ExploreASL-1.10.0, dcm2niix does RWVSlope scalings correctly and we do not have to fix that.


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
### xASL\_adm\_GetUserName.m

**Format:**

```matlab
UserName = xASL_adm_GetUserName()
```

**Description:**

Get the name of the current user.



----
### xASL\_adm\_Hex2Num.m

**Format:**

```matlab
outNum = xASL_adm_hex2num(inStr)
```

**Description:**

Takes a hexadecimal string and converts it to number or string. Works
also when the string contains escape characters, and for single-floats and
for a little and big endian. If containing 8 and less
characters than treat as float, if more than as double.


----
### xASL\_adm\_LesionResliceList.m

**Format:**

```matlab
[INname, OUTname] = xASL_wrp_LesionResliceList(x,bLesion_T1,bLesion_FLAIR,bROI_T1,bROI_FLAIR)
```

**Description:**

Creates list of structural image paths to reslice.



----
### xASL\_adm\_LoadParms.m

**Format:**

```matlab
[Parms, x] = xASL_adm_LoadParms(ParmsPath[, x, bVerbose])
```

**Description:**

This function loads the internal memory x struct, any
legacy \*\_parms.mat sidecar, any \*.json BIDS sidecar, to use scan-specific
parameters for image processing/quantification. Also, per BIDS
inheritance, any x.S.SetsID parameters (from participants.tsv) are loaded
as well. This function performs the following steps:

1. Load .mat parameter file
2. Load JSON file
3. Deal with warnings
4. Find fields with scan-specific data in x.S.Sets, and use this if possible (per BIDS inheritance)
5. Sync Parms.\* with x.(Q.)\* (overwrite x/x.Q)
6. Fix M0 parameter if not set



----
### xASL\_adm\_LoadX.m

**Format:**

```matlab
[x[, IsLoaded]] = xASL_adm_LoadX(x[, Path_xASL, bOverwrite])
```

**Description:**

This function loads x.Output & x.Output\_im struct fields
from the x.mat on the hard drive & adds them to the current x struct
located in memory. If it didnt exist in the x.mat, it will
set IsLoaded to false, which can be catched externally & a warning issued if managed so
in the calling function. If it didnt exist in the memory x
struct, or bOverwrite was requested, the contents of x.mat
will be loaded to the memory x struct

1. Admin
2. Load X-struct from disc
3. Look for and update deprecated fields
4. Add fields from disc to the current x-struct


----
### xASL\_adm\_MergeStructs.m

**Format:**

```matlab
mergedStruct = xASL_adm_MergeStructs(mainStruct, secondaryStruct)
```

**Description:**

It merges two structures. It takes everything from the mainStruct and keep it as it is. It adds all fields from the secondaryStructure
to the main structure while checking for duplicates. It is not overwriting anything, all duplicit content is taken from mainStruct.
It works iteratively by correctly merging also the substructs.


----
### xASL\_adm\_OrderFields.m

**Format:**

```matlab
outStruct = xASL_adm_OrderFields(inStruct,orderStruct)
```

**Description:**

Order fields in the structure **inStruct** to match **orderStruct**,
unmatching fields in inStruct are copied as
they are at the end, unmatching fields in **orderStruct** are
ignored. This is just a cosmetic change and no values are
edited.



----
### xASL\_adm\_OtherListSPM.m

**Format:**

```matlab
[OtherListSPM, OtherListOut] = xASL_adm_OtherListSPM(OtherList, bList4D)
```

**Description:**

bPadComma1 is to add the ,1 to the end of the pathstring, which SPM uses
to assign the first image of a 4D image array (OPTIONAL, DEFAULT = true)
bList4D: boolean, true for listing multiple 4D volumes separately in the
list (OPTIONAL, DEFAULT=true).



----
### xASL\_adm\_ParReadHeader.m

**Format:**

```matlab
info =xASL_adm_ParReadHeader(filename)
```

**Description:**

Function for reading the header of a Philips Par /
Rec  MR V4.\* file.




----
### xASL\_adm\_RemoveDirectories.m

**Format:**

```matlab
xASL_adm_RemoveDirectories(root)
```

**Description:**

Script to remove all ExploreASL related paths.



----
### xASL\_adm\_RemoveLogFilesForRerun.m

**Format:**

```matlab
xASL_adm_RemoveLogFilesForRerun(rootDir);
```

**Description:**

Removes all log files from any directory containing .log files.




----
### xASL\_adm\_Remove\_1\_SPM.m

**Format:**

```matlab
[OtherList] = xASL_adm_Remove_1_SPM(OtherList)
```

**Description:**

Remove ,1 at end of OtherLists, if exists.
These are appended in CoregInit, OldNormalizeWrapper etc,
since this should allow 4rd dim (e.g. as in ASL4D).



----
### xASL\_adm\_ReplaceSymbols.m

**Format:**

```matlab
strOut = xASL_adm_ReplaceSymbols(strIn, symbolTable[, bracketLeft, bracketRight])
```

**Description:**

It takes the STRIN on input, then looks for symbols between BRACKETLEFT and BRACKETRIGHT and replaces these symbols in
in the string by the values provided in the SYMBOLTABLE as SYMBOLTABLE.SYMBOL, SYMBOLTABLE.D.SYMBOL, or SYMBOLTABLE.P.SYMBOL


----
### xASL\_adm\_ResetVisualizationSlices.m

**Format:**

```matlab
[x] = xASL_adm_ResetVisualizationSlices(x)
```

**Description:**

Removes any predefined slices that should be visualized,
allowing to show the default slices. Comes in handy when different
pipeline visualization parts are repeated.



----
### xASL\_adm\_SaveX.m

**Format:**

```matlab
xASL_adm_SaveX(x[, Path_xASL, bOverwrite])
```

**Description:**

This function saves the x.mat either to the predefined path or the the subject x.mat


----
### xASL\_adm\_UnzipOrCopy.m

**Format:**

```matlab
unpackedFiles = xASL_adm_UnzipOrCopy(srcDir, wildCard, destDir [, bOverwrite])
```

**Description:**

This is a simple wrapper function to (g)unzip one or more files to the specified destination
directory. Existing files or directories will not be overwritten, unless forced with bOverwrite.
A regular file-copy will be used if the source files don't have gz or zip filename extensions.


----
### xASL\_adm\_Voxel2RealWorldCoordinates.m

**Format:**

```matlab
[X Y Z] = xASL_adm_Voxel2RealWorldCoordinates(X,Y,Z,VoxelSize)
```

**Description:**

Converts MNI coordinates from voxel coordinates/indices.
Assumes X Y Z = LR LeftRight AP AnteriorPosterior IS InferiorSuperior.
VoxelSize should be [1 3]-sized input.


----
### xASL\_adm\_ZipFileList.m

**Format:**

```matlab
filepaths = xASL_adm_ZipFileList(strDirectory, strRegExp[, bRecurse, bUseGzip, nRequired])
xASL_adm_ZipFileList(strDirectory, strRegExp[, bRecurse, bUseGzip, nRequired])
```

**Description:**

Zip the files that match regular expression STRREGEXP in the given directory STRDIRECTORY.
Zips recursively if specified in BRECURSE. Zips all files unless the number is specified
by NREQUIRED, if the number is not met, then does not zip anything and throws an error.

----
### xASL\_adm\_uiGetInput.m

**Format:**

```matlab
[Parms] = xASL_adm_uiGetInput(Parms)
```

**Description:**

Checks whether input fields are present, or requests them.



## BIDS

----
### xASL\_bids\_Add2ParticipantsTSV.m

**Format:**

```matlab
xASL_bids_Add2ParticipantsTSV(DataIn, DataName, x, bOverwrite)
```

**Description:**

This function adds metadata/statistical variables to the
participants.tsv in the root/analysis folder, by the following steps.
This function will iterate over Data provided at DataIn and fill them
in the participants.tsv, overwriting if allowed.
Empty data is filled in as 'n/a', and the first column "participants\_id"
is sorted for participants.

This function runs the following steps:

1. Admin - Validate that there are not too many columns
2. Admin - Detect nSubjectsSessions
3. Admin - Load pre-existing participants.tsv or create one
4. Admin - Get column number of data
5. Add data to CellArray
6. Sort rows on subjects
7. Fill empty cells
8. Write data to participants.tsv



----
### xASL\_bids\_AddGeneratedByField.m

**Format:**

```matlab
xASL_bids_AddGeneratedByField(x, pathJSONin[, pathJSONout])
```

**Description:**

Add the generated by field to the struct.



----
### xASL\_bids\_BIDS2Legacy\_CompilePathsForCopying.m

**Format:**

```matlab
[bidsPar, TypeIs, pathOrig, pathDest] = xASL_bids_BIDS2Legacy_CompilePathsForCopying(bidsPar, TypeIs, ModalityIs, RunIs, iSubjSess, BIDS, TypeRunIndex, ModalityFields, pathLegacy_SubjectVisit)
```

**Description:**

Compile paths for BIDS to Legacy copying.



----
### xASL\_bids\_BIDS2Legacy\_ManageSidecars.m

**Format:**

```matlab
[bidsPar, pathOrig, pathDest, TypeIs] = xASL_bids_BIDS2Legacy_ManageSidecars(bidsPar, pathOrig, pathDest, TypeIs)
```

**Description:**

Manage JSON sidecars for BIDS2Legacy conversion.



----
### xASL\_bids\_BIDS2Legacy\_ParseModality.m

**Format:**

```matlab
xASL_bids_BIDS2Legacy_ParseModality(BIDS, bidsPar, SubjectVisit, iSubjSess, ModalitiesUnique, nModalities, bOverwrite, pathLegacy_SubjectVisit)
```

**Description:**

Parse modality for BIDS to Legacy conversion.



----
### xASL\_bids\_BIDS2Legacy\_ParseScanType.m

**Format:**

```matlab
xASL_bids_BIDS2Legacy_ParseScanType(modalityConfiguration, SubjectVisit, RunsUnique, RunsAre, bOverwrite, Reference, bidsPar, ModalityIs, iSubjSess, BIDS, ModalityFields, pathLegacy_SubjectVisit)
```

**Description:**

Parse scan type during BIDS to Legacy conversion.



----
### xASL\_bids\_BIDS2xASL\_CopyFile.m

**Format:**

```matlab
xASL_bids_BIDS2xASL_CopyFile(pathOrig, pathDest, bOverwrite)
```

**Description:**

Copy files for BIDS to Legacy conversion.



----
### xASL\_bids\_BIDSifyASLJSON.m

**Format:**

```matlab
jsonOut = xASL_bids_BIDSifyASLJSON(jsonIn, studyPar, headerASL)
```

**Description:**


It makes all the conversions to a proper BIDS structure, checks the existence of all BIDS fields, removes superfluous fields, checks all the conditions and orderes
the structure on the output. It works according to the normal BIDS, or ASL-BIDS definition

0. Admin
1. Obtain the dimensions of the ASL data
2. Take all the manually predefined fields from studyPar
3. Extract the scaling factors from the JSON header
4. Convert certain DICOM fields
5. Prioritize DICOM fields over the manually provided studyPar fields
6. Field check and name conversion
7. Check for time encoded sequence
8. Merge data from the Phoenix protocol
9. Background suppression check
10. SliceTiming check
11. Check if length of vector fields match the number of volumes
12. Reformat ASLcontext field
13. Verify TotalAcquiredPairs against ASLContext
14. Final field check


----
### xASL\_bids\_BIDSifyASLNII.m

**Format:**

```matlab
jsonOut = xASL_bids_BIDSifyASLNII(jsonIn, bidsPar, pathIn, pathOutPrefix)
```

**Description:**

It modifies the NIfTI file to take into account several BIDS
specifics. Specifically, it applies the previously calculated
scalings, and  it saves the ASLcontext.tsv file,


----
### xASL\_bids\_BIDSifyAnatJSON.m

**Format:**

```matlab
jsonOut = xASL_bids_BIDSifyAnatJSON(jsonIn)
```

**Description:**


It makes all the conversions to a proper BIDS structure, checks the existence of all BIDS fields, removes superfluous fields, checks all the conditions and orderes
the structure on the output. It works according to the normal BIDS, or ASL-BIDS definition


----
### xASL\_bids\_BIDSifyCheckTimeEncoded.m

**Format:**

```matlab
[jsonOut,bTimeEncoded, bTimeEncodedFME] = xASL_bids_BIDSifyCheckTimeEncoded(jsonIn, jsonOut, nVolumes)
```

**Description:**

Check for time encoded sequence.


----
### xASL\_bids\_BIDSifyM0.m

**Format:**

```matlab
[jsonOutM0, jsonOutASL] = xASL_bids_BIDSifyM0(jsonIn, jsonASL, studyPar, pathM0In, pathM0Out, headerASL)
```

**Description:**

It makes all the conversions to a proper BIDS structure, checks the existence of all BIDS
fields, removes superfluous fields, checks all the conditions and orders the structure on
the output. It works according to the normal BIDS, or ASL-BIDS definition. It modifies
the NIfTI file to take into account several BIDS specifics.
Specifically, it applies the previously calculated scalings.

1. Check the scaling in DICOMs
2. Check the JSON parameters
3. Save or move the NII to the correct location


----
### xASL\_bids\_CheckDatasetDescription.m

**Format:**

```matlab
[bImportedExploreASL, bImportedSameVersion, versionExploreASLBIDS, bImportedBETA] = xASL_bids_CheckDatasetDescription(datasetDescription, versionExploreASL)
```

**Description:**

Check the dataset\_description.json field contents with special regard to the import version.



----
### xASL\_bids\_CompareFieldLists.m

**Format:**

```matlab
strError = xASL_bids_CompareFieldLists(jsonStructA, jsonStructB, fieldList, ignoreFields)
```

**Description:**

This script compares the content of two JSON files for
the BIDS flavor testing.




----
### xASL\_bids\_CompareStoreDifference.m

**Format:**

```matlab
[identical,differences,dn] = xASL_bids_CompareStoreDifference(bPrintReport,differences,dn,allFiles,iFile)
```

**Description:**

Store the difference found in a TEXT file.




----
### xASL\_bids\_CompareStructures.m

**Format:**

```matlab
[identical,results,reportTable] = xASL_bids_CompareStructures(pathDatasetA,pathDatasetB,[bPrintReport,threshRmseNii,detailedOutput,printWarnings,ignoreLogs]);
```

**Description:**

Function that compares two BIDS folders with several subfolders and studies and prints the differences.
We recommend to set bPrintReport to true, because you otherwise can't see significant file content differences.




----
### xASL\_bids\_CompareStructuresCheckContent.m

**Format:**

```matlab
[identical,differences] = xASL_bids_CompareStructuresCheckContent(filesDatasetA,filesDatasetB,pathDatasetA,pathDatasetB,identical,bPrintReport,detailedOutput,threshRmseNii)
```

**Description:**

This script iterates over the provided files (filesDatasetA,filesDatasetB).
There are different comparisons implemented for JSON, TSV, TEXT, & NIFTI files.




----
### xASL\_bids\_CompareStructuresJSON.m

**Format:**

```matlab
[differences,identical,dn] = xASL_bids_CompareStructuresJSON(differences,identical,bPrintReport,allFiles,iFile,dn,currentFileA,currentFileB)
```

**Description:**

This script compares the content of two JSON files for
the BIDS flavor testing.




----
### xASL\_bids\_CompareStructuresNIFTI.m

**Format:**

```matlab
[differences,identical,dn] = xASL_bids_CompareStructuresNIFTI(differences,identical,bPrintReport,detailedOutput,allFiles,iFile,dn,currentFileA,currentFileB,threshRmseNii)
```

**Description:**

This script compares the content of two NIFTI files for
the BIDS flavor testing.




----
### xASL\_bids\_CompareStructuresTEXT.m

**Format:**

```matlab
[differences,identical,dn] = xASL_bids_CompareStructuresTEXT(differences,identical,bPrintReport,allFiles,iFile,dn,currentFileA,currentFileB)
```

**Description:**

This script compares the content of two TEXT files for
the BIDS flavor testing.




----
### xASL\_bids\_CompareStructuresTSV.m

**Format:**

```matlab
[differences,identical,dn] = xASL_bids_CompareStructuresTSV(differences,identical,bPrintReport,allFiles,iFile,dn,currentFileA,currentFileB)
```

**Description:**

This script compares the content of two TSV files for
the BIDS flavor testing.




----
### xASL\_bids\_Config.m

**Format:**

```matlab
bidsPar = xASL_bids_Config()
```

**Description:**

Creates several structures necessary for configuring the DICOM to BIDS conversion and saving of BIDS JSON files and NII structure.


----
### xASL\_bids\_CreateDatasetDescriptionTemplate.m

**Format:**

```matlab
[json] = xASL_bids_CreateDatasetDescriptionTemplate(draft, versionExploreASL)
```

**Description:**

This script creates a JSON structure which can be saved
using spm\_jsonwrite to get a dataset\_description.json template.
Missing fields that are required are added. BIDSVersion checked against the current configured version.
Remaining fields will be validated. Other fields not belonging to dataset\_description.json are ignored.



----
### xASL\_bids\_DRO2BIDS.m

**Format:**

```matlab
xASL_bids_DRO2BIDS(droTestPatient, [droSubject, deleteGroundTruth, exploreaslVersion])
```

**Description:**

Prepare DRO test patient for BIDS2RAW conversion.
This script uses the output of the asldro python script and
converts it into a bids structure that can be read by our
xASL\_wrp\_BIDS2Legacy script.
An exemplary usage is shown in the unit test called
xASL\_ut\_UnitTest\_function\_BIDS2Legacy.



----
### xASL\_bids\_Dicom2JSON.m

**Format:**

```matlab
[parms pathDcmDictOut] = xASL_bids_Dicom2Parms(imPar, pathIn[, pathJSON, dcmExtFilter, bUseDCMTK, pathDcmDictIn])
```

**Description:**

The function goes through the pathIn files, reads the DICOM or PAR/REC files
and parses their headers. It extracts the DICOM parameters important for ASL,
makes sure they are in the correct format, if missing then replaces with default
value, it also checks if the parameters are consistent across DICOM files for a
single sequence.

1. Admin
2. Set up the default values
3. Recreate the parameter file from raw data




----
### xASL\_bids\_GetPhoenixProtocol.m

**Format:**

```matlab
[xasl,parameters,parameterList,phoenixProtocol] = xASL_bids_GetPhoenixProtocol(pathData,bUseDCMTK)
```

**Description:**

Function that reads raw DICOM data (".dcm" or ".IMA") and extracts the phoenix protocol parameters.
Only works for Siemens DICOM data with phoenix protocol (tag = [0x29,0x1020]).




----
### xASL\_bids\_JsonCheck.m

**Format:**

```matlab
jsonOut = xASL_bids_JsonCheck(jsonIn,fileType)
```

**Description:**


It checks the existence of all BIDS fields, removes superfluous fields, checks all the conditions and orderes
the structure on the output. It works according to the normal BIDS, or ASL-BIDS definition


----
### xASL\_bids\_MergeNifti.m

**Format:**

```matlab
NiftiPaths = xASL_bids_MergeNifti(NiftiPaths, seqType[, niiTable])
```

**Description:**

This function takes a list of M0 or ASL4D files and concatenates them together in a longer 4D volume if possible
following certain patterns: works only with 3D and 4D files; all files in the list must have the same size of the
first three dimensions; files are generarily sorted according to the last number in the filename and outputted
to M0.nii or ASL4D.nii; first JSON is taken and renamed, all other JSONs and NIIs are deleted after merging;
M0\*\_parms.m or ASL\*\_parms.mat is renamed to M0\_parms.m or ASL4D\_parms.m; M0 files are checked if the field
PhaseEncodingAxis is consistent through all the volumes, if not the nothing is merged; this is applied to a generic case
and 3 other specific Siemens scenarios are implemented:

- i) All NII files have two volumes, then simply concatenate according to the last number.
- ii) Two files with a single volume each are merged according to the last number in the file name.
- iii) Multiple files with each containing a single volume are sorted to tags ASL4D\_x\_x\_Y and controls ASL4D\_Y and merged in the order
of the last number in the filename (Y) alternating the tags and controls

dcm2nii outputs GEs deltaM and M0scan separately, even though they are scanned in a single sequence and should belong together
Siemens sometimes stores all controls and labels separately, which need to be reordered according their acquisition order

This function performs the following steps in subfunctions:

1. xASL\_bids\_MergeNifti\_M0Files Generic merging of M0 files
2. xASL\_bids\_MergeNifti\_GEASLFiles Merge GE ASL files and extract scan order from DICOM tags
3. xASL\_bids\_MergeNifti\_SeriesNumber Merge ASL files by SeriesNumber if different
4. xASL\_bids\_MergeNifti\_SiemensASLFiles Merge Siemens ASL files with specific filename pattern
5. xASL\_bids\_MergeNifti\_AllASLFiles Merge any ASL files
6. xASL\_bids\_MergeNifti\_Merge Merge NiftiPaths & save to pathMerged
7. xASL\_bids\_MergeNifti\_Delete Delete NiftiPaths and associated JSONs
8. xASL\_bids\_MergeNifti\_RenameParms Find \*\_parms.m files in directory and shorten to provided name


----
### xASL\_bids\_MergeStudyPar.m

**Format:**

```matlab
jsonIn = xASL_bids_MergeStudyPar(jsonIn,studyPar,bidsModality);
```

**Description:**

Check if required fields exist in studyPar but not in jsonIn or if we can find them in other ways.

The BIDSification of JSON metadata requires at least some basic fields. If dcm2niix can't extract
fields like Manufacturer from the DICOM data (strict anonymization), we need to be able to read them
from the studyPar JSON (manually inserted). Alternatively we can check other DICOM tags for information.

This function can potentially be enhanced in future release to fix other fields besides the Manufacturer as well.
To enable this functionality for different modalities, we introduced the bidsModality parameter.

This function is called by:

- `xASL\_bids\_BIDSifyM0`
- `xASL\_bids\_BIDSifyASLJSON`
- `xASL\_bids\_BIDSifyAnatJSON`


----
### xASL\_bids\_Par2JSON.m

**Format:**

```matlab
parms = xASL_bids_Par2JSON(pathPar, pathJSON)
```

**Description:**

Opens the Philips PAR file. Reads the relevant DICOM headers and saves them to JSON sidecar in a BIDS format.
The JSON file is created automatically by the dcm2nii readout, so it always looks for this JSON file and
add the same time reads the PAR file and adds further parameters to the JSON that were not identified by
the dcm2nii tool.


----
### xASL\_bids\_PhoenixProtocolAnalyzer.m

**Format:**

```matlab
[bidsPar,sourcePar] = xASL_bids_PhoenixProtocolAnalyzer(parameterList);
```

**Description:**

This function analyzes the parameter list of the phoenix protocol (tag = [0x29,0x1020]).
This function is usually called from xASL\_bids\_GetPhoenixProtocol.




----
### xASL\_bids\_PhoenixProtocolReader.m

**Format:**

```matlab
[parameterList,phoenixProtocol] = xASL_bids_PhoenixProtocolReader(rawPhoenixProtocol)
```

**Description:**

Function to parse the raw phoenix protocol. This function is usually called from xASL\_bids\_GetPhoenixProtocol.




----
### xASL\_bids\_ValidateNiftiName.m

**Format:**

```matlab
xASL_bids_ValidateNiftiName(fileName,perfType)
```

**Description:**

Validate the NIFTI filename based on the regular expressions from bids-matlab.


----
### xASL\_bids\_VendorFieldCheck.m

**Format:**

```matlab
jsonOut = xASL_bids_VendorFieldCheck(jsonIn,bIsASL)
```

**Description:**


It checks all the JSON fields, make sure that they are renamed from vendor specific names to common BIDS names


----
### xASL\_bids\_determineImageTypeGE.m

**Format:**

```matlab
imageType = xASL_bids_determineImageTypeGE(jsonPar)
```

**Description:**

Determine the image type of a GE DICOM.


----
### xASL\_bids\_parms2BIDS.m

**Format:**

```matlab
outBids = xASL_bids_parms2BIDS(inXasl[, inBids, bOutBids, priorityBids])
```

**Description:**

This functions takes two parameter structures and merges them. At the same time, renames all fields
according to the output type (note that only some fields have two standardised names different between the two formats.
In case of duplicities, takes the field value from the preferred format.
Also takes into account that the units in BIDS are s, but in xASL ms.
This function performs the following steps:

1. Define field names that need to be convert/renamed/merged
2. Convert XASL fields to the output format (BIDS or XASL)
3. Convert BIDS fields to the output format (BIDS or XASL)
4. Merge the BIDS and XASL fields, convert field values


----
### xASL\_bids\_parseM0.m

**Format:**

```matlab
xASL_bids_parseM0(pathASLNifti)
```

**Description:**

Check the .JSON and aslContext.tsv sidecards of an ASL file in BIDS format and find the
specified M0 possibilities. Then it converts the ASL file to ExploreASL legacy format including
splitting of ASL and M0 NIFTIes if needed. Note that the sidecars are in BIDS, but the file-structure
is already expected to be in Legacy format

The following options are processed:
1. Included - The M0 file is included in the ASL timeseries as specified in the aslContext
2. Separate - M0 image is already provided as a separate image - checks for its existence
3. Estimate - If a single M0 values is provided then keep it, just rename the field
4. Absent   - In this case we use the control image as pseudo-M0, but we have to verify if there is no background suppression
or if yes, then the background suppression timings also need to be specified


## Development

----
### xASL\_dev\_DocCrawler.m

**Format:**

```matlab
xASL_dev_DocCrawler(inputPath)
```

**Description:**

This function checks each individual file header from the ExploreASL source coude and
extracts the information. The results is saved as a markdown file to be used for
Deploying the documentation web.

If you want to use star symbols (\*testFile.m e.g.) we recommend not to use them in the same
line with bold text (which is written like this: **bold text**).

1. Define defaults and admin
2. Iterate over files
3. Extract header information from each file
4. Final formatting


----
### xASL\_dev\_DocInitialize.m

**Format:**

```matlab
xASL_dev_DocInitialize
```

**Description:**

This function generates all markdown files, which are necessary for the mkdocs documentation - i.e. it initializes the MD files needed
for deploying the documentation. It copies the manually edited MD files and crawls through function headers to ready everything for the
deployment

1. Administration
2. Copy MD-files from ExploreASL source-code
3. Copy the manually edited MD-files
4. Crawl through function headers to generate reference manual


----
### xASL\_dev\_MakeStandalone.m

**Format:**

```matlab
xASL_adm_MakeStandalone(outputPath, bCompileSPM, markAsLatest);
```

**Description:**

This function creates an output folder including a standalone version of ExploreASL,
which can be used with the Matlab Runtime outside of Matlab itself.

A quick fix to solve path dependencies etc. is to first compile SPM (but this can be turned off for speed).

This function performs the following steps:

1. Manage ExploreASL and compiler code folders
2. Capture version/date/time
3. File management output folder & starting diary
4. Handle SPM Specific Options
5. Manage compilation paths
6. Run SPM compilation
7. Run ExploreASL compilation
8. Print done


## FSL

----
### xASL\_fsl\_RunFSL.m

**Format:**

```matlab
[x] = xASL_adm_RunFSL(FSLCommand, x[, OutputZipping, NicenessValue, bVerbose])
```

**Description:**

This function runs an FSL command from ExploreASL:

1. Checking the FSL dir
2. Manage CUDA/CPU parallelization (currently disabled, WIP)
3. Setting up FSL environment
4. Running the command

Supports .nii & .nii.gz, Linux, MacOS & Windows (WSL)


----
### xASL\_fsl\_SetFSLdir.m

**Format:**

```matlab
[FSLdir[, x, RootWSLdir]] = xASL_adm_SetFSLdir(x, bUseLatestVersion)
```

**Description:**

This function finds the FSLdir & puts it out, also in
x.FSLdir to allow repeating this function without having to repeat
searching.
If the FSLdir & RootFSLdir are already defined in x.FSLdir & x.RootFSLdir, this function
is skipped.
Supports Linux, MacOS & Windows (WSL), & several different
default installation folders for different Linux
distributions


----
### xASL\_fsl\_TopUp.m

**Format:**

```matlab
xASL_fsl_TopUp(InDir[, ScanType], x)
```

**Description:**

This function runs FSL TopUp. It assumes that there are 2
TopUp images, i.e. 1 blip up & 1 blip down.

0. Admin: manage ScanType, NIfTI paths, create TopUp
parameter file for image to apply TopUp to & for the TopUp NIfTIs,
delete files from previous run, define the image with the
same acquisition parameters as TopUp (does the image
we apply TopUp to, have the Blip up or down?)
1. Register images to image that we apply TopUp to
(registration between blip up/down images is performed by
TopUp)
2. Run TopUp estimate (i.e. estimate the geometric distortion field from B0 NIfTI &
parameters file), this takes quite long. Also has a x.settings.Quality=0 option that is very fast
but inaccurate, to try out this pipeline part. Before
TopUp, NaNs (e.g. from resampling) are removed from the images
TopUp is run with default settings
3. Apply TopUp


## Image Processing

----
### xASL\_im\_BilateralFilter.m

**Format:**

```matlab
[ovol] = xASL_im_BilateralFilter(volIM, mask, VoxelSize, x)
```

**Description:**

This function runs a spatial lowpass temporally
highpass filter, and removes outliers within this signal, and adapts the
time-series accordingly.



----
### xASL\_im\_CenterOfMass.m

**Format:**

```matlab
xASL_im_CenterOfMass(PathNIfTI, OtherList, AllowedDistance)
```

**Description:**

This function estimates the center of mass of the image
matrix, and if this is too far off the current orientation
matrix center, the center will be reset.
This fixes any incorrect orientation outputted by the
scanner.
The realignment is only applied when any of the X/Y/Z
dimensions have a higher offset than AllowedDistance.




----
### xASL\_im\_CleanupWMHnoise.m

**Format:**

```matlab
xASL_im_CleanupWMHnoise(InputPath, OutputPath, MinLesionVolume, pThresh)
```

**Description:**

Threshold white matter lesions,
acknowledging the fact that they may be confluent with subresolution connection
through a dilation. This part is executed conservatively, as FLAIR hyperintensities
inside the GM can be erroneously segmented as WMH, and should not be lesion-filled
(otherwise these cannot be fixed later in the Structural module).

Note that LST lesion filling expects a probability map, doesnt work nicely with binary mask



----
### xASL\_im\_ClipExtremes.m

**Format:**

```matlab
[NewIM] = xASL_im_ClipExtremes(InputIm[, ThreshHigh, ThreshLow, bVerbose, bNormalize])
```

**Description:**

This function clips an image to a given percentile. The percentile is found
using non-zeros sorted intensities, so both isfinite & non-zeros.
This function performs the following steps:

1. Constrain clippable intensities
2. Clip high intensities
3. Clip low intensities
4. Normalize to 4096 (12 bit, 12^2)
5. Save as NIfTI if the input was a NIfTI



----
### xASL\_im\_Column2IM.m

**Format:**

```matlab
[ImageOut] = xASL_im_Column2IM(ColumnIn, BrainMask)
```

**Description:**

This function "decompresses" an image matrix (or multiple matrices)
from a single-dimensional column, by reconstructing the image matrix
from the voxel positions within the BrainMask.
NB: Important to use the same BrainMask as used for converting the
image matrix to the column!
See also: xASL\_im\_IM2Column.m

The mask mostly used for xASL\_im\_IM2Column is x.S.masks.WBmask, which completely
engulfes pGM, pWM & pCSF.


----
### xASL\_im\_CompareNIfTIResolutionXYZ.m

**Format:**

```matlab
[IsEqualResolution] = xASL_im_CompareNIfTIResolutionXYZ(PathNIfTI1, PathNIfTI2)
```

**Description:**

This function checks whether the X, Y and Z resolution of a
NIfTI with any number of dimensions is equal. It rounds for 2 floating
points, for both NIfTIs, to ensure that the same precision is compared.


----
### xASL\_im\_CompareNiftis.m

**Format:**

```matlab
[identical,RMSE] = xASL_im_CompareNiftis(pathA,pathB)
```

**Description:**

Compare two niftis. Untouched comparison based on copies.




----
### xASL\_im\_ComputeDice.m

**Format:**

```matlab
[DiceCoeff] = xASL_im_ComputeDice(imA, imB)
```

**Description:**

This function calculates the Dice coefficient of image overlap.



----
### xASL\_im\_CreateASLDeformationField.m

**Format:**

```matlab
xASL_im_CreateASLDeformationField(x, bOverwrite, EstimatedResolution)
```

**Description:**

This function smooths a transformation flow field to a lower
resolution. Usually, we use a high resolution anatomical
image (e.g. **3D T1w**) to obtain the flowfields from native
space to standard space, and apply these to the lower
resolution ASL images. Because of the resolution
differences, the flowfields need to be downsampled/smoothed,
to avoid deformation effects that are crispier than the
functional image that is investigated. This function
performs the following steps:

1. Obtain resolutions
2. Fill NaNs at edges y\_T1.nii flowfield to prevent interpolation artifact
3. Smooth flowfield
4. Fill NaNs at edges y\_ASL.nii

Note that if the resolution of ASL is not significantly (i.e. >0.5 mm in
any dimension) lower than T1w, the y\_T1.nii is copied to y\_ASL.nii
--------------------------------------------------------------------------------------------------------------------

----
### xASL\_im\_CreatePseudoCBF.m

**Format:**

```matlab
xASL_im_CreatePseudoCBF(x, spatialCoV[, bPVC])
```

**Description:**

This function creates a pseudo-CBF image from mean CBF template,
arterial transit time (ATT) bias field & vascular artifacts, weighted through spatial CoV
The first part of this code puts templates in the native space and
creates a pseudoCBF image from a downsampled pGM & pWM tissue (PseudoTissue). The latter
is used for registration but also as reference for the template
registration, to speed this up.
The second part of this code computes a pseudoCBF image based on the
pseudoTissue & the CBF templates of CBF, ATT biasfield and vascular peaks, based on spatial CoV.

This submodule performs the following steps:

1. Create the pseudoTissue CBF reference image, if it doesnt exist already
2. Create the native space copies of ASL templates, if they dont exist already
3. Spatial CoV input argument check
4. Load native space copies of templates
5. Create pseudoTissue from segmentations, mix this with the mean CBF template depending on spatial CoV
6. Create pseudoCBF reference image used for CBF-based registration
7. Scale mean\_PWI\_Clipped source image to the same range as PseudoCBF


----
### xASL\_im\_CreateSliceGradient.m

**Format:**

```matlab
xASL_im_CreateSliceGradient(x)
```

**Description:**

1. Create slice gradient in same space as input file
2. Reslice slice gradient to MNI (using existing ASL matrix changes from e.g. registration to MNI, motion correction, registration to GM)
3. Creating average slice gradient



----
### xASL\_im\_DecomposeAffineTransformation.m

**Format:**

```matlab
[M, P] = xASL_im_DecomposeAffineTransformation(Mtransformation)
```

**Description:**

This function splits a transformation matrix into individual
components, which can be useful to guide the SPM reslicing.
The components are the same as in spm\_(i)matrix.m, except for
the shearing: these are included in the rotations, and
the 90 degree rotations, these are separated.

Reason for the separation of the 90 degree rotations, is
that these indicate if orientations (transversal, coronal &
sagittal) have been switched in the NIfTI.

This can be useful to correct for any erroneous 90degree rotations from registration,
or to put two images in the same orientation order or voxelsize without applying
their subtle realignment (e.g. for manipulating registration references)

THEORY 90 degree rotations:
Any rotation will always swap the other dims (X rotation swaps Y/Z, Y
rotation swaps X/Z etc.) because they are perpendicular (haaks)

Dims X Y Z care for LR, AP and IS translation.
- X-rotation will rotate the transverse slice (LR <-> AP)
swapping Y (coronal) & Z (saggital)
- Y-rotation will rotate the coronal slice (IS <-> LR) slice,
swapping X (transversal) and Z (sagittal)
- Z-rotation will rotate the sagittal slice (AP <-> IS)
swapping X (transversal) and Y (sagittal)

E.g., MPRAGE is acquired in sagittal slices, and ASL/fMRI/BOLD in
transversal slices. This is an Y rotation (you look into the coronal
plane, rotate this, which will swap the sagittal slices into transversal)

This function performs the following steps:

0. Start with an output P and M struct
1. Obtain translations
2. Obtain zoom
3. Obtain 90degree rotations
4. Obtain subtle rotations & shearing
5. Check the rounding errors of the decomposition


----
### xASL\_im\_DetermineFlip.m

**Format:**

```matlab
[LR_flip_YesNo] = xASL_im_DetermineFlip(PathOrientationResults)
```

**Description:**

This functions check determinants before and after image processing
(nii.mat0 vs nii.mat, respectively) to find any potential
left-right processing. This function performs the following
steps:
1. Determine correct row, differs between Matlab versions
2. If units are printed as second row, the data starts on the third row
3. Determine column indices
4. Find left-right flips



----
### xASL\_im\_DilateErodeFull.m

**Format:**

```matlab
imOut = xASL_im_DilateErodeFull(imIn, type, kernel)
```

**Description:**

Runs dilation or erosion on a binary imIn in full three dimensions.
It uses its own dilate\_erode function and crops the image so that it
contains only the mask. The size of all three dimensions of the kernel needs to be an odd number.



----
### xASL\_im\_DilateErodeSeparable.m

**Format:**

```matlab
imOut = xASL_im_DilateErodeSeparable(imIn, type, kernel_x, kernel_y, kernel_z)
```

**Description:**

Runs dilation or erosion on a binary imIn separably in three dimensions. Dilation/erosion
in each dimension is done by using the specified kernels. It uses its own dilate\_erode function
and crops the image so that it contains only the mask.
Works only with odd sized kernels



----
### xASL\_im\_DilateErodeSphere.m

**Format:**

```matlab
el = xASL_im_DilateErodeSphere(R)
```

**Description:**

Creates a 3D structuring element (binary) sphere with the given diameter (R) and size 2\*R+1



----
### xASL\_im\_DummyOrientationNIfTI.m

**Format:**

```matlab
xASL_im_DummyOrientationNIfTI(PathSrc, PathRef, PathDummyOut[, bApplyRotationMinor, bApplyRotation90, bApplyZoom, bApplyTranslation])
```

**Description:**

This function creates a dummy image as reference for xASL\_spm\_reslice,
allowing to only apply specific parts of the transformation between the
two images. E.g. only the rotation, or only the zooming.
This can be useful to correct for any erroneous rotations from registration,
or to put two images in the same space without applying their
realignment. This function performs the following steps:

1. Load orientations & calculate transformation
2. Calculate the desired transformation
3. Calculate new orientation matrix
4. Calculate the new image size
5. Save the dummy NIfTI



----
### xASL\_im\_EstimateResolution.m

**Format:**

```matlab
[resFWHM, resSigma, resErr, imSmo, imMask] = xASL_im_EstimateResolution(imCBF, imGM, imWM[, imMaskOrig, PSFtype, maxIter])
```

**Description:**

Creates a high-resolution pseudo-CBF image based on segmented GM and WM maps and iteratively adjusts its resolution
by smoothing until reaching a perfect fit with the CBF image thus obtaining the resolution difference between the GM and CBF image and
uses this to calculate the estimated effective resolution of hte CBF. Note that all the calculations are done using voxels as measures
and not mm, so the output resolution is also in voxels and has to be transfered to mm by using the knowledge about the voxel size.
It is assumed that for imGM and imWM, the voxel size equals the resolution, and the imCBF is upsampled to the smaller voxels of imGM.



----
### xASL\_im\_Flip.m

**Format:**

```matlab
[MatrixOut] = xASL_im_Flip(MatrixIn, varargin)
```

**Description:**

Backwards compatibility for flipping left-right in standard
space (NB: this can be different than in native space!).



----
### xASL\_im\_FlipNifti.m

**Format:**

```matlab
xASL_im_FlipNifti(pathInput[, flipAxis, bOverwrite])
```

**Description:**

This function allows correcting an inappropriate flip in the image matrix. It
will not change the orientation matrix in the header but the image
itself. So any NifTI program will not be aware of this flip!

This function runs the following steps:
1. Manage if we overwrite the new NIfTI
2. Manage if we zip the new NIfTI
3. Load image from NIfTI
4. Flip image
5. Save image to NIfTI


----
### xASL\_im\_HausdorffDist.m

**Format:**

```matlab
xASL_im_HausdorffDist(imIn1,imIn2)
```

**Description:**

Calculate Hausdorff and modified Hausdorff distance between two ROIs in volumes imIn1, imIn2.
Input images are binarized as 0 and non-0



----
### xASL\_im\_IM2Column.m

**Format:**

```matlab
[ColumnOut] = xASL_im_IM2Column(ImageIn, BrainMask[, ApplyShiftDim])
```

**Description:**

This function "compresses" an image matrix (or multiple matrices)
for optimization of memory and CPU resources. The output column only includes
voxels that lie within the BrainMask. This excludes extracranial
zero-information voxels from computations and memory use.
NB: Important to use the same BrainMask for converting the
column back to an image matrix!
See also: `xASL\_im\_Column2IM.m`

The mask mostly used for xASL\_im\_IM2Column is `x.S.masks.WBmask`, which completely
engulfes pGM, pWM & pCSF


----
### xASL\_im\_JointHist.m

**Format:**

```matlab
imHist = xASL_im_JointHist(imA,imB[,imMask,minA,maxA,minB,maxB,nBins])
```

**Description:**

It calculates a joint histogram of two images of any dimensions over a mask of the same size.
The boundaries and number of bins can either be given or min and max values are used. Values
outside of the bins are counted to the first/last bin.

----
### xASL\_im\_Lesion2CAT.m

**Format:**

```matlab
LesionPathOut = xASL_im_Lesion2CAT (PathIn)
```

**Description:**

For all lesion masks in the anatomical directory, load
them, merge them and save them for the CAT segmentation.
If there are no lesions found, the images are untouched.



----
### xASL\_im\_Lesion2Mask.m

**Format:**

```matlab
LesionIM = xASL_im_Lesion2Mask(LesionPath, x)
```

**Description:**

This function takes a mask and adds several ROIs, to be used as custom "atlas", e.g. when computing region-average CBF values.
The mask % can be an ROI or lesion, if we assume it is a lesion, the following masks are created:

1. Intralesional
2. Perilesional (15 mm rim around the lesion)
3. Hemisphere (ipsilateral to lesion)
4. Contralateral version of 1
4a First create separate masks
4b Check if they are mutually exclusive
4c Save NIfTI file
5. Contralateral version of 2
6. Contralateral version of 3

All these masks are masked by a brainmask `(pGM+pWM)>0.5`

This function performs the following steps:

1. If lesion is empty, skip this & delete the file
2. BrainMasking
3. Create hemispheres
4. Save mutually exclusive masks
5. Create tsv-sidecar containing the names of the ROIs
6. Visual QC



----
### xASL\_im\_M0ErodeSmoothExtrapolate.m

**Format:**

```matlab
[ImOut, VisualQC] = xASL_im_M0ErodeSmoothExtrapolate(ImIn, DirOutput, NameOutput, pvGM, pvWM, brainCentralityMap[, LowThreshold])
```

**Description:**

This function erodes, smooths & extrapolates M0 in MNI standard space (tested at 1.5x1.5x1.5mm as used in ExploreASL).
It assumes that the M0 image is in standard space & that the GM & WM probability maps
are aligned. Here, we mask the M0, to remove high CSF signal and low extracranial signal,
enabling us to smooth the image without inserting wrong signal. See also
the ExploreASL manuscript for a more extensive explanation.

A visual QC figure is created, showing the M0 image processing steps for a single transversal slice (slice 53 in `1.5 mm` MNI standard space)
`OutputFile = fullfile(DirOutput,['M0\_im\_proc\_' NameOutput '.jpg']);`
The original M0 image (a) is masked with a (pGM+pWM)>50% mask (b)
eroded with a two-voxel sphere to limit the influence of the ventricular and extracranial signal (c)
and thresholded to exclude significantly high (i.e. median + 3\*mean absolute deviation (MAD)) border region values (d)
This masked M0 image is smoothed with a 16x16x16 mm full- width-half-maximum Gaussian filter (Mutsaerts et al., 2018) (e)
after which the signal at the border is smoothly extrapolated until the full image is filled (f).
Whereas the masking avoids mixing with cerebrospinal fluid or extracranial signal, the extrapolation avoids M0 division artifacts

This function performs the following steps:

1.  Mask 1: structural mask
2.  Mask 2: intensity-based mask to remove extracranial signal
2.a Multiplication with brain mask centrality map
2.b Mask the "dummy image" from 2.a based on intensities
2.c Enforce to include the eroded brain in the mask
3.  Mask 3: Erode the combined masks
4.  Mask 4: Determine any odd borders
5.  Smoothing
6.  Extrapolating only
7.  Scale back to the GM M0
8.  Print visual QC figure


----
### xASL\_im\_MaskNegativeVascularSignal.m

**Format:**

```matlab
[NegativeMask, TreatedPWI] = xASL_quant_DetectNegativeVascularSignal(x)
```

**Description:**

This function segments clusters with significant negative
ASL signal. This can be tricky as there is also the negative tail of Gaussian noise
from the ASL subtraction. The image feature we use here, is that negative
vascular signal will be a relatively large region with
significant median negative value, whereas noise will be
regions with relatively small negative signal.
Negative signal from wrong background suppression timing
(e.g. in the first slice with \*\*2D EPI\*\*) can be masked out with
this as well.
The procedure works as follows:

1. Obtain mask of negative voxels within `pGM>0.5` mask
2. Obtain distribution of subzero clusters
3. Define the negative threshold
4. Create mask by thresholding whole image

Note that the definition of the threshold is obtained within
the GM only, but that this threshold is applied to the full image.

Note that instead of the PWI path input, a CBF image should
work equally well, as we don't expect a smooth M0 biasfield
to change the distribution of negative clusters



----
### xASL\_im\_MaskPeakVascularSignal.m

**Format:**

```matlab
[MaskIM, CBF] = xASL_quant_VascularContrast(PathPWI, Path_M0, CompressionRate, ClipThresholdValue, bClip)
```

**Description:**

This function searches for an acceptable high
threshold as definition of high intra-vascular ASL signal.
It also allows to compress the values here (when
bClip==true). Compression retains some variability, but limits their outlying influence
on statistics.
The procedure works as follows:

1. Segment **GM** on **ASL** image at 80th percentile of **CBF** image distribution
2. For PWI & CBF images, select voxels higher than median + ClipThresholdValue **MAD**
Vascular artifacts will have a high intensity in both images, whereas errors by division by M0 will only have a high
intensity on the M0 image, and high values due to a biasfield will only be noticeable on the PWI image
3. Combine the two created masks
4. Obtain average clipping value from selected voxels from the combined masks
5. Apply compression if requested. If not, output image will
have NaNs for intra-vascular voxels.

Note that the definition of the threshold is obtained within
the GM only, but that this threshold is applied to the full
image to also remove extracranial extreme values.

Note that the performance may change when using this script
with or without M0, as this will change the distribution
that determines where the threshold for extremes lies.


----
### xASL\_im\_Modulation.m

**Format:**

```matlab
xASL_im_Modulation(x)
```

**Description:**

Combines the transformations to create Jacobians, &
multiplies the standard space segmentations with these to create volumetric
images for volumetric analyses.



----
### xASL\_im\_NormalizeLabelingTerritories.m

**Format:**

```matlab
image_out = xASL_im_NormalizeLabelingTerritories( imageIN, GMmask, x)
```

**Description:**

Normalizes per perfusion territory mask should be GM mask.



----
### xASL\_im\_PCA.m

**Format:**

```matlab
[pc, score, eigenvalues, tsquare, loadings, Xmean] = xASL_im_PCA(dataIn)
```

**Description:**

Perform a Principal Component Analysis.



----
### xASL\_im\_PVCbspline.m

**Format:**

```matlab
[imPVEC,imCBFrec,imResidual,FWHM] = xASL_im_PVCbspline(imCBF,imPV[,bsplineNum])
```

**Description:**

PVC of ASL data using prior GM-,WM-partial volume maps.
Follows the principles of the PVEc algorithm by I. Asllani (MRM, 2008).
The PV-corrected CBF\_GM and CBF\_WM maps are approximated using an
uniformly sampled cubic B-splines.
----------------------------------------------------------------------------------------------

REFERENCES:
-Asllani I, Borogovac A, Brown TR. Regression algorithm correcting for
partial volume effects in arterial spin labeling MRI. Magnetic Resonance
in Medicine. 2008 Dec 1;60(6):1362-71.
-Petr J, Mutsaerts HJ, De Vita E, Steketee RM, Smits M, Nederveen AJ,
Hofheinz F, van den Hoff J, Asllani I. Effects of systematic partial
volume errors on the estimation of gray matter cerebral blood flow with
arterial spin labeling MRI. MAGMA 2018. DOI:10.1007/s10334-018-0691-y



----
### xASL\_im\_PVCkernel.m

**Format:**

```matlab
[imPVC,imCBFrec,imResidual] = xASL_im_PVCkernel(imCBF, imPV [,kernel,mode])
```

**Description:**

Partial volume correction (PVC) of ASL data using prior GM-,WM-partial volume maps.
Follows the principles of the PVC algorithm by I. Asllani (MRM, 2008).



----
### xASL\_im\_PreSmooth.m

**Format:**

```matlab
pathOut = xASL_im_PreSmooth(pathRef,pathSrc[,pathSmo,resRef,resSrc,srcAffinePath, bInvAffine])
```

**Description:**

It assumes that the FWHM is equal to voxel size, unless the real resolution is given.
Then takes into account the voxel sizes and orientation difference between the volumes, but
performs the smoothing according to the given real resolution (it is possible to supply the
resolution for just one image) - this should be helpful primarily when the
It creates a Gaussian covariance matrix for the reference, transforms it according to
the difference between the two images a produces the Gaussian covariance matrix
of the pre-smoothing in the source space. Then it performs the smoothing.

The following steps are performed:

1. Obtain the voxel size
2. Skip this function if reference resolution is equal to, or lower than source resolution
3. Deal with affine transformation
4. Obtain the transformation matrix from the Reference to the Source space
5. Apply the smoothing filter on the source image(s)
6. Save the smoothed image


----
### xASL\_im\_ProcessM0Conventional.m

**Format:**

```matlab
[Corr_M0] = xASL_im_ProcessM0Conventional(ImIn, x)
```

**Description:**

This function uses the conventional M0 masking,
and only a little smoothing, following what Philips uses for its 3D
{{GRASE}}. Advantages of the newer M0 processing in ExploreASL are the lack
of use of **M0** threshold-based masking, the removal of high CSF values and
higher **SNR** for **ASL** division.



----
### xASL\_im\_ProjectLabelsOverData.m

**Format:**

```matlab
OutputIM = xASL_im_ProjectLabelsOverData(DataIM,LabelIM,x,ScaleFactorData,ScaleFactorLabel)
```

**Description:**

This script projects labels over an image,
but works only in 2D. Make sure to make a 2D image from a 3D or 4D image
using xASL\_vis\_TransformData2View.m
can be used in combination with xASL\_vis\_Imwrite.m



----
### xASL\_im\_ReplaceLabel.m

**Format:**

```matlab
xASL_im_ReplaceLabel(pathNifti, LabelNumbersOld, LabelNumbersNew, pathNewNifti)
```

**Description:**

This function replaces label values/numbers inside a NIfTI
image, by the following steps:

1. Load NIfTI
2. Replace numbers
3. Save NIfTI



----
### xASL\_im\_ResampleLinear.m

**Format:**

```matlab
imOutput = xASL_im_ResampleLinear(imInput, newSize)
```

**Description:**

Downsample or upsample an image from its old to a new resolution.



----
### xASL\_im\_RestoreOrientation.m

**Format:**

```matlab
xASL_im_RestoreOrientation(PathNIfTI)
```

**Description:**

This function reverts the NIfTI header orientation matrix
to the original orientation from the scanner/dcm2nii conversion.



----
### xASL\_im\_SkullStrip.m

**Format:**

```matlab
xASL_im_SkullStrip(InPath, PathMNIMask, x, OutPath)
```

**Description:**

Creates skull-stripped **T1w** image based on **MNI** -> native
space registration from segmentation.



----
### xASL\_im\_Smooth3D.m

**Format:**

```matlab
[imSmo, imGaussX, imGaussY, imGaussZ] = xASL_im_Smooth3D(imIn, sigma[, PSFtype])
```

**Description:**

It smooths the 3D image with a 3D kernels that has defined the shape and SD of the smoothing separably in three dimension.



----
### xASL\_im\_SplitImageLabels.m

**Format:**

```matlab
xASL_im_SplitImageLabels(ImagePaths, LabelTable[, OutputFolder, bOverwrite, ResampleDir, SubRegExp])
```

**Description:**

This function allows extracting of labels from a NIfTI file
containing multiple labels, into single NIfTI files each
containing a single label.
Not all existing labels need to be extracted.

The following steps are performed:

1. Load TSV file
2. Process images


----
### xASL\_im\_Upsample.m

**Format:**

```matlab
xASL_im_Upsample(PathOrig, PathDest, NewVoxelSize, LeaveEmpty, PaddingDim, Kernel)
```

**Description:**

Upsamples an ASL image, without changing the orientation
matrix, which can be used e.g. for PVEc in higher
resolution but same space.



----
### xASL\_im\_ZeroEdges.m

**Format:**

```matlab
[IM] = xASL_im_ZeroEdges(IM[, EdgeThicknessPerc])
```

**Description:**

Resampling can sometimes give some strange errors near image edges. These should be NaNs,
but sometimes can be zeros or ones, or even weird numbers. For resampling, NaNs should be set to 0 (this is done
in another function) as they can influence the resampling (depending on the transformation matrix). To be sure
that the edges are nicely fixed, this function sets a border at the image matrix edges to zero.


----
### xASL\_im\_dilateROI.m

**Format:**

```matlab
xASL_im_dilateROI(PathIn, [PathOut, minVolume])
```

**Description:**

The function loads a binary image from PathIn and if smaller than the defined volume (40 mL by default) it
dilates it with a 3x3 sphere element until a minimal volume is reached. When it is small enough, it is saved to PathOut.
40 mm^3 is equal to 3 voxels in all directions in DARTEL space, or around the highest obtainable ASL effective resolution (3x3x4 mm).



----
### xASL\_im\_rotate.m

**Format:**

```matlab
rotated = xASL_im_rotate(im, angle)
```

**Description:**

Simple rotation of the first two dimension of a ND image by
0, 90, 180, 270 degrees.



## Import

----
### xASL\_imp\_AppendNiftiParameters.m

**Format:**

```matlab
s = xASL_imp_AppendNiftiParameters(nii_files)
```

**Description:**

Append Nifti Parameters.


----
### xASL\_imp\_AppendParmsParameters.m

**Format:**

```matlab
[s, FieldNames] = xASL_imp_AppendParmsParameters(parms)
```

**Description:**

Append Parms Parameters.


----
### xASL\_imp\_BasicParameterChecks.m

**Format:**

```matlab
x = xASL_imp_BasicParameterChecks(x)
```

**Description:**

Basic parameter checks for the import pipeline.

- check that x.dir.DatasetRoot is correct
- determine x.dir.sourceStructure
- determine x.dir.studyPar
- check x.opts.bImport
- set the defaults for ...
- x.modules.import.settings.bCopySingleDicoms
- x.modules.import.settings.bUseDCMTK
- x.modules.import.settings.bCheckPermissions



----
### xASL\_imp\_CatchErrors.m

**Format:**

```matlab
[dcm2niiCatchedErrors] = xASL_imp_CatchErrors(WarningID, WarningMessage, WarningLine, WarningFileName, WarningPath, scan_name, scanpath, destdir, dcm2niiCatchedErrors, imPar, StackIn)
```

**Description:**

Catch reported warnings/errors, print them if verbose, & add them to a structure of warnings/errors to be stored for later QC.



----
### xASL\_imp\_CheckDirectoriesAndPermissions.m

**Format:**

```matlab
[x] = xASL_imp_CheckDirectoriesAndPermissions(x)
```

**Description:**

Check directories and permissions.

- Check if the RawRoot exists
- Search for temp, derivatives, source and sourcedata
- Raise error if not a single directory exists
- Check the access rights for temp and rawdata
- DCMTK & DicomInfo realted settings



----
### xASL\_imp\_CheckIfFME.m

**Format:**

```matlab
bTimeEncodedFME = xASL_imp_CheckIfFME(jsonIn[, jsonOut, bTimeEncoded])
```

**Description:**

Check if the sequence, based on its JSON structure, is a FME (Fraunhofer Mevis) time encoded sequence.



----
### xASL\_imp\_CheckImportSettings.m

**Format:**

```matlab
[x] = xASL_imp_CheckImportSettings(x)
```

**Description:**

Basic import checks before execution.

- Check permissions for DCM2NII
- Get correct DCMNII version
- Define DCM Extension Filter
- Set default for skip subjects option



----
### xASL\_imp\_CompleteBIDS2Legacy.m

**Format:**

```matlab
x = xASL_imp_CompleteBIDS2Legacy(x)
```

**Description:**

Finish the remaining import steps, which are not done for each subject individually.

1. Create dataPar.json
2. Copy participants.tsv
3. Copy dataset\_description JSON and add 'GeneratedBy' fields
4. Add missing fields



----
### xASL\_imp\_CreateSummaryFile.m

**Format:**

```matlab
xASL_imp_CreateSummaryFile(thisSubject, PrintDICOMFields, x)
```

**Description:**

Create summary file.

1. Create summary file
2. Report totals

For the detailed description of the overview sub-structure (thisSubject & thisVisit)
please check out the description within xASL\_imp\_DetermineSubjectStructure.



----
### xASL\_imp\_DCM2NII\_CheckIfFME.m

**Format:**

```matlab
[resultJSON, bTimeEncoded, bTimeEncodedFME] = xASL_imp_DCM2NII_CheckIfFME(nii_files, bTimeEncoded, bTimeEncodedFME)
```

**Description:**

Check if the current sequence is a FME (Fraunhofer Mevis) time encoded sequence.



----
### xASL\_imp\_DCM2NII\_ConvertScan.m

**Format:**

```matlab
[x, thisSubject, dcm2niiCatchedErrors, PrintDICOMFields] = xASL_imp_DCM2NII_ConvertScan(x, matches, thisSubject, dcm2niiCatchedErrors, thisVisit, thisRun, scanFields)
```

**Description:**

Run DCM2NII for one individual scan.

4. Iterate over scans
-  1. Initialize variables (scanID, summary\_line, first\_match)
-  2. Convert scan ID to a suitable name and set scan-specific parameters
-  3. Minimalistic feedback of where we are
-  4. Now pick the matching one from the folder list
-  5. Determine input and output paths
-  6. Start the conversion if this scan should not be skipped
-  7. Store JSON files
-  8. In case of a single NII ASL file loaded from PAR/REC, we need to shuffle the dynamics from CCCC...LLLL order to CLCLCLCL... order
-  9  Copy single dicom as QC placeholder
- 10. Store the summary info so it can be sorted and printed below



----
### xASL\_imp\_DCM2NII\_ReorderTimeEncoded.m

**Format:**

```matlab
xASL_imp_DCM2NII_ReorderTimeEncoded(nii_files, bTimeEncoded, timeEncodedMatrixSize, vectorPLD, resultJSON)
```

**Description:**

Reorder TEs and PLDs accordingly for time encoded sequences.



----
### xASL\_imp\_DCM2NII\_SanityChecks.m

**Format:**

```matlab
xASL_imp_DCM2NII_SanityChecks(x, thisSubject, thisVisit)
```

**Description:**

Sanity check for missing elements.



----
### xASL\_imp\_DCM2NII\_Subject\_SortASLVolumes.m

**Format:**

```matlab
[x, nii_files, summary_line, globalCounts, ASLContext] = xASL_imp_DCM2NII_Subject_SortASLVolumes(x, globalCounts, scanpath, scan_name, nii_files, iSubject, iVisit, iSession, iScan)
```

**Description:**

Sort ASL Volumes.

1. Fallbacks
2. Fill NIfTI Table
3. Get ASL context if possible
4. Only try shuffling if you dont know the ASL context already
5. Merge NIfTIs if there are multiples for ASL or M0, merge multiple files
6. Extract relevant parameters from nifti header and append to summary file



----
### xASL\_imp\_DCM2NII\_Subject\_StartConversion.m

**Format:**

```matlab
[globalCounts, x, summary_line, destdir, scanpath, scan_name, dcm2niiCatchedErrors, nii_files, first_match] = xASL_imp_DCM2NII_Subject_StartConversion(globalCounts, x, bSkipThisOne, summary_line, destdir, scanpath, scan_name, dcm2niiCatchedErrors)
```

**Description:**

Start of DCM2NII subject conversion.



----
### xASL\_imp\_DCM2NII\_Subject\_StoreJSON.m

**Format:**

```matlab
[parms, pathDcmDict] = xASL_imp_DCM2NII_Subject_StoreJSON(imPar, SavePathJSON, first_match, bUseDCMTK, pathDcmDict)
```

**Description:**

Store JSON.



----
### xASL\_imp\_DetermineStructureFromSourcedata.m

**Format:**

```matlab
[x] = xASL_imp_DetermineStructureFromSourcedata(x)
```

**Description:**

Determine structure from sourcedata.



----
### xASL\_imp\_DetermineSubjectStructure.m

**Format:**

```matlab
x = xASL_imp_DetermineSubjectStructure(x)
```

**Description:**

Determine subject/session/run structure from sourcedata or temp data.

The main goal is to create a sub-structure of x called x.importOverview.
x.importOverview does include a separate field for each subject.
Each subject has visit fields and visits have run fields.
All of this is used to track the number of subjects, number of visits, number of runs,
number of scans, etc. reliably. Within later parts of the pipeline it is also possible
to extract the current subject, current visit or current run to determine the part of
a population/subject/visit/run we're working on right now. We mostly used the variables
thisSubject, thisVisit, etc. for that.



----
### xASL\_imp\_Initialize.m

**Format:**

```matlab
imPar = xASL_imp_Initialize(studyPath, imParPath)
```

**Description:**

Initialize DCM2NII.

1. Read study file
2. Specify paths
3. Finalize the directories
4. Specify the tokens
5. Specify the additional details of the conversion



----
### xASL\_imp\_NII2BIDS\_Run.m

**Format:**

```matlab
x = xASL_imp_NII2BIDS_Run(x, bidsPar, studyPar, listRuns, nameSubjectSession, bidsLabel, iRun)
```

**Description:**

NII2BIDS conversion for a single run.

1. Make a subject directory with a correct session name
2. Convert structural and ASL runs



----
### xASL\_imp\_NII2BIDS\_RunAnat.m

**Format:**

```matlab
xASL_imp_NII2BIDS_RunAnat(bidsPar, studyPar, subjectSessionLabel, outSessionPath, listRuns, iRun, nameSubjectSession)
```

**Description:**

NII2BIDS conversion for a single sessions, single run.



----
### xASL\_imp\_NII2BIDS\_RunPerf.m

**Format:**

```matlab
xASL_imp_NII2BIDS_RunPerf(imPar, bidsPar, studyPar, subjectSessionLabel, inSessionPath, outSessionPath, listRuns, iRun)
```

**Description:**

NII2BIDS conversion for a single sessions, single run.

1. Define the pathnames
2. Load the JSONs and NIfTI information
3. BIDSify ASL
4. Prepare the link to M0 in ASL.json
5. BIDSify M0
6. Save all ASL files (JSON, NIFTI, CONTEXT) to the BIDS directory



----
### xASL\_imp\_NII2BIDS\_Subject\_DefineM0Type.m

**Format:**

```matlab
[jsonLocal, bJsonLocalM0isFile] = xASL_imp_NII2BIDS_Subject_DefineM0Type(studyPar, bidsPar, jsonLocal, pathM0, linkM0prefix)
```

**Description:**

Define M0 Type



----
### xASL\_imp\_PreallocateGlobalCounts.m

**Format:**

```matlab
x = xASL_imp_PreallocateGlobalCounts(nSubjects, subject, visit)
```

**Description:**

Preallocate space for (global) counts.



----
### xASL\_imp\_ReadSourceData.m

**Format:**

```matlab
x = xASL_imp_ReadSourceData(x)
```

**Description:**

Read source data.



----
### xASL\_imp\_StudyParPriority.m

**Format:**

```matlab
studyParSpecificSubjVisitSess = xASL_imp_StudyParPriority(studyParAll[, subjectName, sessionName, runName, bVerbose])
```

**Description:**

Takes studyPar with possibly several studyPar instances and resolves the priority and saves the individual studyPar.
First studyPar instance has the lowest priority, following ones are more important and review previous variables.
The alias hierarchy within each studyPar instance contains a list of regexp to be matched against subject/session/run.
If subject doesn't exist or regexp doesn't exist or aliasHierarchy doesn't exist, it allows it.



----
### xASL\_imp\_TokenBackwardsCompatibility.m

**Format:**

```matlab
imPar = xASL_imp_TokenBackwardsCompatibility(imPar)
```

**Description:**

Fix the token backwards compatibility.



----
### xASL\_imp\_UpdateDatasetRoot.m

**Format:**

```matlab
x = xASL_imp_UpdateDatasetRoot(x)
```

**Description:**

Update x.opts.DatasetRoot to dataset\_description.json after NII2BIDS conversion


## Initialization

----
### xASL\_init\_DataLoading.m

**Format:**

```matlab
[x] = xASL_init_DataLoading(x)
```

**Description:**

Load dataset by adding relevant fields to xASL x struct.



----
### xASL\_init\_DefaultEffectiveResolution.m

**Format:**

```matlab
[EffectiveResolution] = xASL_init_DefaultEffectiveResolution(PathASL, x)
```

**Description:**

This ExploreASL module provides an educated guess on
the effective spatial resolution of ASL. This may depend on the
combination of acquisition PSF, reconstruction filter, head motion.
Note that the derived/processed images may have a lower effective
resolution because of smoothing effects from interpolation. Note that
this remains an educated-guess, the actual FWHM may still differ,
especially for 3D GRASE sequences, where e.g. the choice of number of
segments can affect the smoothness.

This function conducts the following steps:

1. Educated-guess FWHM
2. Attempt accounting for in-plane interpolation in reconstruction
3. Calculate and report effective spatial resolution


----
### xASL\_init\_DefineDataDependentSettings.m

**Format:**

```matlab
[x] = xASL_init_DefineDataDependentSettings(x)
```

**Description:**

Define ExploreASL environment parameters, dependent of loaded data.


----
### xASL\_init\_DefineIndependentSettings.m

**Format:**

```matlab
[x] = xASL_init_DefineIndependentSettings(x)
```

**Description:**

Define ExploreASL environment parameters, independent of loaded data.


----
### xASL\_init\_DefinePaths.m

**Format:**

```matlab
[x] = xASL_init_DefinePaths(x)
```

**Description:**

Define paths used by ExploreASL.


----
### xASL\_init\_DefineStudyData.m

**Format:**

```matlab
[x] = xASL_init_DefineStudyData(x)
```

**Description:**

This initialization wrapper initializes the parameters for
this pipeline run, i.e. subjects, sessions (runs), timepoints (visits),
exclusions, sites, cohorts etc.

Note that ASL sessions are defined here as what BIDS calls "runs".

The "longitudinal\_Registration functions here manage different
TimePoints, which is what BIDS calls "visits".
With different structural scans, from the same participant. This is
managed by subject name suffixes \_1 \_2 \_n, and can be used for comparing
visits in the population module, or running SPM's longitudinal within-subject
registration.

Parallelization is allowed here by calling ExploreASL different times,
where it divides the subjects/images for processing across the nWorkers,
using iWorker as the reference for the part that the current ExploreASL
call will process. This requires having a Matlab license that can be
started multiple times on a server, or alternatively running the
ExploreASL compilation, and doesn't require the Matlab parallel toolbox.

This function exists from the following parts:

1. Manage included & excluded subjects
2. Create dummy defaults (exclusion list, ASL sessions)
3. Create list of total baseline & follow-up subjects, before exclusions
4. Create TimePoint data-lists
5. Manage exclusions
6. Add sessions as statistical variable, if they exist
7. Parallelization: If running parallel, select cases for this worker
8. Add Longitudinal TimePoints (different T1 volumes) as covariate/set, after excluding
9. Load & add statistical variables
10. Make x.S.SetsOptions horizontal if vertical by transposing
11. Create single site dummy, if there were no sites specified
12. Check for time points sets
13. Create list of baseline & follow-up subjects (i.e. after exclusion)
14. Check what excluded from which TimePoints




----
### xASL\_init\_DetermineRequiredPaths.m

**Format:**

```matlab

[x] = xASL_init_DetermineRequiredPaths(x)
```

**Description:**

Check the BIDS dataset root for the metadata JSON files.



----
### xASL\_init\_FileSystem.m

**Format:**

```matlab
[x] = xASL_init_FileSystem(x)
```

**Description:**

This function initializes the file system used throughout ExploreASL, for processing a single dataset/scan.
It is repeated for each scan, and runs the following parts:

1. Create folders
2. Subject/session definitions
3. Add prefixes & suffixes
4. Add Subject-specific prefixes
5. Add sidecars
6. Add atlas paths



----
### xASL\_init\_Import.m

**Format:**

```matlab
x = xASL_init_Import(x)
```

**Description:**

Initialization before xASL\_module\_Import.



----
### xASL\_init\_InitializeMutex.m

**Format:**

```matlab
[x] = xASL_init_InitializeMutex(x, ModuleName)
```

**Description:**

This function initializes the mutex/lock system of
ExploreASL for a module. Mutex (for mutual exclusion) is a
synchronization mechanism for enforcing limits of access to data (here a
module for a single scan) to allow parallelization. It also allows
stopping and continuing of ExploreASL. This function runs the following
steps:

1. Lock folder management
2. Initialize mutex object



----
### xASL\_init\_Iteration.m

**Format:**

```matlab
[bAborted, xOut] = xASL_init_Iteration(x, moduleName[, dryRun, stopAfterErrors])
```

**Description:**

Parses the settings and runs the DatabaseLoop sub-function.


----
### xASL\_init\_LoadDataParameterFile.m

**Format:**

```matlab
[x] = xASL_init_LoadDataParameterFile(x)
```

**Description:**

Load data parameter file.


----
### xASL\_init\_LoadMetadata.m

**Format:**

```matlab
[x] = xASL_init_LoadMetadata(x)
```

**Description:**

This function loads all metadata used in the study, either statistical
covariates (age, MMSE) or groups to compare between (site, sequence,
cohort), or parameters to be used in quantification/image processing

These parameters should be provided in .mat files in the root analysis
folder. Each .mat file should contain a single type of metadata, and
the filename should equal the variable name.
Metadata content should be a cell array
with subjects as first column and metadata as last column.
Sessions (runs) can be included as second column.

Metadata can be in any string or numerical format.

participants.tsv is now added per BIDS. It looks for metadata in participants.tsv first,
before going through the mat-files

This function iterates through the following steps for each variable:

1. Admin (what nOptions do we call ordinal, convert subject numeric to string, remove white spaces from data)
2. Get unique list of data options & check for missing data
3. Deal with data format (correct NaNs, deal with numeric vs strings)
4. Distinguish continous data (e.g. age) or ordinal data (groups to compare, e.g. cohort)
5. Check if data is complete for all subjects
6. Include complete data in x.S.SETS



----
### xASL\_init\_LongitudinalRegistration.m

**Format:**

```matlab
[SubjectNlist, TimePoint, IsSubject, SubjectID_FirstVolume, VolumeList, VolumeN] = xASL_init_LongitudinalRegistration(x)
```

**Description:**



This function initializes the longitudinal registration for ExploreASL,
which implements the SPM longitudinal registration.

This function recognizes and defines visits (i.e. multiple scans per subject) in the initialization of
ExploreASL, based on the suffixes \_1 \_2 \_n in the subject names (identified as foldernames).

Specifically, this function is called in the registration modules LongReg and DARTEL,
the first carrying out within-subject registration and
the second between-subject registration, based on the first time point only.

For the first function, we specify here a list of visits/timepoints that
should be registered longitudinally, for the second function we specify a
list of first visits only, as the between-subject registration in
ExploreASL is based on the first scan (as opposed to the average subject's scan).

This function runs the following steps:

1. Get TimePoint-list (list of visits)
2. Find subject IDs



----
### xASL\_init\_MapsAndAtlases.m

**Format:**

```matlab
x = xASL_adm_CleanUpX(x)
```

**Description:**

Add maps and atlases.



----
### xASL\_init\_PrintCheckSettings.m

**Format:**

```matlab
x = xASL_init_PrintCheckSettings(x)
```

**Description:**

Check whether pre-defined settings existed in `dataPar.json`.

Prints these on the screen as the start of the pipeline.
Runs following steps:

1. Set default settings if not defined
2. Print data/study specific settings
3. Print warnings


----
### xASL\_init\_PrintUserFeedback.m

**Format:**

```matlab
xASL_init_PrintUserFeedback(x)
```

**Description:**

Print user feedback.



----
### xASL\_init\_Process.m

**Format:**

```matlab
x = xASL_init_Process(x)
```

**Description:**

Initialization before ExploreASL\_Process.



----
### xASL\_init\_RemoveLockDirs.m

**Format:**

```matlab
[x] = xASL_init_RemoveLockDirs(x)
```

**Description:**

Remove 'lock-dir' if present from aborted previous run, for current subjects only.



----
### xASL\_init\_SubStructs.m

**Format:**

```matlab
[x] = xASL_init_SubStructs(x)
```

**Description:**

Initialize the ExploreASL x structure substructs/fields.
Only fields which do not exist so far are added.
This script is supposed to help with the overall modularity of ExploreASL.
This script is identical to the function ExploreASL\_Initialize\_SubStructs within
ExploreASL\_Initialize. We can not call this script from ExploreASL\_Initialize,
since the paths are not initialized at that part of the script yet.


----
### xASL\_init\_Toolboxes.m

**Format:**

```matlab
x  = xASL_init_Toolboxes(x)
```

**Description:**

Check & load ancillary toolboxes, versions and paths.


----
### xASL\_init\_VisualizationSettings.m

**Format:**

```matlab
[x] = xASL_init_VisualizationSettings(x)
```

**Description:**

This function defines several visualization settings are
used throughout ExploreASL's pipeline and tools, assuming a [121 145 121]
matrix with 1.5 mm isotropic resolution in MNI space.


----
### xASL\_init\_checkDatasetRoot.m

**Format:**

```matlab

[x] = xASL_init_checkDatasetRoot(x)
```

**Description:**

Check the ExploreASL parameter "DatasetRoot".



----
### xASL\_init\_printSettings.m

**Format:**

```matlab
xASL_init_printSettings(x)
```

**Description:**

Print chosen settings.


## Input and Output

----
### xASL\_io\_CheckDeprecatedFieldsX.m

**Format:**

```matlab
x = xASL_io_CheckDeprecatedFieldsX(x[, bVerbose])
```

**Description:**

Check deprecated fields of x and fix them based on a conversion table.
This table is used within:

- xASL\_bids\_parms2BIDS
- xASL\_io\_ReadDataPar
- xASL\_adm\_LoadParms
- xASL\_adm\_LoadX

It is not only used to convert deprecated x structure
fields to fields within up-to-date substructures of x, but
also to rename fields and to move them back and forwards
for the comparison with BIDS parameters within
xASL\_bids\_parms2BIDS e.g., which is why it is important to
make sure that if a row within the table is used to move &
rename, that there is also another row where the new
fieldname is moved to the same substructure.




----
### xASL\_io\_CreateNifti.m

**Format:**

```matlab
xASL_io_CreateNifti(pathNewNifti, imNew, resMat, nBits, bGZip)
```

**Description:**

This function creates a new NIfTI file, using the SPM "nifti" functionality, with the parameters
specified as input arguments. This function performs the
following steps:

1. Initialize NIfTI
2. Choose datatype (bit resolution)
3. Create scale slopes
4. Create orientation matrix
5. Write the new NIfTI, image matrix & scale slopes
6. Zip and deal with zipping (.nii vs. .nii.gz)



----
### xASL\_io\_DcmtkRead.m

**Format:**

```matlab
header = xASL_io_DcmtkRead(filepath, bPixel)
```

**Description:**

SHORT  Reads DICOM headers using DCMTK

FORMAT: header = xASL\_io\_DcmtkRead(filepath, bPixel)

INPUT:
filepath (string) - full path to the DICOM file
bPixel (bool) - read pixel data, default 0
OUTPUT:
header (structure) - structure containing parsed DICOM header


Calls the MEX function that uses DCMTK library to read the DICOM header.
To change which parameters are read and their names - the MEX file needs to be edited.
This function also corrects formating of certain parameters.



----
### xASL\_io\_ExportVTK.m

**Format:**

```matlab
xASL_io_ExportVTK(nifti, [mask, exportPath])
```

**Description:**

Export a VTK image file based on a 3D NIFTI or a 3D/4D image matrix.
4D images will be exported as a VTK time series (export-1.vtk, export-2.vtk, etc.).
This script uses vtkwrite (MIT License, Copyright 2016, Joe Yeh).



----
### xASL\_io\_MakeNifti4DICOM.m

**Format:**

```matlab
xASL_io_MakeNifti4DICOM(PathIn, x)
```

**Description:**

This function converts a NIfTI file to one that is ready to convert to DICOM for
PACS visualization purposes:

For scaling/visualization:

1. Remove peak signal
2. Remove valley signal
3. Remove NaNs
4. Rescale to 12 bit integers
5. Save NIfTI. We also zip the NIfTI as this NIfTI won't be opened by ExploreASL
6. Manage scale slope/datatype
7. Apply original orientation
8. Zip NIfTI



----
### xASL\_io\_MergeTextFiles.m

**Format:**

```matlab
xASL_io_MergeTextFiles(pathA,pathB,pathOut[,headerText])
```

**Description:**

Merge text files A and B and write the output to the pathOut file.


----
### xASL\_io\_PairwiseSubtraction.m

**Format:**

```matlab
xASL_io_PairwiseSubtraction(InputFile,outputPath,do_mask,switch_sign)
```

**Description:**

Subtracts controls from labels and takes mean.
Creates new perfusion-weighted delta\_M file, prefaced with 's'.
Converts into single precision floating point values (32 bit), removes scale slope.
Only runs if ['s' input\_file\_ASL] doesn't exist.
Remember to consider motion correction/ SPM realign (isotropically).
Alternative to this function is robust fit (Camille Maumet).



----
### xASL\_io\_ReadDataPar.m

**Format:**

```matlab
[x] = xASL_io_ReadDataPar(pathDataPar[, bStudyPar])
```

**Description:**

This function reads the data-parameter file, which is a file containing settings specific to processing a certain dataset or study
(abbreviated as DataPar) and creates the x-structure out of it. The file can be in .json or .m format.
The input file name pathDataPar is given as a string or character array. The output is the x structure. It only loads the data, removes the x-prefixes,
but keeps all the field names and units. It doesn't do any conversions to or from BIDS. The
only added value to normal json-read is that it detects invalid entries (numbers in strings, and
weird arrays), converts them correctly and reports this incorrect entries so that they can be manually
fixed. Also, if an .m file is provided, it converts and saves it to a JSON file (doesn't overwrite)
and reports that you should stop using .m files.




----
### xASL\_io\_ReadTextFileLineByLine.m

**Format:**

```matlab
textArray = xASL_io_ReadTextFileLineByLine(pathTextFile)
```

**Description:**

Read a text file line by line.


----
### xASL\_io\_ReadTheDicom.m

**Format:**

```matlab
[Info] = xASL_io_ReadTheDicom(bUseDCMTK, DicomPath)
```

**Description:**

This function tries to read a DICOM and throws a warning if it fails to


----
### xASL\_io\_SplitASL.m

**Format:**

```matlab
xASL_io_SplitASL(inPath[, iM0, iDummy])
```

**Description:**

This function splits ASL4D & M0 & Dummy images if they were in the same sequence.
If dcm2niiX has already splitted the ASL4D NIfTI, this is reconstructed first.
If no M0 exists, or only ASL splitting is desired, leave iM0 empty ([]).
The dummy scans can be excluded from the ASL sequence during the splitting. Both iM0 and iDummy
are the absolute positions of both in the original time series

Vendor product sequence examples:
GE 3D spiral sometimes puts the M0 at the last volume of the series -> iM0 = [2];
Philips 3D GRASE puts the M0 as control-label volume pair -> iM0 = [1 2];
Siemens 3D GRASE puts the M0 as the first volume -> iM0 = 1;
Some Siemens 3D GRASE puts a second Dummy control image -> iDummy = 2;

1. Input parameter admin
2. Prepare paths
3. First concatenate NIfTIs
4. Save M0 NIfTI
5. Determine ASL indices
6. Save ASL4D NIfTI
7. Split relevant JSON parameters/arrays
8. Split ASL4Dcontext.tsv
9. Modify JSON fields
10. Copy sidecars



----
### xASL\_io\_dcm2nii.m

**Format:**

```matlab
[niifiles, ScanNameOut, usedinput, msg] = xASL_io_dcm2nii(inpath, destdir, series_name, imPar, myPath)
```

**Description:**

Convert DICOM NIfTI/BIDS format using the dcm2nii command line utility.

1. Initial settings
2. Parse parameters
3. Locate dcm2nii executable
4. Check if we are reading a DICOM folder
5. Set dcm2niiX initialization loading
6. Check for existing targets
7. Create temporary subfolder for converting
8. Run dcm2nii and move files to final destination using specified series name
9. Cleanup temp
10. Optionally return the used input file



## QC

----
### xASL\_qc\_AddLoggingInfo.m

**Format:**

```matlab
[x] = xASL_qc_AddLoggingInfo(x, loggingEntry)
```

**Description:**

Logging of errors and warnings within the x structure.



----
### xASL\_qc\_AsymmetryIndex.m

**Format:**

```matlab
[AI_perc] = xASL_qc_AsymmetryIndex(ImageIn)
```

**Description:**

Extract voxel-wise asymmetry index for QC purposes.



----
### xASL\_qc\_CAT12\_IQR.m

**Format:**

```matlab
[QA_Output] = xASL_qc_CAT12_IQR(InputImage, InputC1, InputC2, InputC3, bFLAIR)
```

**Description:**

Prepare and run CAT12s QC parameters (also for other images).



----
### xASL\_qc\_CheckValidityJSON.m

**Format:**

```matlab
[IsValid] = xASL_qc_CheckValidityJSON(PathJSON)
```

**Description:**

This function loads a QC JSON (simply JSON file, won't
take any exotic files) and simply check whether there is any empty value
after a key. If this is the case, it will throw a warning, which will skip
reading this JSON by the compiled spm\_jsonread, avoiding the crash that
this may result in.


----
### xASL\_qc\_CollectParameters.m

**Format:**

```matlab
x = xASL_qc_CollectParameters(x, iSubject, ScanType, CollectQCFunction)
```

**Description:**

This function collects QC parameters for a module.


----
### xASL\_qc\_CollectQC\_ASL.m

**Format:**

```matlab
[x] = xASL_qc_CollectQC_ASL(x, iSubject)
```

**Description:**

This functions collects QC parameters for the ASL module

These are stored in x.Output.ASL:

ID - SubjectName
ASL\_LR\_flip\_YesNo - Checks whether any image processing changed the left-right orientation
by checking whether the determinant differs between nii.mat & nii.mat0
SPM realign (too much motion is suspicious)
MotionMean\_mm    - mean motion
MotionExcl\_Perc  - percentage of excluded outliers
MotionMax\_mm     - max motion
MotionSD\_mm      - SD motion

ASL quantification (strange average CBF, or strange GM-WM contrast)
ASL acquisition parameters (should be fairly consistent over subjects/scans):
TE - echo time
TR - repetition time
RescaleSlope - Philips
Scaleslope - Philips
Matrix X Y Z - matrix size
Matrix Z - number of slices
VoxelSize X Y - in plane resolution
VoxelSize Z - slice thickness
RigidBody2Anat\_mm - Net Displacement Vector (RMS) from ASL to T1w image (mm) from registration


----
### xASL\_qc\_CollectQC\_Structural.m

**Format:**

```matlab
[x] = xASL_qc_CollectQC_Structural(x, iSubject)
```

**Description:**

This functions collects QC parameters for the structural module
These are stored in x.Output.Structural:
ID - SubjectName
T1w\_LR\_flip\_YesNo - Checks whether any image processing changed the left-right orientation
by checking whether the determinant differs between nii.mat & nii.mat0
LST output:
WMH\_vol\_mL        - WMH volume
WMH\_n             - WMH number of lesions
CAT12 output:
T1w\_IQR\_Perc      - CAT12 IQR quality parameter for T1w
volumetric: GM\_vol\_mL, WM\_vol\_mL, CSF\_vol\_mL, ICV\_vol\_mL, GM\_ICV\_Ratio


----
### xASL\_qc\_CollectQC\_func.m

**Format:**

```matlab
[x] = xASL_qc_CollectQC_func(x, iSubject)
```

**Description:**

This functions collects QC parameters for the func module

These are stored in x.Output.func:

ID - SubjectName
func\_LR\_flip\_YesNo - Checks whether any image processing changed the left-right orientation
by checking whether the determinant differs between nii.mat & nii.mat0
SPM realign (too much motion is suspicious)
MotionMean\_mm    - mean motion
MotionExcl\_Perc  - percentage of excluded outliers
MotionMax\_mm     - max motion
MotionSD\_mm      - SD motion
func quantification (strange average CBF, or strange GM-WM contrast)
CBF\_GM\_Median\_mL100gmin - median GM CBF
CBF\_WM\_Median\_mL100gmin - median WM CBF
SpatialCoV\_GM\_Perc      - GM spatial CoV
SpatialCoV\_WM\_Perc      - WM spatial CoV
CBF\_GM\_WM\_Ratio         - GM-WM CBF ratio

func acquisition parameters (should be fairly consistent over subjects/scans):
TE - echo time
TR - repetition time
RescaleSlope - Philips
Scaleslope - Philips
Matrix X Y Z - matrix size
Matrix Z - number of slices
VoxelSize X Y - in plane resolution
VoxelSize Z - slice thickness
RigidBody2Anat\_mm - Net Displacement Vector (RMS) from func to T1w image (mm) from registration


----
### xASL\_qc\_CollectSoftwareVersions.m

**Format:**

```matlab
[x] = xASL_qc_CollectSoftwareVersions(x)
```

**Description:**

This functions collects software versions for Matlab, SPM, CAT, LST & ExploreASL
If FSL is installed, it will obtain its version as well.
These are stored in x.Output.Software.


----
### xASL\_qc\_CompareTemplate.m

**Format:**

```matlab
[QC] = xASL_qc_CompareTemplate(x, ModPrefix, iSubjectSession)
```

**Description:**

This function computes several advanced template-based QC parameters:
RMSE\_Perc        - Root Mean Square Error between image and template (%)
nRMSE\_Perc       - Same but then normalized
AI\_Perc          - Asymmetry Index between image and template (%)
Mean\_SSIM\_Perc   - mean structural similarity index -> xASL\_stat\_MeanSSIM.m
PeakSNR\_Ratio    - peak signal-to-noise ratio -> xASL\_stat\_PSNR.m


----
### xASL\_qc\_ComputeFoVCoverage.m

**Format:**

```matlab
[CoveragePerc] = xASL_qc_ComputeFoVCoverage(InputPath, x)
```

**Description:**

This function computes the intersection/overlap between
brainmask on field-of-view (FoV) of low resolution image
(native space) & the same brainmask with expanded FoV.
It uses the pGM+pWM+pCSF as brainmask
This assumes that the structural reference image has full brain coverage,
and was properly segmented into GM, WM and CSF
Also, we assume that the InputPath contains a single 3D volume


----
### xASL\_qc\_ComputeNiftiOrientation.m

**Format:**

```matlab
[structOut] = xASL_qc_ComputeNiftiOrientation(PathNIfTI[, structIn])
```

**Description:**

It loads the input Nifti, finds its dimension, voxel size and a net vector distance from its
original position before registration. Adds all these information into an output structure structOut
while copying all from structIn and keeping it intact.



----
### xASL\_qc\_CreatePDF.m

**Format:**

```matlab
xASL_qc_CreatePDF(x[, DoSubject])
```

**Description:**

This function iterates over all values in x.Output and all
images in x.Output\_im, and prints them in a PDF file.
x.Output & x.Output\_im should contain the QC/result output
of all ExploreASL pipeline steps.

Further code explanation:
Below, using the Matlab & SPM Figure tools we create an image, which is
then printed to a PDF file

- fg = the main Figure handle
- ax = "axes" handles, these are objects containing either 1) text or 2)
images, with fg as "parent" (1) & (2) images have ax as "parent"
Positions are calculated in such a way that 4 categories can be printed,
which will be the first 4 fields found in x.Output
then allowing 8 single slice images, and 15 text lines (name & value
columns)


----
### xASL\_qc\_FA\_Outliers.m

**Format:**

```matlab
[FA_Outliers_mL] = xASL_qc_FA_Outliers(InputFA)
```

**Description:**

Extract the number of FA outliers, i.e. values of FA
above 1 or below 0, from a FA image.




----
### xASL\_qc\_ObtainQCCategoriesFromJPG.m

**Format:**

```matlab
xASL_qc_ObtainQCCategoriesFromJPG(x)
```

**Description:**

This function obtains QC categories as covariant/set,
based on the JPGs in //Population/ASLCheck. These are initially sorted by
spatial CoV, and should be visually checked & put in the correct folder.
-------------------------------------------------------------------------------------------------------------------------

----
### xASL\_qc\_PCPStructural.m

**Format:**

```matlab
[anatQA] = xASL_qc_PCPStructural(PathT1, Pathc1T1, Pathc2T1, x, PopPathT1)
```

**Description:**

This function computes several anatomical QC parameters as proposed in SPM Univariate Plus:

- WM\_ref\_vol\_mL    - volume of the WM reference region (mL)
- WMref\_vol\_Perc   - same but as percentage of total WM volume
- SNR\_GM           - GM signal-to-Noise Ratio (SNR), ie the mean intensity within GM divided
by SD of WM reference region. Higher = better.
- CNR\_GM\_WM        - GM-WM Contrast-to-Noise Ratio (CNR), i.e. the mean of GM - mean of WM
divided by the SD of the WM reference region. Higher = better.
- FBER\_WMref\_Ratio - Foreground to Background Energy Ratio (FBER), i.e. the variance of voxels within the brain (in pGM+pWM mask)
divided by the variance of voxels in the WM reference region. Higher = better.
- EFC\_bits         - Shannon Entropy Focus Criterion (EFC), i.e. the entropy of voxel intensities proportional to the maximum
possibly entropy for a similarly sized image. Indicates ghosting and head motion-induced blurring. Lower = better.
- Mean\_AI\_Perc     - mean relative voxel-wise absolute Asymmetry Index (AI) within the brain (pGM+pWM mask) (%)
- SD               - same but SD (%)

REFERENCES:
Preprocessed Connectome Project Quality Assurance Protocol (QAP):
http://preprocessed-connectomes-project.org/quality-assessment-protocol/
http://ieeexplore.ieee.org/document/650886/


----
### xASL\_qc\_PrintOrientation.m

**Format:**

```matlab
xASL_qc_PrintOrientation(niftiList, outputDir, outputFile);
```

**Description:**

This function lists NifTI orientation matrices before and after
image processing, respectively nii.mat0 and nii.mat. In ExploreASL this
is used for QC to detect accidental left-right flips, as these can occur
unnoticed as the brain structure appears relatively symmetrical.
This can be detected by negative determinants.
Also, this can be used to detect any significant differences in
acquisition or image processing.

So orientation parameters and determinants should be similar across
all scans from single scanner/coil, and registration should not
give a relatively negative determinant.
Results are saved in a TSV file

This functions performs the following steps:
1. Print the header
2. Load the data
3. Print original orientation matrix
4. Print current orientation matrix
5. Print registration transformation matrix
6. Print FileName
7. Get statistics (mean & SD)



----
### xASL\_qc\_ReportLeftRightFlips.m

**Format:**

```matlab
xASL_qc_ReportLeftRightFlips(dirRoot [, bZip])
```

**Description:**

This function identifies and reports illegal left-right
flips for image matrices within a NIfTI. This can be useful as these are
not readily observed by the human eye, as the left and right hemispheres
are too symmetrical by default.

All NifTIs are found recursively (i.e. in the folder and its subfolders),
irregardless of them being .nii or .nii.gz.


----
### xASL\_qc\_TanimotoCoeff.m

**Format:**

```matlab
TC = xASL_qc_TanimotoCoeff(Image1, Image2[, imMask, type])
```

**Description:**

Compares images Image1 and Image2 within the mask imMask. TYPE specifies the input data type.

RATIONALE:   Note that the Tanimoto Coefficient is a measure of image
overlap/intersection, similar to the Dice coefficient. With
the option type 3, this is a fuzzy coefficient, which
doesn't require to convert the two images to a binary mask.
The TC can be interpreted as a stringent Kappa, ranging from
0 (completely dissimilar) to 100% (identical images).
Assuming that perfect registration should not lead to
identical images but still retain physiological differences,
TC>70% can be regarded as excellent image agreement. The TC
will be overestimated when smoothing, but this may lead to more stable artifact detection.


----
### xASL\_qc\_WADQCDC.m

**Format:**

```matlab
xASL_qc_WADQCDC(x, iSubject[, ScanType])
```

**Description:**

This QC function runs WAD-QC specific Python script to zip QC information &
incorporate this into a DICOM field for analysis on the
WAD-QC server, by the following:

1. Define QCDC script: this is the Python script written by
Gaspare, edited by Joost Kuijer & copied to the EPAD
CustomScripts folder of ExploreASL
2. Python installation location is checked, with several known
locations, for several servers. If we cannot find it,
the QCDC is not ran
3. Previous QCDC results are cleaned. QCDC stores all its
results in a separate folder (Something like 2 layers up from the current
folder, here referred to as QCDCDir = [x.D.ROOT 'qcdc\_output'])
from these result files, only the filled DICOM file is
interesting, all the rest are copies of the QC results
that we embedded into the DICOM
4. Run QCDC (if Python installation detected)
The following files need to be set as executable:
('QCDC', 'src', 'qc\_data\_collector.py')
('QCDC', 'src', 'bash', 'create\_dcm\_only\_wadqc.sh')
('QCDC', 'src', 'bash', 'sendwadqc.sh')
5. Clean up new QCDC results (as above) & move the filled
DICOM to ['qcdc\_' DummyFile] within the current ScanType
folder
6. Sending the DICOM to the WAD-QC server using storescu


----
### xASL\_qc\_WADQC\_GenerateDescriptor.m

**Format:**

```matlab
xASL_qc_WADQC_GenerateDescriptor(x, iSubject)
```

**Description:**

This QC function generates a JSON descriptor for Gaspare'
QCDC script, by the following steps:

- a) include information about where to find the dummy DICOM (i.e. placeholder DICOM)
- b) For ExploreASL' QC fields (as passed through in
x.Output), here we note all these QC fields for each
ScanType, as the x.Output should have been collected
equally in the QC file 'QC\_collection\_SubjectName.json'
by function xASL\_qc\_CollectParameters
- c) Subfunction xASL\_qc\_WADQC\_images - Includes visual standard space QC
images, by searching them on prescribed paths within the
Population folder (where currently all derivatives reside)
- d) Insert the PDF report; this PDF report is
subject-specific, not scan-specific. For completeness it
is added to each QCDC descriptor
- e) Add WAD-QC server details (i.e. IP address etc)
- f) Save the Descriptor JSON file.


----
### xASL\_qc\_temporalSNR.m

**Format:**

```matlab
tSNR = xASL_qc_temporalSNR(pathIm4D,pathImTissueProb)
```

**Description:**

This function computes several temporal SNR QC parameters as proposed in SPM Univariate Plus:
tSNR.tSNR\_GM\_Ratio      : mean GM signal / std GM over time
tSNR.tSNR.tSNR\_WM\_Ratio : mean WM signal / std WM over time
tSNR.tSNR.tSNR\_CSF\_Ratio: mean CSF signal / std CSF over time
tSNR.tSNR\_WMref\_Ratio   : mean signal/std over time in eroded deep WM
tSNR.tSNR\_GMWM\_Ratio    : mean (GM+WM) signal / sqrt(std(GM+WM)^2+std(WMref)^2)
tSNR.tSNR\_GMWM\_WMref\_Ratio: mean (GM+WM) signal / std WMref over time
tSNR.tSNR\_Physio2Thermal\_Ratio: sqrt((tSNR(GM+WM)/tSNR\_GMWM\_WMref\_Ratio))^2-1)
tSNR.tSNR\_Slope\_Corr:

Differences to the SPM U+ suggestion:

- eroded WM is used for estimating background noise
- Brainmask is determined in the same way as the structural anatQC,
- CSF is determined from the pGM&pWM maps;

REFERENCES:
1. Thomas Liu (2016). Noise contributions to the fMRI signal: An overview NeuroImage, 343, 141-151
http://dx.doi.org/10.1016/j.neuroimage.2016.09.008
2. Cesar Caballero-Gaudes and Richard C. Reynolds (2016). Methods For Cleaning The BOLD fMRI Signal. NeuroImage, 154,128-149
3. Lawrence Wald and Jonathan R Polimeni (2016). Impacting the effect of fMRI noise through
hardware and acquisition choices ??? Implications for controlling false positive rates. NeuroImage, 154,15-22
4. SPM Utility + toolbox. Cyril Pernet. https://osf.io/wn3h8/


## Quantization

----
### xASL\_quant\_AgeSex2Hct.m

**Format:**

```matlab
[Hematocrit] = xASL_quant_AgeSex2Hct([age, sex])
```

**Description:**

This function estimates a participants hematocrit, based on
literature-based values for age and sex. It performs the following steps:

1. Warning unknown age/sex
2. Imputing unknown age/sex
3. Define hematocrit per age for unknown sex
4. Define Hematocrit per age for males
5. Define Hematocrit per age for females



----
### xASL\_quant\_BSupCalculation.m

**Format:**

```matlab
signalPercentage = xASL_quant_BSupCalculation(BackgroundSuppressionPulseTime, ReadoutTime[, PresaturationTime, T1Time, SliceTime, PathGraph])
```

**Description:**

This function computes the tissue signal percentage that
remains after background suppression pulses are played in the ASL
acquisition.
It assumes that the signal is, at first, optionally saturated by a 90 degree flip at PresaturationTime before readout.
Then follows a series of BSup pulses (times before readout are given) that do a 180 degree flip. The observed tissue relaxes with time
T1time and the signal attenuation is calculated for several slices acquired at times relative to the readout.



----
### xASL\_quant\_Basil.m

**Format:**

```matlab
[CBF_nocalib, ATT_map, Tex_map, resultFSL] = xASL_quant_Basil(PWI, x)
```

**Description:**

This script performs quantification of the PWI using the FSL Basil/Fabber pipeline. Final calibration to
physiological units is performed by dividing the quantified PWI by the M0 image/value.
Fabber is used instead of Basil for multiTE data.

This function performs the following steps:

1. Define paths
2. Delete previous BASIL/Fabber output
3. Write the PWI as Nifti file for Basil/Fabber to read as input
4. Create option\_file that contains options which are passed to the FSL command
5. Run Basil and retrieve CBF output
6. Scaling to physiological units
7. Householding



----
### xASL\_quant\_FEAST.m

**Format:**

```matlab
xASL_quant_FEAST(x)
```

**Description:**

This function quantifies ATT using the FEAST equations,
using crushed and non-crushed sessions, of which the ratio is
proportional to ATT.
Note that the order of sessions should be 1) crushed 2) non-crushed

This function runs the following steps:

1. Skip this function if no FEAST data available
2. Admin
3. Load data & correct for timing differences (PLD etc)
4. Smooth and clip CBF maps & FEAST ratio
5. Compute TT maps



----
### xASL\_quant\_GetControlLabelOrder.m

**Format:**

```matlab
[ControlIm, LabelIm, OrderContLabl] = xASL_quant_GetControlLabelOrder(ASLTimeSeries)
```

**Description:**

This function automatically checks (and corrects if required)
the control and label order of ASL timeseries
based on the larger signal in control volumes.
It supposes that data is acquired in pairs. Works also for multiPLD but only for sequences
with alternative control/label or label/control order


----
### xASL\_quant\_GetMeanControl.m

**Format:**

```matlab
imMeanControl = xASL_quant_GetMeanControl(x, imASLTimeSeries)
```

**Description:**

This function calculates the mean control image. It automatically checks (and corrects if required)
the control order of ASL timeseries and takes into account if multiPLD or Hadamard data are provided


----
### xASL\_quant\_HadamardDecoding.m

**Format:**

```matlab
[imDecoded] = xASL_quant_HadamardDecoding(imPath, xQ)
```

**Description:**

Hadamard-4 & Hadamard-8 Decoding.

0. Admin: Check inputs, load data
1. Specify the decoding matrix
2. Reorder multi-TE data
3. Decode the Hadamard data
4. Reorder multi-TE back to the initial order of PLD/TE
5. Normalization of the decoded data



----
### xASL\_quant\_Hct2BloodT1.m

**Format:**

```matlab
BloodT1 = xASL_quant_Hct2BloodT1(Hematocrit, Y, B0, bVerbose)
```

**Description:**

This function converts hematocrit to blood T1, according to
calculations defined by Patrick Hales. With courtesy and thanks!
Note that we assume a venous O2 saturation of 68% (Yv=0.68)

This function performs the following steps:

1. Check fraction vs percentage hematocrit & Y, should be between 0 and 1
2. Specify defaults (Hb, Fe)
3. Perform calculation
4. Convert s to ms
5. Print what we did

--------------------------------------------------------------------------------------------------------------

----
### xASL\_quant\_M0.m

**Format:**

```matlab
[M0IM] = xASL_quant_M0(inputM0, x)
```

**Description:**

This function quantifies the M0, except for the difference in voxel size
between the M0 and ASL source data (which is scaled in
xASL\_wrp\_ProcessM0.m). This function runs the following steps:

1. Correct scale slopes, if Philips
2. Convert control image with background suppression to pseudo-M0
3. Skip M0 quantification if ~x.modules.asl.ApplyQuantification(4)
4. Set TR specifically for GE
5. Check for correct TR values
6. Quantify the M0, either for single 3D volume or slice-wise
7. Apply custom scalefactor if requested (x.modules.asl.M0\_GMScaleFactor)



----
### xASL\_quant\_MultiPLD.m

**Format:**

```matlab
[ScaleImage[, CBF, ATT, Tex]] = xASL_quant_MultiPLD(PWI, M0_im, imSliceNumber, x[, bUseBasilQuantification])
```

**Description:**

This script performs a multi-step quantification, by
initializing a ScaleImage that travels through this script & gets changed by the following quantification
factors:

1.    **PLD scalefactor** (gradient if 2D multi-slice) (if x.modules.asl.ApplyQuantification(3))
2.    **Label decay scale factor** for single (blood T1) - or dual-compartment (blood+tissue T1) model, CASL or PASL
Single-compartment model: Alsop MRM 2014
Dual-compartment model: Wang MRM 2002: Gevers JMRI 2012 (if x.modules.asl.ApplyQuantification(3))
3.    **Scaling to physiological units** [ml/gr/ms =>ml/100gr/min =>(60,000 ms=>min)(1 gr=>100gr)]
(if x.modules.asl.ApplyQuantification(3))
4.    **Manufacturer-specific scalefactor** (if x.modules.asl.ApplyQuantification(1) -> future move to dcm2niiX stage)
Finally, we:
5.    Divide PWI/M0 (if x.modules.asl.ApplyQuantification(5)) & apply all scaling (if x.modules.asl.ApplyQuantification(6))
6.    Print parameters used

Note that the output always goes to the CBF image (in the
future this could go to different stages, e.g. dcm2niiX or
PWI stage)

Note that BASIL is also implemented, but it doesn't allow a
standard space quantification yet (it would need to use
imSliceNumber)



----
### xASL\_quant\_SinglePLD.m

**Format:**

```matlab
[ScaleImage[, CBF]] = xASL_quant_SinglePLD(PWI, M0_im, imSliceNumber, x)
```

**Description:**

This script performs a multi-step quantification, by
initializing a ScaleImage that travels through this script & gets changed by the following quantification
factors:

1.    **PLD scalefactor** (gradient if 2D multi-slice) (if x.modules.asl.ApplyQuantification(3))
2.    **Label decay scale factor** for single (blood T1) - or dual-compartment (blood+tissue T1) model, CASL or PASL
Single-compartment model: Alsop MRM 2014
Dual-compartment model: Wang MRM 2002: Gevers JMRI 2012 (if x.modules.asl.ApplyQuantification(3))
3.    **Scaling to physiological units** [ml/gr/ms =>ml/100gr/min =>(60,000 ms=>min)(1 gr=>100gr)]
(if x.modules.asl.ApplyQuantification(3))
4.    **Manufacturer-specific scalefactor** (if x.modules.asl.ApplyQuantification(1) -> future move to dcm2niiX stage)
Finally, we:
5.    Divide PWI/M0 (if x.modules.asl.ApplyQuantification(5)) & apply all scaling (if x.modules.asl.ApplyQuantification(6))
6.    Print parameters used

Note that the output always goes to the CBF image (in the
future this could go to different stages, e.g. dcm2niiX or
PWI stage)

Note that BASIL is also implemented, but it doesn't allow a
standard space quantification yet (it would need to use
imSliceNumber)



----
### xASL\_quant\_SliceTiming.m

**Format:**

```matlab
SliceTiming = xASL_quant_SliceTiming(x, inputIm)
```

**Description:**

This function takes the x.Q.SliceReadoutTime and returns the SliceTiming parameter.
The function creates a vector (of the relatives timings for each slices) out of it with the correct
length corresponding to the number of slices in the inputIm
corresponding to the BIDS definition. It also checks the x.Q.readoutDim, and for 3D readouts it returns 0.
It loads the image from inputIm and calculates the SliceTiming according to the number of slices in the third dimension
If a path is given, it also checks if it can find a JSON sidecar, then it loads the JSON sidecar, and looks for SliceTiming inside it. If
SliceTiming/SliceReadoutTime is found in the JSON sidecar, it prioritize it over the value in the x-struct
For reference, we use these terms:

SliceTiming (the BIDS parameter) - it is a vector with the same length as the number of slices and contains the timing of the start
of the readout of each slice relative to the first slice
SliceReadoutTime - Legacy xASL parameter that will be phased out. It contains either a vector matching the BIDS definition of SliceTiming
or a scalar with difference in readout times between the consecutives slices (i.e. the xASL legacy definition of SliceTiming)
SliceTimingDiff - Internal parameter in this function for calculating the time difference between consecutive slices.

0. Admin
1. ShortestTR
2. Assign the vector value and check for vector consistency


----
### xASL\_quant\_SliceTiming\_ShortestTR.m

**Format:**

```matlab
[x] = xASL_quant_SliceTiming_ShortestTR(x)
```

**Description:**

When the TR is set to "shortestTR" in the ASL acquisition,
each ASL scan will have its unique TR. As this is shortest,
there won't be a delay between the readout of the last slice
and the end of the TR. Therefore, the time to read out all
slices is TR - InitialPostLabelDelay - LabelingDuration, and
dividing this by the number of slices gives the
SliceReadoutTime



## SPM

----
### xASL\_spm\_BiasfieldCorrection.m

**Format:**

```matlab
xASL_spm_BiasfieldCorrection(PathIn, SPMdir, Quality, MaskName, PathOut)
```

**Description:**

This function is a wrapper around the SPM "old segment"
function, for biasfield removal. It is tested for M0 and mean control
images. It conducts the following steps:

1. Create implicit mask
2. Define SPM 'old segmentation' settings
3. Run SPM 'old segmentation'
4. Delete temporary files
5. Rename temporary SPM file into output file



----
### xASL\_spm\_affine.m

**Format:**

```matlab
xASL_spm_affine(srcPath, refPath, fwhmSrc, fwhmRef[,otherList, bDCT, bQuality])
```

**Description:**

This SPM wrapper runs SPM's old normalize-estimate function, which calculates the affine transformation (i.e. linear + zooming and shearing) that is required to
align the source image with the reference image. By default it does not estimate the low-degree Discrete Cosine Transform (DCT) to have a simple affine transformation
but this can be enabled in this wrapper. Also note that this affine transformation uses a correlation cost function, hence it requires the source and reference images
to have similar contrasts and resolution - or provide the resolution estimates so the smoothing can be done.
This function does not change the orientation header by default, but it allows to change those of others through the otherList. If bDCT is used or no otherList given,
it stores its estimated affine transformation as orientation difference matrix in a file with the same path but \_sn.mat extension.
For the provided smoothing FWHM, note that smoothnesses combine with Pythagoras' rule (i.e. square summing).



----
### xASL\_spm\_coreg.m

**Format:**

```matlab
xASL_spm_coreg(refPath, srcPath[, OtherList, x, sep, FastReg])
```

**Description:**

This SPM wrapper runs SPMs coregister-estimate function, which calculates the 6 parameter rigid-body transformation (a.k.a. linear) that is required to
align the source image with the reference image. This 6 parameter transformation (i.e. 3 XYZ translations and 3 rotations) is applied to
the orientation header of the source NIfTI image, and also to the images provided in OtherList (optional).
Note that this SPM registration function uses a normalized mutual information (NMI) by default, enabling registration between two images
with different contrast.
Note that this algorithm will use the first volume of a multi-volume NIfTI



----
### xASL\_spm\_deface.m

**Format:**

```matlab
xASL_spm_deface(PathIn, bReplace)
```

**Description:**

This function removes the face from an anatomical NIfTI
image, e.g. T1w or FLAIR, for disidentification/privacy purposes.
When this script is run after the ExploreASL structural
module, it does a pretty good job even for 2D images.
However, note that this can always fail, strip part of
the brain, or change the output of pipelines. So best not
to compare results from defaced and non-defaced images.
Also, note that defacing makes it difficult to ensure that
the FLAIR and T1w are from the same subject.




----
### xASL\_spm\_deformations.m

**Format:**

```matlab
xASL_spm_deformations([x,] PathIn, PathOut[, Interpolation, InverseSpace, AffineTrans, DeformationPath])
```

**Description:**

This ExploreASL wrapper manages the SPM deformation tool.
It takes multiple (ExploreASL pipeline) transformations and combines/concatenates them
into a single transformation prior to applying it to the input images.
This allows to apply multiple transformations with a single interpolation, avoiding
propagation of undesired interpolation effects. Mainly used to get native
space images into standard space, or vice versa.
Best to combine as many files as possible within this function, since the
deformation calculation (which is the most computation intensive part) needs to be performed once for multi-file resampling
This function runs the following steps:



## Statistics

----
### xASL\_stat\_AtlasForStats.m

**Format:**

```matlab
[x] = xASL_stat_AtlasForStats(x)
```

**Description:**

This function loads atlases, checks them, and
their ROI names, for later use as ROI definition in xASL\_stat\_GetROIstatistics
Note that the atlases should be integer values, or different 4rd
dimensions (i.e. multiple images), that are mutually
exclusive. This function takes the following steps:

1. Load atlas ROI names
There should be a TSV sidecar to the atlas NIfTI file, as
explained above.
2. deal with memory mapping
3. Resample atlas 50 1.5 mm^3 MNI
4. Converted atlas with integers to 4D binary image
5. Convert/compress masks into Columns
6. Print atlas overview image



----
### xASL\_stat\_ComputeDifferCoV.m

**Format:**

```matlab
diffCoV = xASL_stat_ComputeDifferCoV(imCBF)
diffCoV = xASL_stat_ComputeDifferCoV(imCBF,imMask)
diffCoV = xASL_stat_ComputeDifferCoV(imCBF,imMask,bPVC,imGM,imWM)
diffCoV = xASL_stat_ComputeDifferCoV(imCBF,imMask,bPVC,imGM,imWM,b3D)
```

**Description:**

It calculates the spatial DiffCoV value on finite part of imCBF. Optionally a mask IMMASK is provide,
and PVC is done for bPVC==2 using imGM and imWM masks and constructing
pseudoCoV from pseudoCBF image. For bPVC~=2, imGM and imWM are ignored. It is calculated in 2D or assuming also 3D edges based on B3D.
Calculate derivate spatial CoV, by summing up differences in CBF between neighbors.
The derivative uses Sobels filter.


----
### xASL\_stat\_ComputeMean.m

**Format:**

```matlab
[CBF_GM CBF_WM] = xASL_stat_ComputeMean(imCBF[, imMask, nMinSize, bPVC, bParametric, imGM, imWM])
```

**Description:**

It calculates mean or median of CBF over the mask imMask if the mask volume exceeds nMinSize. It calculates either
a mean, a median, or a mean after PVC, depending on the settings of bPVC. For the PVC options, it needs also imGM and imWM and returns the
separate PV-corrected values calculated over the entire ROI.

1. Admin
2. Mask calculations
3. Calculate the ROI statistics
3a. No PVC and simple mean
3b. No PVC and median
3c. Simple PVC
3d. Full PVC on a region


----
### xASL\_stat\_ComputeSpatialCoV.m

**Format:**

```matlab
sCov = xASL_stat_ComputeSpatialCoV(imCBF[, imMask, nMinSize, bPVC, bParametric, imGM, imWM])
```

**Description:**

It calculates the spatial CoV value on finite part of imCBF. Optionally a mask IMMASK is provide,
ROIs of size < NMINSIZE are ignored, and PVC is done for bPVC==2 using imGM and imWM masks and constructing
pseudoCoV from pseudoCBF image. For bPVC~=2, imGM and imWM are ignored

1. Admin
2. Create masks
3. sCoV computation


----
### xASL\_stat\_EqualVariancesTest.m

**Format:**

```matlab
[resTest, P] = xASL_stat_EqualVariancesTest(X[, alpha, type])
```

**Description:**

Brown-Forsythe or Levene's test for equality of variances. The response variable is
transformed (yij = abs(xij - median(xj)) for Brown-Forsythe and yij = abs(xij - mean(xj))
for Levene's test). And then runs a one-way ANOVA F-test to check if the variances are equal.


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
### xASL\_stat\_MadNan.m

**Format:**

```matlab
y = xASL_stat_MadNan(x[,flag, dim])
```

**Description:**

Calculates a Median/Mean Absolute deviation, but ignoring NaNs in the calculation.
xASL\_stat\_MadNan(X) or xASL\_stat\_MadNan(X,0) computes xASL\_stat\_MeanNan(ABS(X-xASL\_stat\_MeanNan(X))
xASL\_stat\_MadNan(X,1) computes xASL\_stat\_MedianNan(ABS(X-xASL\_st\_MedianNan(X)).



----
### xASL\_stat\_MeanSSIM.m

**Format:**

```matlab
mssim=xASL_stat_MeanSSIM(imRef,imSrc[,dynRange])
```

**Description:**

Calculates the similarity index according to Want et al.


----
### xASL\_stat\_MultipleLinReg.m

**Format:**

```matlab
[b,CI,pval,stats] = xASL_stat_MultipleLinReg(X,Y[,bIntercept])
```

**Description:**


Performs a multiple linear regression Y=b\*X+a and provides the intercept and regression coefficients beta
including their significance and confidence intervals. It calculates additionally the goodness of the fit.


----
### xASL\_stat\_PSNR.m

**Format:**

```matlab
PSNR=xASL_stat_PSNR(imRef,imSrc)
```

**Description:**

Calculates the PSNR, needs two input arguments - 3D images of the same size.
Uses 95% percentile instead of MAX.




----
### xASL\_stat\_PairwiseDice.m

**Format:**

```matlab
[DiceCoeff] = xASL_stat_PairwiseDice(GroupA, GroupB)
```

**Description:**

This function obtains for two lists of images Dice
coefficients, for all possible permutations of both lists, by the
following steps:
1. Admin (check cell, image exist etc)
2. Obtain matrix of pair-wise permutations
3. Obtain DICE scores

PM: Allow entering one group only
PM: could extend with xASL\_qc\_TanimotoCoeff


----
### xASL\_stat\_PrintStats.m

**Format:**

```matlab
[x] = xASL_stat_PrintStats(x)
```

**Description:**

This function prints an overview of statistics from
data that were acquired per ROI, in a TSV file. It starts by
printing covariates (called "Sets"). Rows will be
subjects/sessions, columns will be the sets and
ROI-statistics.
Any missing data will be skipped (setting them to NaN should
have happened in a previous function).

This function performs the following steps:

1. First remove previous TSV-file, if already existed
printing to a TSV file can be tricky if it is opened by
Excel. Make sure to close previous versions first,
otherwise this part will crash.
2. Print overview of sets to TSV
as explained above. Uses subfunction
xASL\_stat\_CreateLegend to put legends. Aim is to create a
single TSV file that has a proper overview of the data,
& is self-explanatory to those reading/using it.
3. Define number of ASL sessions, force to 1 in case of TT or volume metrics
4. Print the overview



----
### xASL\_stat\_QuantileNan.m

**Format:**

```matlab
y = xASL_stat_QuantileNan(x[,quant, dim])
```

**Description:**

Calculates a quantile, but ignoring NaNs in the calculation


----
### xASL\_stat\_RobustMean.m

**Format:**

```matlab
[NoOutliers, iOutliers, ThresholdDeviation] = xASL_stat_RobustMean(IM, ParameterFunction)
```

**Description:**

This function detects outlier images, that can be used to create
a robust average, e.g. for template or biasfield creation. This is based either on the sum-of-squares
with the mean image (SoS), or on the average relative asymmetry index (AI). Images that are
median+/-3 mad off are defined as outliers. MAD = median/mean absolute difference


----
### xASL\_stat\_ShapiroWilk.m

**Format:**

```matlab
[H, P, W] = xASL_stat_ShapiroWilk(x[, alpha])
```

**Description:**

Performs the statistical test of normality - null hypothesis is that the sample is from normal
distribution with unspecified mean and variance. Based on the sample kurtosis it performs either
Shapiro-Wilk (for platykurtic) or Shapiro-Francia test (for leptokurtic).

----
### xASL\_stat\_StdNan.m

**Format:**

```matlab
y = xASL_stat_StdNan(x[,w,dim])
```

**Description:**

It behaves in a similar way as VAR - it directly passes all arguments to xASL\_stat\_VarNan.


----
### xASL\_stat\_SumNan.m

**Format:**

```matlab
y = xASL_stat_SumNan(x[,dim])
```

**Description:**

It uses the function SUM, but it sets all the NaNs to zero before calling it.


----
### xASL\_stat\_UniquePairwisePermutations.m

**Format:**

```matlab
[PermutationList] = xASL_stat_UniquePairwisePermutations(GroupA, GroupB)
```

**Description:**

This function lists for one or two samples of indices
all possible permutations of indices,
performing the following steps:

1. One sample permutations
2. Two sample permutations
3. Print conclusion

PM: Allow entering one group only
PM: could extend with xASL\_qc\_TanimotoCoeff


----
### xASL\_stat\_VarNan.m

**Format:**

```matlab
y = xASL_stat_VarNan(x[,w,dim])
```

**Description:**

It behaves in a similar way as VAR.


----
### xASL\_stat\_fcdf.m

**Format:**

```matlab
F = xASL_stat_fcdf(F,M,N)
```

**Description:**

Calculates the cumulative distribution function of the F-distribution for degrees of freedom M,N at value F.


----
### xASL\_stat\_tcdf.m

**Format:**

```matlab
F = xASL_stat_tcdf(T,nu)
```

**Description:**

Calculates the cumulative distribution function of the Student's t-distribution for degrees of freedom nu at value T.


----
### xASL\_stat\_ticdf.m

**Format:**

```matlab
T = xASL_stat_ticdf(P,nu)
```

**Description:**

Calculates the inverse of cumulative distribution function of the Student's t-distribution for degrees of freedom nu at value P.


----
### xASL\_stat\_ttest.m

**Format:**

```matlab
[H,P,CI,stats] = xASL_stat_ttest(X[,M,alpha,tail,dim])
```

**Description:**

Performs a t-test that the distribution of the input data X has a mean different from 0 (or from a
given mean M, or that the distributions X and Y have different means). A normal distribution of the data
with an unknown variance is assumed.


----
### xASL\_stat\_ttest2.m

**Format:**

```matlab
[H,P,CI,stats] = xASL_stat_ttest2(X,Y[,alpha,tail,vartype,dim])
```

**Description:**

Performs a unpaired t-test that the distribution of the input data X has a mean different from that of Y.
A normal distribution of the data with an unknown variance is assumed.


## Visualization

----
### xASL\_vis\_AddIM2QC.m

**Format:**

```matlab
[x] = xASL_vis_AddIM2QC(x,parms);
```

**Description:**

Checks which images already are loaded, and  adds new image.



----
### xASL\_vis\_CreateVisualFig.m

**Format:**

```matlab
[ImOut, FileName] = xASL_vis_CreateVisualFig(x, ImIn[, DirOut, IntScale, NamePrefix, ColorMap, bClip, MaskIn, bWhite, MaxWindow, bTransparancy, bVerbose, bContour])
```

**Description:**

This function creates a visualization Figure by merging flexibly rearranging NIfTI slices, input matrix or
path, managing colormaps for different merged image layers. Current use is for visual QC figures and overview in papers.
Function is structured as:

1. Admin, deal with input arguments
2. Process image layers separately
\* xASL\_im\_TransformData2View: Reshapes image data into visualization figure
\* xASL\_im\_ClipExtremes: Clips image to given percentile
also we scale for peak intensity, we make sure that there is no
visible clipping/distortion
\* Convert to colors, using any input colormaps
3. combine image layers, using input argument IntScale
4. print figure

This function assumes that the first image is a grayscale background
image (e.g. for transparancy reasons), if there are multiple
images


----
### xASL\_vis\_CropParmsAcquire.m

**Format:**

```matlab
[xmin xmax ymin ymax] = xASL_vis_CropParmsAcquire(temp_image)
```

**Description:**

Goes from outside to inside to acquire crop settings.
Works with grayscale images (2 dimensions per slice).
Image position information (2D matrix) should be first
2 dimensions. Could include colordimension later on.



----
### xASL\_vis\_CropParmsApply.m

**Format:**

```matlab
ImageOut = xASL_vis_CropParmsApply(ImageIn,CropParameters)
```

**Description:**

This function crops 2D image matrices.




----
### xASL\_vis\_Imwrite.m

**Format:**

```matlab
[ImOut] = xASL_vis_Imwrite(ImIn, PathOut[, ColorMap, bRescale])
```

**Description:**

This functions takes an input image matrix, interpolates it
to HD resolution (1920x1080) for visibility, and saves the image as jpg.
This function avoids the graphic interface of Matlab, for running from CLI
Careful: this function overwrites any existing PathOut.




----
### xASL\_vis\_OverlapT1\_ASL.m

**Format:**

```matlab
xASL_vis_OverlapT1_ASL( x, ASL)
```

**Description:**

Part of ExploreASL.
Shows spatial agreement ASL and probability maps.



----
### xASL\_vis\_TileImages.m

**Format:**

```matlab
[ImOut] = xASL_vis_TileImages(ImIn, nColumns)
```

**Description:**

Merges selected slices (3D) into one single 2D picture.
Plots all slices in one figure with specified rows and
columns, aiming for a square tile.

PM: can be extended to multiple slices



----
### xASL\_vis\_TransformData2View.m

**Format:**

```matlab
FigureOut = xASL_vis_TransformData2View(ImagesIn, x)
```

**Description:**

This function changes the dimensionality and reshapes the input images
in such a way that they are nicely tiled in a mosaic for visualization purposes.
Reshaping a series of images with this function can be useful for
visualization of SPM/voxel-based analyses.


----
### xASL\_vis\_VisualQC\_TopUp.m

**Format:**

```matlab
[MeanAI_PreTopUp_Perc, MeanAI_PostTopUp_Perc] = xASL_vis_VisualQC_TopUp(PathPopB0, PathPopUnwarped, x, iSubject, CheckDir)
```

**Description:**

This function creates a Figure that showes the effect of TopUp
with 6 images with axial slices: the NormPE, RevPE and
their difference image in colorscale, and this before (upper
row) & after (lower row) TopUp.



----
### xASL\_vis\_VisualizeROIs.m

**Format:**

```matlab
xASL_vis_VisualizeROIs(x, ROI_list)
```

**Description:**

Creates for each subject a JPEG image containing
the original T1w, WMH\_SEGM and T1w after lesion-filling.



