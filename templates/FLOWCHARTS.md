
# Flow Charts

Check out [mermaid](https://github.com/mermaid-js/mermaid) and its [live editor](https://mermaid-js.github.io/mermaid-live-editor). Download the browser extensions [here](https://github.com/BackMarket/github-mermaid-extension).

## Import

````mermaid
graph TD
    A[ExploreASL_ImportMaster] --> B{bImportData}
    B --> |false| C(ExploreASL_Master)
    B --> |true| D{ImportModules}
    D --> |run DCM2NII| E(xASL_module_Import)
    D --> |run NII2BIDS| E(xASL_module_Import)
    D --> |run DEFACE| E(xASL_module_Import)
    D --> |run BIDS2LEGACY| F(xASL_imp_BIDS2Legacy)
    F --> G(xASL_bids_BIDS2Legacy)
    G --> A
    E --> H(xASL_init_SubStructs)
    H --> I(xASL_imp_DCM2NII_Initialize)
    I --> J{bRunSubmodules}
    J --> |run DCM2NII| K(xASL_imp_DCM2NII)
    J --> |run NII2BIDS| L(xASL_imp_NII2BIDS)
    J --> |run DEFACE| M(xASL_imp_Deface)
    L --> N(xASL_imp_Import_UpdateDatasetRoot)
    K --> A
    N --> A
    M --> A
```




