---
name: python-development-for-medical-imaging
description: Contains opinionated general instructions and guidelines on how to develop software using Python for medical imaging.
---

Always read `python-development-general` skill before developing Python code.

### Clinical imaging (radiology, nuclear medicine)

- Interact with DICOM data using `pydicom`. When exporting or sharing data, remove or anonymise PHI first; prefer `pydicom` anonymisation helpers or a dedicated de-identification library.
- If you need to perform general operations on medical images which you believe are relatively common please install `cciu` (`uv add git+https://github.com/CCIG-Champalimaud/cciu`). Pin the dependency to a stable reference in `pyproject.toml` for reproducibility. If you are looking for a functionality which is not available there, please suggest it before proceeding. `cciu` contains utilities for SimpleITK-readable formats, `pydicom`, and Orthanc access
- Interact with nifti/nrrd/mha files using `SimpleITK`. Avoid converting to numpy or other Python formats unless strictly necessary and always use `SimpleITK`-based operations
- Validate images before processing: check spacing, origin, direction, and size are consistent; reject empty or zero-volume images and log the reason.
- For CT, apply windowing/level transforms using the modality-specific units (HU) before visualisation or model input.
- When resampling, match the reference spacing/origin/direction explicitly. When resampling mask data, always use `sitkLabelLinear`

### Histopathology

- Always use openslide (`uv add openslide-python openslide-bin`) to read histopathology images
- When exporting predictions always use _at least_ GeoJSON unless otherwise specified
- If requested, please also implement `SpatialData` data exports (see: https://spatialdata.scverse.org/en/stable/tutorials/notebooks/notebooks/examples/intro.html)
- Never export patient identifiers alongside WSI annotations; ensure all exported metadata is de-identified.
