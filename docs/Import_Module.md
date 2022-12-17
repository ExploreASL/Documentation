# Submodules of the Import Module

----
### xASL\_wrp\_DCM2NII.m

**Format:**

```matlab
x = xASL_wrp_DCM2NII(x)
```

**Description:**

Run the dcm2nii part of the import.

1. Initialize defaults of dcm2nii
2. Create the temp directory for DCM2NII
3. Import visit by visit, session by session, scan by scan for current subject
4. Create summary file
5. Clean-up



----
### xASL\_wrp\_DCM2NII\_Subject.m

**Format:**

```matlab
[x, PrintDICOMFields, dcm2niiCatchedErrors] = xASL_wrp_DCM2NII_Subject(x, matches, dcm2niiCatchedErrors)
```

**Description:**

Run DCM2NII for one individual subject.

1. Run DCM2NII for one individual subject
2. Iterate over visits
3. Loop through all sessions
4. Iterate over scans



----
### xASL\_wrp\_Deface.m

**Format:**

```matlab
xASL_wrp_Deface(x)
```

**Description:**

Run defacing.

1. Iterate over list of subjects
2. Get subject labels
3. Process all anatomical files (`xASL\_spm\_deface`)



----
### xASL\_wrp\_NII2BIDS.m

**Format:**

```matlab
x = xASL_wrp_NII2BIDS(x)
```

**Description:**

Run the NII2BIDS conversion.

1. Load the study parameters + dataset description
2. Create the study description output and verify that all is there
3. Go through all subjects and check all the M0 and ASLs and modify the JSONs



----
### xASL\_wrp\_NII2BIDS\_Subject.m

**Format:**

```matlab
x = xASL_wrp_NII2BIDS_Subject(x, bidsPar, studyParAll, nameSubjectSession)
```

**Description:**

Run NII to ASL-BIDS for one individual subject.

1. Initialize
2. Process the anat & perfusion files
- 1. Make a subject directory
- 2. Iterate over sessions
- 3. Iterate over runs



