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
### xASL\_imp\_BIDS2Legacy.m

**Format:**

```matlab
[x] = xASL_imp_BIDS2Legacy(x);
```

**Description:**

BIDS to Legacy conversion script which calls xASL\_bids\_BIDS2Legacy.

1. Input check
2. Start with checking dataset\_description.json & rawdata
- 1. The input is dataset\_description.json in the rawdata folder
- 2. The input is dataPar.json or sourceStructure.json - have to look for a rawdata folder
3. Run the legacy conversion: Check if a dataPar is provided, otherwise use the defaults
4. Overwrite DatasetRoot



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
xASL_imp_CreateSummaryFile(imPar, PrintDICOMFields, x, fid_summary)
```

**Description:**

Create summary file.

1. Create summary file
2. Report totals



----
### xASL\_imp\_DCM2NII.m

**Format:**

```matlab
xASL_imp_DCM2NII(imPar, x)
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
[imPar, summary_lines, PrintDICOMFields, globalCounts, scanNames, dcm2niiCatchedErrors, pathDcmDict] = xASL_imp_DCM2NII_Subject(x, imPar, iSubject, matches, dcm2niiCatchedErrors)
```

**Description:**

Run DCM2NII for one individual subject.

1. Run DCM2NII for one individual subject
2. Iterate over visits
3. Loop through all sessions
4. Iterate over scans
- 1. Initialize variables (scanID, summary\_line, first\_match)
- 2. Convert scan ID to a suitable name and set scan-specific parameters
- 3. Minimalistic feedback of where we are
- 4. Now pick the matching one from the folder list
- 5. Determine input and output paths
- 6. Start the conversion if this scan should not be skipped
- 7. Store JSON files
- 8. In case of a single NII ASL file loaded from PAR/REC, we need to shuffle the dynamics from CCCC...LLLL order to CLCLCLCL... order
- 9. Make a copy of analysisdir in sourcedir
- 10. Store the summary info so it can be sorted and printed below



----
### xASL\_imp\_DCM2NII\_Subject\_CopyTempDir.m

**Format:**

```matlab
xASL_imp_DCM2NII_Subject_CopyTempDir(nii_files, bClone2Source)
```

**Description:**

Make a copy of the temp directory in the source directory.

1. Iterate over the NIfTI files
2. Copy the NIfTIs
3. Copy the JSONs



----
### xASL\_imp\_DCM2NII\_Subject\_ShuffleTheDynamics.m

**Format:**

```matlab
[nii_files, summary_line, globalCounts, ASLContext] = xASL_imp_DCM2NII_Subject_ShuffleTheDynamics(globalCounts, scanpath, scan_name, nii_files, iSubject, iSession, iScan)
```

**Description:**

Shuffle the dynamics.

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
[imPar, globalCounts, x, summary_line, destdir, scanpath, scan_name, dcm2niiCatchedErrors, nii_files, first_match] = xASL_imp_DCM2NII_Subject_StartConversion(imPar, globalCounts, x, bSkipThisOne, summary_line, destdir, scanpath, scan_name, dcm2niiCatchedErrors)
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
### xASL\_imp\_Import\_UpdateDatasetRoot.m

**Format:**

```matlab
[x] = xASL_imp_Import_UpdateDatasetRoot(x, studyPath)
```

**Description:**

Update x.opts.DatasetRoot to dataset\_description.json after NII2BIDS conversion

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
3. Process the perfusion files (iterate over sessions)
- 1. Make a subject directory
- 2. Iterate over runs



----
### xASL\_imp\_NII2BIDS\_SubjectSession.m

**Format:**

```matlab
[imPar, bidsPar, studyPar, iSubject, fSes, listSubjects, subjectLabel] = xASL_imp_NII2BIDS_SubjectSession(imPar, bidsPar, studyPar, iSubject, fSes, listSubjects, subjectLabel, kk)
```

**Description:**

NII2BIDS conversion for a single sessions.



----
### xASL\_imp\_NII2BIDS\_SubjectSessionRun.m

**Format:**

```matlab
[imPar, bidsPar, studyPar, subjectLabel, sessionLabel, listSubjects, fSes, inSessionPath, outSessionPath, nSes, iSubject] = xASL_imp_NII2BIDS_SubjectSessionRun(imPar, bidsPar, studyPar, subjectLabel, sessionLabel, listSubjects, fSes, inSessionPath, outSessionPath, nSes, iSubject, kk, mm)
```

**Description:**

NII2BIDS conversion for a single sessions, single run.



----
### xASL\_imp\_NII2BIDS\_Subject\_DefineM0Type.m

**Format:**

```matlab
[studyPar, bidsPar, jsonLocal, inSessionPath, subjectLabel, sessionLabel, bJsonLocalM0isFile] = xASL_imp_NII2BIDS_Subject_DefineM0Type(studyPar, bidsPar, jsonLocal, inSessionPath, subjectLabel, sessionLabel)
```

**Description:**

Define M0 Type.



