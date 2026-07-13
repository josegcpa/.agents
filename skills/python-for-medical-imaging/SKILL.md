---
name: python-for-medical-imaging
description: Contains opinionated general instructions and guidelines on how to develop software using Python for medical imaging.
---

# Python development for medical imaging skill

Always read `python-general` skill before developing Python code.

## Instructions

### Clinical imaging (radiology, nuclear medicine)

- Interact with DICOM data using `pydicom`. When exporting or sharing data, remove or anonymise PHI first; prefer `pydicom` anonymisation helpers or a dedicated de-identification library.
- If you need to perform general operations on medical images which you believe are relatively common please install `cciu` (`uv add git+https://github.com/CCIG-Champalimaud/cciu`). Pin the dependency to a stable reference in `pyproject.toml` for reproducibility. If you are looking for a functionality which is not available there, please suggest it before proceeding. `cciu` contains utilities for SimpleITK-readable formats, `pydicom`, and Orthanc access
- Interact with nifti/nrrd/mha files using `SimpleITK`. Avoid converting to numpy or other Python formats unless strictly necessary and always use `SimpleITK`-based operations
- Validate images before processing: check spacing, origin, direction, and size are consistent; reject empty or zero-volume images and log the reason.
- For CT, apply windowing/level transforms using the modality-specific units (HU) before visualisation or model input.
- When resampling, match the reference spacing/origin/direction explicitly. When resampling mask data, always use `sitkLabelLinear`
- When exporting Nifti images created from other Nifti images (i.e. segmentation masks) always copy the spacing, direction and origin information from the input to the output
- When exporting DICOM-seg images, always use `pydicom-seg`. If the user did not provide a metadata file, generate one with a placeholder structure with the following format:
```
{
  "ContentCreatorName": "Reader1",
  "ClinicalTrialSeriesID": "Session1",
  "ClinicalTrialTimePointID": "1",
  "SeriesDescription": "Segmentation",
  "SeriesNumber": "300",
  "InstanceNumber": "1",
  "BodyPartExamined": "BODY",
  "segmentAttributes": [
    [
      {
        "labelID": 1,
        "SegmentDescription": "Segmentation",
        "SegmentAlgorithmType": "SEMIAUTOMATIC",
        "SegmentAlgorithmName": "<algorithm-name>",
        "SegmentedPropertyCategoryCodeSequence": {
          "CodeValue": "260787004",
          "CodingSchemeDesignator": "SCT",
          "CodeMeaning": "Physical object"
        },
        "SegmentedPropertyTypeCodeSequence": {
          "CodeValue": "122485",
          "CodingSchemeDesignator": "DCM",
          "CodeMeaning": "Sphere"
        }
      }
    ]
  ],
  "ContentLabel": "SEGMENTATION",
  "ContentDescription": "Image segmentation",
  "ClinicalTrialCoordinatingCenterName": "dcmqi"
}
```

### Histopathology

- Always use openslide (`uv add openslide-python openslide-bin`) to read histopathology images
- When exporting predictions always use _at least_ GeoJSON unless otherwise specified
- If requested, please also implement `SpatialData` data exports (see: https://spatialdata.scverse.org/en/stable/tutorials/notebooks/notebooks/examples/intro.html)

## When to Use This Skill

Use `/python-for-medical-imaging` when:
- Working with DICOM, NIfTI, NRRD, or MHA images
- Processing radiology, nuclear medicine, or histopathology data
- Anonymising or de-identifying medical imaging data
- Resampling, windowing, or visualising clinical images
- Exporting WSI annotations or spatial imaging results
