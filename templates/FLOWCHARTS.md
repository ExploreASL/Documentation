
# Flow Charts

Check out [mermaid](https://github.com/mermaid-js/mermaid) and its [live editor](https://mermaid-js.github.io/mermaid-live-editor). Download the browser extensions [here](https://github.com/BackMarket/github-mermaid-extension).

## Import

```mermaid
graph TD
    A[ExploreASL_ImportMaster] --> B{bImportData}
    B --> |false| C(ExploreASL_Master)
    B --> |true| E(xASL_module_Import)
    E --> H(xASL_init_SubStructs)
    H --> I(xASL_imp_Initialize)
    I --> J{run DCM2NII}
    J --> |true| K(xASL_imp_DCM2NII)
    J --> |false| L
    K --> L{run NII2BIDS}
    L --> |true| N(xASL_imp_NII2BIDS)
    L --> |false| P
    N --> O(xASL_imp_UpdateDatasetRoot)
    O --> P{run DEFACE}
    P --> |true| Q(xASL_imp_Deface)
    P --> |false| R
    Q --> R{run BIDS2LEGACY}
    R --> |true| S(xASL_imp_BIDS2Legacy)
    R --> |false| A
    S --> A
```

### DCM2NII

```mermaid
graph TD
    A[xASL_imp_DCM2NII] -->|iSubject| B{iSubject<=nSubjects}
    B --> |true| C(xASL_imp_DCM2NII_Subject)
    B --> |false| D(xASL_imp_CreateSummaryFile)
    D --> E(xASL_module_Import)
    C --> |iVisit,iSession,iScan| F{iVisit<=nVisits, iSession<=nSessions, iScan<=nScans}
    F --> |true| G(xASL_imp_DCM2NII_Subject_StartConversion)
    G --> H(xASL_imp_DCM2NII_Subject_StoreJSON)
    H --> I(xASL_imp_DCM2NII_Subject_ShuffleTheDynamics)
    I --> J(xASL_imp_AppendParmsParameters)
    F --> |false| B
    J --> F
```

### NII2BIDS

```mermaid
graph TD
    A[xASL_imp_NII2BIDS] --> B(xASL_bids_Config)
    B --> C(xASL_io_ReadDataPar)
    C --> D(xASL_bids_CreateDatasetDescriptionTemplate)
    D --> |iSubjectSession| E{iSubjectSession <= num of SubjectsSessions}
    E --> |false| G(xASL_Copy log files)
    G --> |false| H(xASL_delete temp folder)
    H --> I(xASL_module_Import)
    E --> |true| F(xASL_imp_NII2BIDS_Subject)
    F --> J(xASL_imp_CheckForAliasInVisit)
    J --> |iSession| K{iSession <= num of Sessions}
    K --> |true| L(xASL_imp_NII2BIDS_Session)
    K --> |false| E
    L --> |iRun| M{iRun <= num of Runs}
    M --> |true| N(xASL_imp_NII2BIDS_Run)
    N --> O(xASL_imp_NII2BIDS_RunAnat)
    O --> P(xASL_imp_NII2BIDS_RunPerf)
    P --> M
    M --> |false| K
```

### BIDS2LEGACY

```mermaid
graph TD
    A[xASL_imp_BIDS2Legacy] --> B(xASL_bids_BIDS2Legacy)
    B --> C(xASL_bids_Config)
    C --> D(Define Subject)
    D --> |iSubjSess| E{iSubjSess<=num of SubjSessions}
    E --> |false| F(Run xASL_bids_parseM0 if M0 exists)
    F --> G(Create dataPar.json if it does not exist)
    G --> H(Copy participants.tsv if it does exist)
    H --> J(Add GeneratedBy field to legacy sidecars)
    J --> K(Clean-up)
    K --> L(xASL_module_Import)
    E --> M(Define SubjectVisit)
    M --> |SubjectVisit| N(xASL_bids_BIDS2Legacy_ParseModality)
    N --> |iModality| O{iModality<=num of Modalities}
    O --> |false| E
    O --> |true| P(xASL_bids_BIDS2Legacy_ParseScanType)
    P --> |iType,iRun| Q{iModality<=num of Types, iRun<=num of Runs}
    Q --> |true| R(xASL_bids_BIDS2Legacy_CompilePathsForCopying)
    R --> S(xASL_bids_BIDS2Legacy_ManageSidecars)
    S --> T(xASL_bids_BIDS2xASL_CopyFile)
    T --> Q
    Q --> |false| O
```



