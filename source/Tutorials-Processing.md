
# Tutorials (Processing)

The main task of the processing module is to take BIDS input data located in `rawdata` folder, convert them to ExploreASL internal (Legacy) structure in `derivatives` subfolder and run processing with the processing parameters defined in the `dataPar.json` file.

## Executing the processing

Before running the processing, it is important to validate that your data are complete and in proper BIDS, using the BIDS validator, which can be found [HERE](https://bids-standard.github.io/bids-validator/). The BIDS validator will also give you hints if your data or metadata differ between scans, which can occur if a scanner is updated.

To run processing of BIDS data located in `drive/.../datasetRoot/rawdata`, you simply execute this command:

```matlab
[x] = ExploreASL('drive/.../datasetRoot', 0, 1);
```
You can provide additional processing parameters in a file at `drive/.../datasetRoot/dataPar.json`. An example content of the `dataPar.json` file is given below on this page. A full reference manual is provided [here](./../ProcessingParameters).

Further options to import DICOM data are specified in the [Import Tutorial](./../Tutorials-Import), and different modes to execute ExploreASL are described in the [Execution Tutorial](./../Tutorials-Execution). Below, you can find examples how to set-up the processing options.

## Configuring the pipeline using dataPar.json
Here, we present how to setup the most basic things for the ExploreASL processing using the `dataPar.json`. Note that you can combine the examples below and concatenate the content of given examples to a single `dataPar.json` to have all the functionalities at once. Here, we only provide examples and full list of all options to set is given in this reference manual for [Processing Parameters](./../ProcessingParameters).

### Evaluate Population module using different atlases
The atlases used in the **ExploreASL** population module can be defined in the `x.S` sub-structure. If you are interested in the `Total-GM`, `Total-WM`, `DeepWM`, `Hammers`, `HOcort_CONN`, `HOsub_CONN`, and `Mindboggle_OASIS_DKT31_CMA` atlases e.g., you can add the following lines to your `dataPar.json` file. You can specify the type of tissue for the given atlas using `TissueMasking` parameter. In case of an absence of this parameter, default type of `GM` is assumed, unless the atlas name contains `WB` or `WM` substring.

```json
{"x":{
    "S": {"Atlases": ["Total","Total","DeepWM","Hammers","HOcort_CONN","HOsub_CONN","Mindboggle_OASIS_DKT31_CMA"],
             "TissueMasking": ["GM", "WM", "WM", "GM", "GM", "GM", "GM"]}   
}}
```

### Processing multi-PLD and Time-encoded data
ExploreASL currently uses BASIL to quantify multi-PLD, multi-TE and time-encoded data (the rest of the processing is done using ExploreASL, BASIL is only used for the quantification itself). Multi-TE sequences execute directly FABBER from FSL. Note that you need to install FSL on you computer for this. Use the following `dataPar.json` to automatically locate installed FSL and to activate CBF quantification using BASIL:

```json
{"x":{"modules":{"asl":{"bUseExternalQuantification":1, "ExternalQuantificationType":"BASIL"}},
"external":{"bAutomaticallyDetectFSL":true}}}
```

In case that FSL is not detected automatically, or if there's a prefered FSL version, you can directly provide path to the FSL directory like this:

```json
{"x":{"modules":{"asl":{"bUseExternalQuantification":1}},
"external":{"bAutomaticallyDetectFSL":false,
"FSLdir":"/home/XASLuser/fsl/",
"RootFSLdir":"/home/XASLuser/fsl/"}}}
```

Note that Tex estimation of BBB permeability from multi-TE data is supported starting at FSL version 6.0.7.1. 

### Processing ASL data without structural T1w scans
ExploreASL uses the structural T1-weighted scans to obtain information about GM and WM tissues used later for evaluation and thresholding the CBF maps. Presence of T1w scans is thus expected for all ASL scans as T1w scans are anyway normally obtained. If a T1-weighted scan is missing, ExporeASL uses internally the average MNI brain instead, but this option needs to be activated explicitly:
```json
{"x":{"modules":{"asl":{"bUseMNIasDummyStructural":1}}}}
```

### Calculating output values in native space
By default, the regional values in the population module are calculated in the MNI space. Additionally, it is also possible to calculate the values in the native space of each subject. This is then done for all specified atlases that are converted for this purpose to the native space of each subject. If activated, both MNI and native space results are calculated.
```json
{"x":{"modules":{"population":{"bNativeSpaceAnalysis":true}}}}
```

### Evaluate Population module on different parameters
By default **ExploreASL** population module evaluates the regional values of CBF. Additionally, it is possible the automatically evaluate values of other parameters if appropriate parameter maps are/can be calculated such as the CBF (qCBF), arterial transit time (ATT), M0, Tex (Time of Exchange across BBB in BBB-ASL. CBF is done by default, but if specifying an own list of parameters, do not forget to add CBF 

```json
{"x":{
    "S": {"DataTypes": ["qCBF", "ATT", "M0", "Tex"]}
}}
```

### Combine previous settings
You can freely combine all the given examples unless they are obvious conflicts between parameters. The five examples above can be combined into a single `dataPar.json` file that will process multi-PLD file wit BASIL, even in the absence of a T1w scan and will use several atlases in the Population module:
```json
{"x":{
    "S": {"Atlases": ["Total","Total","DeepWM","Hammers","HOcort_CONN","HOsub_CONN","Mindboggle_OASIS_DKT31_CMA"], 
            "TissueMasking": ["GM", "WM", "WM", "GM", "GM", "GM", "GM"]"DataTypes": ["qCBF", "ATT", "M0", "Tex"]},
    "Q":{"bUseBasilQuantification":1},
    "external":{"bAutomaticallyDetectFSL":true},
    "modules":{"asl":{"bUseMNIasDummyStructural":1},"population":{"bNativeSpaceAnalysis":true}}
}}
```

### Set M0-scaling factor and skip dummy scans
Correct scaling between the M0 and ASL scans is still an issue for certain ASL implementations. While for most vendors and sequences, this is automatically detected, you might need to add a manual scaling of the M0-scans. Furthermore, in case that dummy scans are part of the ASL sequence, you can indicate their position in the ASL timeseries, so that they are skipped:
```json
{"x":{
    "modules":{
        "asl":{
            "M0_GMScaleFactor": 10,
            "DummyScanPositionInASL4D": [2,3]
}}}}
```

### Turn on Partial volume correction and other ASL-processing parameters
While partial-volume correction is outputted in the Population module as done per region, you can also activate standard Partial-volume correction in the ASL-native space and produce GM-CBF maps in native space (which are later converted to standard space). Additionally, you can switch ON/OFF certain ASL-processing steps:

```json
{"x":{
    "modules":{
        "asl":{
            "motionCorrection": 0,
            "bPVCNativeSpace": 1,
            "PVCNativeSpaceKernel": [10 10 4],
            "bPVCGaussianMM": 1
}}}}
```

### Run at lower quality and skip some missing scans
The general settings allows to run ExploreASL in a faster mode at lower quality, or to not skip certain subjects if they are missing certain scans, or stop the pipeline after too many errors are reported:
```json
{"x":{
    "settings":{
        "Quality": 1,
        "SkipIfNoASL": 1,
        "SkipIfNoM0": 1,
        "SkipIfNoFlair": 1,
        "stopAfterErrors": 5
}}}
```

### Supply missing ASL parameters and set quantification parameters
Sequence parameters should be specified in the Import part that converts DICOMs to BIDS -- see [Import Tutorial](./../Tutorials-Import).
In the rare case where a sequence parameter would be missing in the ASL-BIDS format of data, and that it wouldn't be added in the Import or later, this parameter can be supplied as part of the `dataPar.json` (although adding it directly to subject's JSON-sidecars is better). Similarly, we can alter the CBF quantification parameters and constants either for the whole study by adding it in `dataPar.json` (or better adding the same parameters listed here directly to the subject's JSON-sidecar). Note that in dataPar.json, unlike in ASL-BIDS, most values are given in [ms] instead of [s]. Also, the parameters' name can differ slightly from the ASL-BIDS definition - more info is in [Processing Parameters](./../ProcessingParameters).

```json
{"x":{
    "Q":{
        "BackgroundSuppressionNumberPulses": 2,
        "BackgroundSuppressionPulseTime": [100, 500, 1500],
        "Initial_PLD": 1800,
        "T2art": 50,
        "TissueT1": 1240
}}}
```
