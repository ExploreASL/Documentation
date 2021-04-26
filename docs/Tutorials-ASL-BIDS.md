
# Tutorials (ASL-BIDS)

----
## ExploreASL & ASL-BIDS

Starting with version **v1.6.0**, **ExploreASL** will support an import workflow which allows the user to convert **DICOM** and **NIFTI** data to the **ASL-BIDS** format. Since **ExploreASL** does not fully utilize the **BIDS** format internally, there will also be an automated workflow to convert from **ASL-BIDS** to the **ExploreASL** **legacy format**. In the following subsections we will explain how you can use the automated **ExploreASL** import workflow to convert your data structure to **ASL-BIDS** and how you can process it.

The `ExploreASL_Master` script will have the following format:

```matlab
[x] = ExploreASL([DataParPath, ImportModules, ProcessModules, bPause, iWorker, nWorkers])
```

**Parameter descriptions:**

- `DataParPath`: Path to data parameter file (`OPTIONAL`)

    |                  | DataParPath           |
    | ---------------- |:---------------------:|
    | **Type**         | `CHAR ARRAY`          |
    | **Default**      | Prompting user input  |

- `ImportModules`: Multi-step import workflow (`OPTIONAL`)
    - `DCM2NII`: Run the DICOM to NIFTI conversion
    - `NII2BIDS`: Run the NIFTI to BIDS conversion
    - `ANONYMIZE`: Run the defacing and full anonymization
    - `BIDS2LEGACY`: Run the BIDS to LEGACY conversion

    | ImportModules    | DCM2NII       | NII2BIDS      | ANONYMIZE     | BIDS2LEGACY   |
    | ---------------- |:-------------:|:-------------:|:-------------:|:-------------:|
    | **Type**         | `BOOLEAN`     | `BOOLEAN`     | `BOOLEAN`     | `BOOLEAN`     |
    | **Default**      | `false`       | `false`       | `false`       | `false`       |
    
- `ProcessModules`: Multi-step processing pipeline (`OPTIONAL`)
    - `STRUCTURAL`: Run the Structural Module
    - `ASL`: Run the ASL Module
    - `POPULATION`: Run the Population Module
    
    | ProcessModules   | STRUCTURAL    | ASL           | POPULATION    |
    | ---------------- |:-------------:|:-------------:|:-------------:|
    | **Type**         | `BOOLEAN`     | `BOOLEAN`     | `BOOLEAN`     |
    | **Default**      | `false`       | `false`       | `false`       |  
    
- `bPause`: Pause workflow before ExploreASL pipeline (`OPTIONAL`)

    |                  | bPause                |
    | ---------------- |:---------------------:|
    | **Type**         | `BOOLEAN`             |
    | **Default**      | `false`               |

- `iWorker`: Allows parallelization when called externally  (`OPTIONAL`)

    |                  | iWorker               |
    | ---------------- |:---------------------:|
    | **Type**         | `INTEGER`             |
    | **Default**      | `1`                   |

- `nWorkers`: Allows parallelization when called externally  (`OPTIONAL`)

    |                  | nWorkers              |
    | ---------------- |:---------------------:|
    | **Type**         | `INTEGER`             |
    | **Default**      | `1`                   |

In the following examples, we want to show how you can use the revised import workflow and how the conventional processing is done now.

----
## DICOM source data

Converting DICOM source data according to the [ASL BIDS](https://bids-specification.readthedocs.io/en/latest/99-appendices/12-arterial-spin-labeling.html) standard can be done using the new import workflow. For the upcoming release **v1.6.0** we're preparing an exemplary DICOM source dataset based on the [ASL DRO](https://pypi.org/project/asldro/). To run this workflow, you have to use the path to your `sourceStructure.json` file instead of the path to your `DataPar.json` file for the `DataParPath` input argument. Do not forget to set up the source dataset correctly. You'll be required to define a `studyPar.json` file as well. We're working on a flavor library, to enable the import and processing of a wide variety of different sequence, vendor, and scanner combinations.

The first step to convert your **DICOM** source data to **NIFTI** source data is to run the following command:

```matlab
[x] = ExploreASL('sourceStructure.json', [1 0 0 0], 0, 0, 1, 1);
```

Here we tell **ExploreASL** to run the `DCM2NII` import module by setting the first boolean variable of the `ImportModules` to `1`.

----
## NIFTI source data

Let's assume we have **NIFTI** source data according to the [ASL BIDS](https://bids-specification.readthedocs.io/en/latest/99-appendices/12-arterial-spin-labeling.html) standard now. To convert this **ASL BIDS** source data to **ASL BIDS** raw data, we have to run the second part of the import workflow. To run the second step, we set the second variable of the `ImportModules` to `1`. Similar to the previous step, we pass the path to the `sourceStructure.json` file to **ExploreASL**.

```matlab
[x] = ExploreASL('sourceStructure.json', [0 1 0 0], 0, 0, 1, 1);
```


----
## Data anonymization

There's also a new option to anonymize your data. To do this, you can run the third step of the import workflow. This is done by setting the third variable of the `ImportModules` to `1`. Similar to the previous steps, we pass the path to the `sourceStructure.json` file to **ExploreASL**.

```matlab
[x] = ExploreASL('sourceStructure.json', [0 0 1 0], 0, 0, 1, 1);
```


----
## Data in ExploreASL legacy format

Right now, **ExploreASL** still uses the conventional data structure. To convert our **ASL BIDS** rawdata to the **ExploreASL** legacy format, we run the last step of the import workflow. This is done by setting the fourth variable of the `ImportModules` to `1`. Similar to the previous steps, we pass the path to the `sourceStructure.json` file to **ExploreASL**.

```matlab
[x] = ExploreASL('sourceStructure.json', [0 0 0 1], 0, 0, 1, 1);
```

The import workflow will also generate a `DataPar.json` file. To adapt your pipeline, you can still use the same settings as before, by changing the **JSON** fields.


----
## ExploreASL processing pipeline

If you just want to use the conventional **ExploreASL** processing pipeline, you can simply turn off the import workflow by setting all `ImportModules` variables to `0` individually or by using a single `0` for the `ImportModules`. This results in the following notation:

```matlab
[x] = ExploreASL('sourceStructure.json', 0, 1, 0, 1, 1);
```

To run individual modules, you can set the `ProcessModules` individually. If you only want to the `Structural Module` for example, you can use a `[1 0 0]` vector. To run all modules, you can use a single `1` or a vector of ones, which should look like this `[1 1 1]`. Running the full processing pipeline can therefore be done like this as well:

```matlab
[x] = ExploreASL('sourceStructure.json', 0, [1 1 1], 0, 1, 1);
```

----
## Full pipeline

Running the full pipeline including both import workflow and processing pipeline, can be done by setting both `ImportModules` and `ProcessModules` to `1`.

```matlab
[x] = ExploreASL('sourceStructure.json', 1, 1, 0, 1, 1);
```

----
## Other tips and tricks

Note that we changed both name and behavior of the `SkipPause` variable. The variable is called `bPause` now. Setting it to `true` or `1`, will result in the pipeline being paused before the processing. We removed the `iModules`, but the functionality of `ProcessModules` is basically the same. We use boolean notation now, so instead of `[1 2 3]` you have to use `[1 1 1]` now.

The overall import functionality is a work in progress right now. We expect stable behavior in release `v1.6.0` though. If you plan on using the `develop` branch until then, you have to live with more or less unstable import behavior.

The examples shown below are related to versions `v1.5.1` and older. They will be updated soon. If you want to work with the current `develop` version, these examples obviously do not apply anymore.






