# Submodules of the Import Module

----
### xASL\_imp\_ANONYMIZE.m

#### Format

```matlab
xASL_imp_ANONYMIZE(imPar)
```

#### Description
Run defacing.


----
### xASL\_imp\_AppendNiftiParameters.m

#### Format

```matlab
s = xASL_imp_AppendNiftiParameters(nii_files)
```

#### Description
Append Nifti Parameters.


----
### xASL\_imp\_AppendParmsParameters.m

#### Format

```matlab
[s, FieldNames] = xASL_imp_AppendParmsParameters(parms)
```

#### Description
Append Parms Parameters.


----
### xASL\_imp\_CatchErrors.m

#### Format

```matlab
[dcm2niiCatchedErrors] = xASL_imp_CatchErrors(WarningID, WarningMessage, WarningLine, WarningFileName, WarningPath, scan_name, scanpath, destdir, dcm2niiCatchedErrors, imPar, StackIn)
```

#### Description
Catch reported warnings/errors, print them if verbose, & add them to a structure of warnings/errors to be stored for later QC.


----
### xASL\_imp\_CreateSummaryFile.m

#### Format

```matlab
xASL_imp_CreateSummaryFile(imPar, numOf, listsIDs, PrintDICOMFields, globalCounts, scanNames, summary_lines, fid_summary)
```

#### Description
Create summary file.


----
### xASL\_imp\_DCM2NII.m

#### Format

```matlab
xASL_imp_DCM2NII(imPar, bCopySingleDicoms, bUseDCMTK, bCheckPermissions, bClone2Source,x)
```

#### Description
Run the dcm2nii part of the import.


----
### xASL\_imp\_DCM2NII\_Initialize.m

#### Format

```matlab
imPar = xASL_imp_DCM2NII_Initialize(studyPath, imParPath)
```

#### Description
Initialize DCM2NII.


----
### xASL\_imp\_DCM2NII\_Subject.m

#### Format

```matlab
[imPar, summary_lines, PrintDICOMFields, globalCounts, dcm2niiCatchedErrors, pathDcmDict] = xASL_imp_DCM2NII_Subject(x, imPar, listsIDs, numOf, settings, globalCounts, iSubject, summary_lines, matches, dcm2niiCatchedErrors, pathDcmDict)
```

#### Description
Run DCM2NII for one individual subject.


----
### xASL\_imp\_NII2BIDS.m

#### Format

```matlab
xASL_imp_NII2BIDS(imPar, studyPath, studyParPath)
```

#### Description
Run the NII2BIDS conversion.


----
### xASL\_imp\_NII2BIDS\_Subject.m

#### Format

```matlab
xASL_imp_NII2BIDS_Subject(imPar, bidsPar, studyPar, listSubjects, iSubject)
```

#### Description
Run NII to ASL-BIDS for one individual subject.


