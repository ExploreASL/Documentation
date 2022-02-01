
# Tutorials (Processing)

How to run the pipeline with BIDS data. For import and more options see EXECUTION and IMPORT

We Either provide datapar with the import, or we modify.put it to derivatives
This document describes the ExploreASL internal x-structure that contains processing settings and also sequence parameters. Normally, you do not have to modifies the processing paramaters and the default values are recommended for most studies. Also, you do not have to insert of modify the sequence parameters as these are normally all obtained from the JSON-files for data saved in ASL-BIDS. However, you can change both the processing settings and sequence parameters by providing them in a `dataPar.json` file in the `ROOT\derivatives\ExploreASL\dataPar.json`. Alternatively, you can provide the file at `ROOT\dataPar.json` during the import and it will be automatically copied to the `ROOT\derivatives\ExploreASL\dataPar.json` location. Note that for missing parameters, default will be used. Provided sequence parameters will be ignored if they will be already contained in individual subject's JSON sidecars, and will only be considered when missing for each particular subject.

We present here the most important parameters and examples

Full list is here
Below, we list all useful parameters that can be set by users for both basic and advanced processing. See a full list of parameters in [Developer Tutorial](./../Tutorials-Developer/). Note that the parameters are grouped to several groups by their function - do not forget to add the full hierarchy when creating the `DataPar.json` - see the example at the end of this page.

----
## Atlas Options

The atlases of the **ExploreASL** population module can be defined in the `x.S` sub-structure. If you are interested in the `TotalGM`, `TotalWM`, `DeepWM`, `Hammers`, `HOcort_CONN`, `HOsub_CONN`, and `Mindboggle_OASIS_DKT31_CMA` atlases e.g., you can add the following lines to your `dataPar.json` file.


```json
{
	"x": [{
			"name": "Example",
			"subject_regexp": "Example",
			"S": {"Atlases": ["TotalGM","TotalWM","DeepWM","Hammers","HOcort_CONN","HOsub_CONN","Mindboggle_OASIS_DKT31_CMA"]}
		}]
}
```

Available atlases are:

Free atlases: 

- `TotalGM`: Mask of the entire GM
- `TotalWM`: Mask of the entire WM
- `DeepWM`: Mask of the deep WM
- `WholeBrain`: Mask of the entire brain
- `MNI_Structural`: MNI cortical atlas
- `Tatu_ACA_MCA_PCA`: Original vascular territories by Tatu et al.
- `Tatu_ICA_PCA`: Tatu (only ICA and PCA)
- `Tatu_ICA_L_ICA_R_PCA`: Tatu (L ICA and R PCA)
- `Tatu_ACA_MCA_PCA_Prox_Med_Dist`: Tatu separated to distal/medial/proximal of ACA/MCA/PCA
- `Mindboggle_OASIS_DKT31_CMA`: Mindboggle-101 cortical atlas

Free for non-commercial use only:

- `HOcort_CONN`: Harvard-Oxford cortical atlas
- `HOsub_CONN`: Harvard-Oxford subcortical atlas
- `Hammers`: Alexander Hammers's brain atlas
- `HammersCAT12`: Hammers atlas adapted to DARTEL template of IXI550 space
- `Thalamus`: Harvad-Oxford thalamus atlas

If you want **ExploreASL** to include your favorite atlas in a future version, please get in contact. Only a label NIfTI, a descriptive TSV file, and the corresponding license are required.


### x.modules (Modules)

Parameters for additional **modules** are stored within the main level of `x.modules`.

| Fieldname                            | Description                                   |
| ------------------------------------ |:---------------------------------------------:|
| x.modules.bRunLongReg                | Run longitudinal registration. |
| x.modules.bRunDARTEL                 | Run between-subject registration/create templates. |

