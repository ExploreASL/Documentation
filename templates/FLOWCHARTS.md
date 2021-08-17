
# Flow Charts

Check out [mermaid](https://github.com/mermaid-js/mermaid) and its [live editor](https://mermaid-js.github.io/mermaid-live-editor). Download the browser extensions [here](https://github.com/BackMarket/github-mermaid-extension).

## Import

```mermaid
graph TD
    A[ExploreASL_ImportMaster] --> B{bImportData}
    D --> |run DCM2NII| E(xASL_module_Import)
    D --> |run NII2BIDS| E(xASL_module_Import)
    D --> |run DEFACE| E(xASL_module_Import)
    D --> |run BIDS2LEGACY| E(xASL_module_Import)
    E --> H(xASL_init_SubStructs)
    H --> I(xASL_imp_Initialize)
    I --> J{bRunSubmodules}
    J --> |run DCM2NII| K(xASL_imp_DCM2NII)
    J --> |run NII2BIDS| L(xASL_imp_NII2BIDS)
    J --> |run DEFACE| M(xASL_imp_Deface)
    J --> |run BIDS2LEGACY| F(xASL_imp_BIDS2Legacy)
    L --> N(xASL_imp_Import_UpdateDatasetRoot)
    K --> A
    N --> A
    M --> A
    F --> A
    B --> |false| C(ExploreASL_Master)
    B --> |true| D{ImportModules}
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
    C --> |iSubjSess| D{iSubjSess <= num of SubjSessions}
    D --> |false| E(Run xASL_bids_parseM0 if M0 exists)
    E --> F(Create dataPar.json if it does not exist)
    F --> G(Copy participants.tsv if it does exist)
    G --> H(Add GeneratedBy field to legacy sidecars)
    H --> I(Clean-up)
    I --> J(ExploreASL_ImportMaster)
    D --> |true| K(xASL_TrackProgress)
    K --> |iVisit| L{iVisit <= num of Visits}
    L --> |true| M(xASL_adm_CreateDir)
    M --> |iModality| N{iModality <= num of Modalities}
    N --> |true| O(...) %% we should really put this functionality in subfunctions
    N --> |false| L
    L --> |false| D
    O --> N
```



