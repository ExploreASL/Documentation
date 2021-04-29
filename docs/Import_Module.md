# Submodules of the Import Module

----
### xASL\_imp\_Anonymize.m

**Format:**

```matlab
xASL_imp_Anonymize(imPar)
```

**Description:**

Run defacing.

1. Iterate over list of subjects
2. Get subject labels
3. Process all anatomical files (`xASL\_spm\_deface`)



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
### xASL\_imp\_CatchErrors.m

**Format:**

```matlab
[dcm2niiCatchedErrors] = xASL_imp_CatchErrors(WarningID, WarningMessage, WarningLine, WarningFileName, WarningPath, scan_name, scanpath, destdir, dcm2niiCatchedErrors, imPar, StackIn)
```

**Description:**

Catch reported warnings/errors, print them if verbose, & add them to a structure of warnings/errors to be stored for later QC.



----
### xASL\_imp\_CreateSummaryFile.m

**Format:**

```matlab
xASL_imp_CreateSummaryFile(imPar, numOf, listsIDs, PrintDICOMFields, globalCounts, scanNames, summary_lines, fid_summary)
```

**Description:**

Create summary file.

1. Create summary file
2. Report totals



----
### xASL\_imp\_DCM2NII.m

**Format:**

```matlab
xASL_imp_DCM2NII(imPar, bCopySingleDicoms, bUseDCMTK, bCheckPermissions, bClone2Source,x)
```

**Description:**

Run the dcm2nii part of the import.

1. Initialize defaults of dcm2nii
2. Create the basic folder structure for sourcedata & derivative data
3. Here we try to fix backwards compatibility, but this may break
4. Redirect output to a log file
5. Start with defining the subjects, visits, sessions (i.e. BIDS runs) and scans (i.e. ScanTypes) by listing or typing
6. Sanity check for missing elements
7. Import subject by subject, visit by visit, session by session, scan by scan



----
### xASL\_imp\_DCM2NII\_Initialize.m

**Format:**

```matlab
imPar = xASL_imp_DCM2NII_Initialize(studyPath, imParPath)
```

**Description:**

Initialize DCM2NII.

1. Read study file
2. Specify paths
3. Finalize the directories
4. Specify the tokens
5. Specify the additional details of the conversion



----
### xASL\_imp\_DCM2NII\_Subject.m

**Format:**

```matlab
[imPar, summary_lines, PrintDICOMFields, globalCounts, dcm2niiCatchedErrors, pathDcmDict] = xASL_imp_DCM2NII_Subject(x, imPar, listsIDs, numOf, settings, globalCounts, iSubject, summary_lines, matches, dcm2niiCatchedErrors, pathDcmDict)
```

**Description:**

Run DCM2NII for one individual subject.


----
### xASL\_imp\_NII2BIDS.m

**Format:**

```matlab
xASL_imp_NII2BIDS(imPar, studyPath, studyParPath)
```

**Description:**

Run the NII2BIDS conversion.

1. Load the study parameters + dataset description
2. Create the study description output and verify that all is there
3. Go through all subjects and check all the M0 and ASLs and modify the JSONs



----
### xASL\_imp\_NII2BIDS\_Subject.m

**Format:**

```matlab
xASL_imp_NII2BIDS_Subject(imPar, bidsPar, studyPar, listSubjects, iSubject)
```

**Description:**

Run NII to ASL-BIDS for one individual subject.

1. Initialize
2. Process all the anatomical files
3. Process the perfusion files



