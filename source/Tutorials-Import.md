----
Main function of the import module is to convert input DICOM data to the BIDS format in `rawdata` subfolder. Sequence parameters missing in the DICOMs are supplied in the `studyPar.json` file.

## How to run the import module

A separate import module has been created to:

1. to perform a DICOM to NIfTI conversion and store ancillary DICOM header information in `*.json` files,
2. to further processed the NIfTI files and create full ASL-BIDS data from it,
3. to deface the structural images in the ASL-BIDS data,
4. to restructure ASL-BIDS data to an internal structure that ExploreASL is familiar with and to further process these data.

This import module uses regular expressions and the directory structure can be can be flexibly defined as long as it is fixed per study.
The search function then searches across these directories and copies all files to the ExploreASL directory structure, and performs the conversion to NIFTI if necessary.

For the import of the data, the best is to execute...

```matlab
ExploreASL('/home/username/Path_for_the_root_folder', 1, 0);
```

...where 1 here is the whole Import module, which is divided in 3 parts `[1 1 1]` - more details are provided in the [Tutorial on ExploreASL Execution](./../Tutorials-Execution/).

## Example dataset for Import with ExploreASL

The 2D_Sleep study data example has an original directory structure as follows: `ROOT/sourcedata/subject/session/scan_ID`, and the directories containing the DICOMs. See image below:

```
2D_Sleep/
  sourcedata/
    101/
      Session1/
        M0/
        PSEUDO_10_min/
        T1_weighted/
      Session2/
        ...
    102/
      ...
```

Explore ASL first write the temporary NIfTI files to `ROOT/derivatives/ExploreASL/temp/subject/`.

In the second step writes the full ASL-BIDS data into `ROOT/rawdata/`.

And at the end, Explore ASL creates the following directory structure `ROOT/derivatives/ExploreASL/subject/` and puts structural images directly and ASL images to directories `ASL_1`, `ASL_2` depending on the session - see below:

```
2D_Sleep/
  derivatives/
    ExploreASL/
      101/
        ASL_1/
        ASL_2/
      102/
        ...
```

Ideally, before running the import, the data contains three configuration files
`ROOT/sourcestructure.json`
`ROOT/studyPar.json`
`ROOT/dataPar.json`

The content of the `sourcestructure.json` and `studypar.json` files is explained below. The content of `dataPar.json` is explained in the [Processing Tutorial](./../Tutorials-Processing/).

----
### sourcestructure.json 

The `sourcestructure.json` file contains the following code (different for each study):

```json
{  
       "folderHierarchy": ["^(\\d{3}).*", "^Session([12])$","^(PSEUDO_10_min|T1-weighted|M0)$"],
       "tokenOrdering": [ 1 0 2 3],
       "tokenSessionAliases": ["^1$", "ASL_1", "^2$", "ASL_2"],
       "tokenVisitAliases": ["", ""],
       "tokenScanAliases": [ "^PSEUDO_10_min$", "ASL4D", "^M0$", "M0", "^T1-weighted$", "T1w"],
       "bMatchDirectories": true,
       "dcm2nii_version":"20220720",
}
```

Note that we are using an outdated notation here - Visit/Session/Scan. In BIDS, this corresponds to Sessions/Runs/Scans - this will be updated in the future.

#### 1. folderHierarchy

```
"folderHierarchy": ["^(\\d{3}).*", "^Session([12])$","^(PSEUDO_10_min|T1-weighted|M0)$"]
```

This specifies the names of all directories at all levels. In this case, we have specified regular expressions for directories at three different levels:

* `^(\\d{3}).*`
* `^Session([12])$`
* `^(PSEUDO_10_min|T1-weighted|M0)$`

This means that we will identify three directory levels, each with a certain name, following regular expressions (find more in the [matlab definition](https://de.mathworks.com/help/matlab/ref/regexp.html) of regular expressions). Note that `\\` is used in the JSON to be correctly read and parsed as a single `\` for the regular expression.

At each directory level, it first decides if the directory matches the regular expression, then it also decides if it extract tokens from the string or not - typically tokens are in brackets. More below.

* Extracting a token: `^(\\d{3}).*`
* Not extracting a token: `^\\d{3}.*`

Tokens information extracted from the directory name at each directory level. 
The tokens are numbered by the directory level and can be later used to label patients, sequences etc. See **tokenOrdering** below.

We use normal characters and metacharacters.
Expressions from characters and metacharacters are then further coupled with quantifiers, grouping operators, and conditional operators.
Some basic regular expressions are described here with examples - for a full description see the link above:

**Metacharacters:**

* `.` -- any character -- `'..ain'` == `'drain'` == `'train'` != `'pain'` != `'rain'`
* `[c1c2]` -- any character within the brackets -- `'[rp.]ain'` == `'rain'` == `'pain'` == `'.ain'` != `'wain'`
* `\d` -- any numeric digit, equivalent to `[0-9]`

**Quantifiers (applied on an expression):**

* `expr*` -- repeat 0+ times -- `'.*'` can give anything
* `expr?` -- repeat 0 or 1 times - `'.?'` == `''` == `'p'` != `'pp'`
* `expr+` -- repeat 1+ times
* `expr{n}` -- repeat n times consecutively

**Grouping operators (allows to capture tokens):**

* `expr` -- group elements and capture tokens as a part of the text -- `'(.)ain'` matches * * `'rain'` or `'pain'` and returns `'r'` or `'p'` as a token
* `(?:expr)` -- group elements but do not capture tokens

**Anchors:**

* `^expr` -- beginning of a text -- `'^M.*'` is any string starting with M
* `expr$` -- end of the text -- `'.*M$'` is any string ending with M

**Conditional operators:**

* `expr1|expr2` -- matches expression 1 or 2

**Some examples of strings:**

* `^(\d{3}).*` -- a string starting with 3 digits and any ending, the digits are extracted as tokens. `001a, 212abasd, 231absd`.

* `^(P\d{2})$` -- a string starting with P and ending with two digits, the whole string is taken as a token. `P12, P32`.

* `.*(T1_MPR|pcasl|M0).*\.PAR$` -- a string with any beginning, containing T1_MPR or pcasl or M0 (this is taken as a token), and ending by `.PAR`. `test_T1_MPR_testtest.PAR, another_pcasl.PAR, M0_test_.PAR`.

* `.*(T1|ASL).*(PCASL|PASL)` -- extracts string containing T1 or ASL and PCASL and PASL. Two tokens are extracted. In the above example, the first level is subject, which has three digits (e.g. `101` or `102`), specified by `\d{3}` as regular expression. This is between brackets `( )` to define that this is the first token.

Again, note that the actual strings are given here and all `\` have to be escaped when written to a json-file as `\\`.

#### 2. tokenOrdering

```
"tokenOrdering": [ 1 0 2 3],
```

Tokens (parts of the directory names) were extracted according to the regular expressions above. Here we decide how the tokens are used.

This is specified by "tokenOrdering": `[patientName, SessionName, ScanName]`

* `"tokenOrdering": [1 0 2 3];` = first token is used for patient name, second for session name, third for scan name
* `"tokenOrdering": [1 0 0 2];` = first token is used for patient name, second for scan name, session name is not assigned
* `"tokenOrdering": [1 0 3 2];` = first token is used for patient name, third for session name, second for scan name
* `"tokenOrdering": [2 0 1 3];` = second token is used for patient name, first for session name, third for scan name
* `"tokenOrdering": [2 1 0 3];` = second token is used for patient name, first for visit name, third for scan name

#### 3. tokenVisitAliases
If multiple visits are present - use the following notation to mark them:

```
"tokenVisitAliases":["session_1","1","session_2","2","session_3","3","session_4","4","session_5","5","session_6","6","session_7","7"],`
```

#### 4. tokenSessionAliases

```
"tokenSessionAliases": ["^1$", "ASL_1", "^2$", "ASL_2"],
```

The second token defines the name of the session. The token was extracted using a regular expression `*^Session([12])$*`. This can represent a string `Session1` or `Session2`. And either `1` or `2` is taken as the token.

In the session-aliases, each row represents one session name. The first column is the regular expression for the token, the second column gives the final name.

* `^1$ ASL_1`

* `^2$ ASL_2`

Here, the token name `^1$` - that is a string equaling to `"1"` is replaced in the analysis folder by the session name `ASL_1`.


#### 5. tokenScanAliases

```
"tokenScanAliases": [ "^PSEUDO_10_min$", "ASL4D", "^M0$", "M0", "^T1-weighted$", "T1w"],
```

The third token defines the name of the scan. Each row represents one scan name. The first column is the regular expression for the token, the second column gives the final name of the scan.

* `^PSEUDO_10_min$ ASL4D`

* `^M0$ M0`

* `^T1-weighted$ T1w`

The DICOMs in the directory `PSEUDO_10_min` are thus extracted to a NIFTI file called `ASL4D.nii`.

Note: The names ASL4D, M0, T1, FLAIR and WMH_SEGM are fixed names in the pipeline data structure. ASL4D and T1 are required, whereas M0, FLAIR and WMH_SEGM are optionally. When M0 is available, the pipeline will divide the perfusion-weighted image by the M0 image in the quantification part. When FLAIR and WMH_SEGM are available, tissue misclassification in the T1 gray matter (GM) and white matter (WM) because of white matter hyperintensities (WMH) will be corrected.

#### 6. bMatchDirectories

Set to `true` if it should look for directories and DICOM files inside them. Set to `false` when directly separate files are identified by the folder-hierarchy string.

#### 7. dcm2nii_version

This optional parameter can be used to perform import with an older version of dcm2nii rather than the latest default `20220720`. This is exceptionally needed when the new dcm2nii version does not work well with the data. An alternative is `20190902`. 

----
### Summary of the 2D_Sleep example

In the current example of 2D_Sleep, we have two subjects: 101 and 102 with each two ASL sessions.
In ExploreASL, each subject has a single T1 scan and can have multiple ASL sessions.
This is the case when the anatomy is not expected to change for the different ASL sessions, e.g. when scans are repeated before and after a CO2 or treatment challenge.
The data structure will be `derivatives\ExploreASL\SubjectName\T1.nii` for the anatomical scans (T1 or FLAIR) and `derivatives\ExploreASL\SubjectName\ASL_1\ASL4D.nii` and `derivatives\ExploreASL\SubjectName\ASL_2\ASL4D.nii` for ASL sessions. 

If it concerns a follow-up study, where scan sessions have months or years in between them and brain anatomy is expected to change significantly between the scan sessions, then the data should be set up as a longitudinal study design.
In this case, different time points are set up as `derivatives\ExploreASL\SubjectName_1\T1.nii` and `derivatives\ExploreASL\SubjectName_2\T1.nii` for two longitudinal scans of the same subject, where `_1` designates the time point.
So a longitudinal study with two ASL scans per scan session (e.g. a medication challenge is repeated with 6 months time difference) would look like:

* `derivatives\ExploreASL\SubjectName_1\T1.nii`, `derivatives\ExploreASL\SubjectName_1\ASL_1\ASL4D.nii` & `derivatives\ExploreASL\SubjectName_1\ASL_2\ASL4D.nii` for the first time point and

* `derivatives\ExploreASL\SubjectName_2\T1.nii`, `derivatives\ExploreASL\SubjectName_1\ASL_2\ASL4D.nii` & `derivatives\ExploreASL\SubjectName_2\ASL_2\ASL4D.nii` for the second time point.

----
### studyPar.json 

The `studyPar.json` file contains information about the data that is supposed to be used in the import and filled into the ASL-BIDS sidecar. It should then contain vital parameters of the sequences that are not available inside the DICOM files on the import. This refers to both modality [agnostic BIDS parameters](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html) describing the dataset in general, an [modality specific MRI BIDS parameters](https://bids-specification.readthedocs.io/en/stable/04-modality-specific-files/01-magnetic-resonance-imaging-data.html) describing mostly the ASL data.
The are both saved in an unstructured JSON file using the original BIDS notation for both tags and their values. See an example below:

```json
{"Authors":["Alzheimers Disease Neuroimaging Initiative"],
"DatasetType":"raw",
"License":"http://adni.loni.usc.edu/terms-of-use/",
"Acknowledgements":"http://adni.loni.usc.edu/data-samples/access-data/groups-acknowledgements-journal-format/",
"HowToAcknowledge":"ADNI website: http://adni.loni.usc.edu/",
"Funding":["Alzheimers Disease Neuroimaging Initiative"],
"ReferencesAndLinks":["http://adni.loni.usc.edu/"],
"DatasetDOI":"http://adni.loni.usc.edu/",
"VascularCrushing":false,
"LabelingType":"PCASL",
"BackgroundSuppression":true,
"M0":true,
"ASLContext":"m0scan,deltam",
"Manufacturer":"GE",
"ArterialSpinLabelingType":"PCASL",
"PostLabelingDelay":2.025,
"LabelingDuration":1.45,
"BackgroundSuppressionNumberPulses":4,
"BackgroundSuppressionPulseTime":[1.465,2.1,2.6,2.88]}
```

Note that many of these parameters do not have to be provided (describing the dataset in general). And many of them can be automatically extracted from DICOMs. ExploreASL during import reports which are the missing REQUIRED and RECOMMENDED parameters to help with that. It also checks for the basic logic and validity of these parameters and also reports if provided values differ from the retrieved DICOM values. Note that several fields (PLD, labelingDuration etc.) can be provided both as vectors and scalars and in ASL-BIDS should either be a scalar or the vector dimension should match to the volume number. ExploreASL automatically corrects for this. E.g. for a multi-PLD sequence with 3 different PLDs and 9 volumes, you can provide only

```
"PostLabelingDelay":[1.0, 1.5, 2.0]
```

in the `studyPar.json` and ExploreASL automatically expands it during the import:

```
"PostLabelingDelay":[1.0, 1.5, 2.0,1.0, 1.5, 2.0,1.0, 1.5, 2.0]
```

Also, the field ASLContext should be provided in a .TSV file in ASL-BIDS. ExploreASL import allows to enter it as a field to `studyPar.json` and the automatically writes it into the .tsv file upon the import to ASL-BIDS - also expanding and repeating to the correct number of repetitions. Typically, you would use the following entry:

```
"ASLContext":"control, label",
```

For a multi-PLD sequence with 3 different PLDs and 7 volumes, including an M0 as the first volume, one should provide the 'PLD' and 'LD' for the M0 by setting them to 0 and matching all volumes with their PLD and LD:

```
"PostLabelingDelay":[0, 1.0, 1.0, 1.5, 1.5, 2.0, 2.0],
"LabelingDuration":[0, 1.45, 1.45, 1.45, 1.45, 1.45, 1.45]
```

----
### studyPar.json for multi-sequence study

The `studyPar.json` can be modified so that you can import different sequences with different parameters in a single run. Essentially, studyPar.json structure contains a list of contexts according to the standard definition, each context has a keyword that defines to what kind of data it should be applied. When a dataset fits to several contexts, the results of each context are taken into account and overwritten with the first context having the lowest priority.

The following applies
1. The list of context is in a field called `StudyPars`.
2. The list is processed top to bottom and the matching fields are always overwritten.
3. An extra added keywords `SubjectRegExp`, `VisitRegExp`, or `SessionRegExp` that define strings Subject\Session\Run that sets required conditions to be fulfilled for each context.
4. Missing any of tohse strings or an empty string means that the condition is fulfilled. The conditions are written in regexp format.

```json
{"StudyPars":[
{"DatasetType":"raw",
"ArterialSpinLabelingType":"PCASL",
"PostLabelingDelay": [2],
"BackgroundSuppression":true,
}
{"SubjectRegExp":"",
"SessionRegExp":"3",
"BackgroundSuppression":false,
}
{"SubjectRegExp":"alpha.*|beta.*",
"VisitRegExp":"1",
"PostLabelingDelay": [3],
}
{"VisitRegExp":"2",
"SessionRegExp":"",
"PostLabelingDelay": [4],
}]}
```
The following cases will obtain the following parameters:
1. Subject gamma, session 1, run 1: Fits only to the first context. PostLabelingDelay 2, BSup true.
2. Subject gamma, session 1, run 3: Fits to the first and third context. PostLabelingDelay 2, BSup false.
3. Subject alpha1, session 1, run '': First to the first, second, and third context. PostLabeling delay 3, BSup false.
