
# Import Module

A separate import module has been created for two reasons:

1) to perform a DICOM to NIFTI conversion and store ancillary DICOM header information in *.mat files and

2) to restructure an existing arbitrary scan directory structure into a structure that ExploreASL is familiar with.

This import module uses regular expressions and the directory structure can be can be flexibly defined as long as it is fixed per study. The search function then searches across these directories and copies all files to the ExploreASL directory structure, and performs the conversion to NIFTI if necessary.

For the import of the data, the best is to execute

*ExploreASL_Master('/home/username/Path_for_the_root_folder',1,0)*

where 1 here is the whole Import module, which is divided in 4 parts [ 1 1 1 1] (add link to ExploreASL BIDS tutorials)

The 2D_Sleep study data example has an original directory structure as follows: ROOT\sourcedata\subject\session\scan_ID, and the directories containing the DICOMs. See image below:

![alt text](\\amc.intra\users\B\bestevespadrela\home\My Pictures\2D_Sleep.PNG)

Explore ASL creates the following directory structure ROOT\analysis\subject\ and puts structural images directly and ASL images to directories ASL_1, ASL_2 depending on the session - see below:

![alt text](\\amc.intra\users\B\bestevespadrela\home\My Pictures\2D_Sleep_ExploreASL.PNG)

## sourcestructure.json 

The sourcestructure.json file contains the following code (different for each study):


     {  "folderHierarchy" = [^(\d{3}).*', '^Session([12])$','^(PSEUDO_10_min|T1-weighted|M0)$'],

       "tokenOrdering" = [ 1 2 3],

       "tokenSessionAliases" = [ '^1$', 'ASL_1'; '^2$', 'ASL_2' ],

       "tokenScanAliases" = [ '^PSEUDO_10_min$', 'ASL4D'; '^M0$', 'M0'; '^T1-weighted$', 'T1' ],
       
       "bMatchDirectories" = true; }


### 1. folderHierarchy = { '^(\d{3}).*', '^Session([12])$','^(PSEUDO_10_min|T1-weighted|M0)$'}


This specifies the names of all directories at all levels. In this case, we have specified regular expressions for directories at three different levels:

* ^(\d{3}).*
* ^Session([12])$
* ^(PSEUDO_10_min|T1-weighted|M0)$

This means that we will identify three directory levels, each with a certain name, following regular expressions (see more here https://de.mathworks.com/help/matlab/ref/regexp.html).

At each directory level, it first decides if the directory matches the regular expression, then it also decides if it extract tokens from the string or not - typically tokens are in brackets. More below.

* Extracting a token: ^(\d{3}).*
* Not extracting a token: ^\d{3}.*
Tokens information extracted from the directory name at each directory level. The tokens are numbered by the directory level and can be later used to label patients, sequences etc. See *tokenOrdering* below.

We use normal characters and metacharacters. Expressions from characters and metacharacters are then further coupled with quantifiers, grouping operators, and conditional operators. Some basic regular expressions are described here with examples - for a full description see the link above:

##### Metacharacters:

* . -- any character -- '..ain' == 'drain' == 'train' != 'pain' != 'rain'
* [c1c2] -- any character within the brackets -- '[rp.]ain' == 'rain' == 'pain' == '.ain' != 'wain'
* \d -- any numeric digit, equivalent to [0-9]

##### Quantifiers (applied on an expression)
* expr* -- repeat 0+ times -- '.*' can give anything
* expr? -- repeat 0 or 1 times - '.?' == '' == 'p' != 'pp'
* expr+ -- repeat 1+ times
* expr{n} -- repeat n times consecutively

##### Grouping operators (allows to capture tokens)
* expr -- group elements and capture tokens as a part of the text -- '(.)ain' matches * * 'rain' or 'pain' and returns 'r' or 'p' as a token
* (?:expr) -- group elements but do not capture tokens

##### Anchors
* ^expr -- beginning of a text -- '^M.*' is any string starting with M
* expr$ -- end of the text -- '.*M$' is any string ending with M

##### Conditional operators
* expr1|expr2 -- matches expression 1 or 2

#### Some examples of strings:

* ^(\d{3}).* -- a string starting with 3 digits and any ending, the digits are extracted as tokens.

*001a, 212abasd, 231absd*

* ^(P\d{2})$ -- a string starting with P and ending with two digits, the whole string is taken as a token.

*P12, P32*

* .*(T1_MPR|pcasl|M0).*\.PAR$ -- a string with any beginning, containing T1_MPR or pcasl or M0 (this is taken as a token), and ending by .PAR.

*test_T1_MPR_testtest.PAR, another_pcasl.PAR, M0_test_.PAR*

* .*(T1|ASL).*(PCASL|PASL) -- extracts string containing T1 or ASL and PCASL and PASL. Two tokens are extracted. In the above example, the first level is subject, which has three digits (e.g. 101 or 102), specified by \d{3} as regular expression. This is between brackets ( ) to define that this is the first token.


### 2. tokenOrdering = [ 1 2 3];

Tokens (parts of the directory names) were extracted according to the regular expressions above. Here we decide how the tokens are used.

This is specified by tokenOrdering = [patientName, SessionName, ScanName]

* tokenOrdering = [ 1 2 3]; = first token is used for patient name, second for session name, third for scan name
* tokenOrdering = [ 1 0 2]; = first token is used for patient name, second for scan name, session name is not assigned
* tokenOrdering = [ 1 3 2]; = first token is used for patient name, third for session name, second for scan name
* tokenOrdering = [ 2 1 3]; = second token is used for patient name, first for session name, third for scan name

### 3. tokenSessionAliases = { '^1$', 'ASL_1'; '^2$', 'ASL_2' };

The second token defines the name of the session. The token was extracted using a regular expression *^Session([12])$*. This can represent a string *Session1* or *Session2*. And either *1* or *2* is taken as the token.

In the session-aliases, each row represents one session name. The first column is the regular expression for the token, the second column gives the final name.

^1$ ASL_1

^2$ ASL_2

Here, the token name *^1$* - that is a string equaling to *"1"* is replaced in the analysis folder by the session name *ASL_1*.


### 4. tokenScanAliases = { '^PSEUDO_10_min$', 'ASL4D'; '^M0$', 'M0'; '^T1-weighted$', 'T1' }

The third token defines the name of the scan. Each row represents one scan name. The first column is the regular expression for the token, the second column gives the final name of the scan.

^PSEUDO_10_min$ ASL4D

^M0$ M0

^T1-weighted$ T1

The DICOMs in the directory *PSEUDO_10_min* are thus extracted to a NIFTI file called *ASL4D.nii*.

Note: The names ASL4D, M0, T1, FLAIR and WMH_SEGM are fixed names in the pipeline data structure. ASL4D and T1 are required, whereas M0, FLAIR and WMH_SEGM are optionally. When M0 is available, the pipeline will divide the perfusion-weighted image by the M0 image in the quantification part. When FLAIR and WMH_SEGM are available, tissue misclassification in the T1 gray matter (GM) and white matter (WM) because of white matter hyperintensities (WMH) will be corrected.

### 5. bMatchDirectories = true;

Set to *TRUE* if it should look for directories and DICOMs inside. Set to *FALSE* when you will




.



.


### Summary of the 2D_Sleep example

In the current example of 2D_Sleep, we have two subjects: 101 and 102 with each two ASL sessions. In ExploreASL, each subject has a single T1 scan and can have multiple ASL sessions. This is the case when the anatomy is not expected to change for the different ASL sessions, e.g. when scans are repeated before and after a CO2 or treatment challenge. The data structure will be *derivatives\ExploreASL\SubjectName\T1.nii* for the anatomical scans (T1 or FLAIR) and *derivatives\ExploreASL\SubjectName\ASL_1\ASL4D.nii* and *derivatives\ExploreASL\SubjectName\ASL_2\ASL4D.nii* for ASL sessions. 

If it concerns a follow-up study, where scan sessions have months or years in between them and brain anatomy is expected to change significantly between the scan sessions, then the data should be set up as a longitudinal study design. In this case, different time points are set up as *derivatives\ExploreASL\SubjectName_1\T1.nii* and *derivatives\ExploreASL\SubjectName_2\T1.nii* for two longitudinal scans of the same subject, where _1 designates the time point. So a longitudinal study with two ASL scans per scan session (e.g. a medication challenge is repeated with 6 months time difference) would look like:

*derivatives\ExploreASL\SubjectName_1\T1.nii*, *derivatives\ExploreASL\SubjectName_1\ASL_1\ASL4D.nii* & *derivatives\ExploreASL\SubjectName_1\ASL_2\ASL4D.nii* for the first time point and

*derivatives\ExploreASL\SubjectName_2\T1.nii*, *derivatives\ExploreASL\SubjectName_1\ASL_2\ASL4D.nii* & *derivatives\ExploreASL\SubjectName_2\ASL_2\ASL4D.nii* for the second time point.


