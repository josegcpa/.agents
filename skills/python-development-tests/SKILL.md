---
name: python-development-tests
description: Contains instructions and guidelines on how to implement tests for Python code.
---

# Python tests development skill

Always read `python-development-general` skill before developing Python code.

## Instructions

- When asked to implement tests, use `pytest` and always use `pytest` fixtures to define common test data and configurations. Any data download should make use of a fixture which downloads the data to a temporary directory and cleans it up after the test
- Prefer deterministic, synthetic test data when possible. Keep tests fast and isolated; if the slowness of a test becomes an impediment, use `pytest-mock`, but keep this to a minimum.
- When testing applications which require GPU use/other resources which might be available across all machines (i.e. specific API accesses) do not mock these. Instead, skip these tests by checking for the availability of these resources
- Use `pytest.mark.parametrize` for testing multiple inputs/edge cases against the same logic.
- Use the `tmp_path` fixture for file output and ensure cleanup is automatic.
- Measure coverage with `pytest-cov` and aim for meaningful coverage of critical paths rather than chasing a number.
- When asked to implement tests for DICOM images, make use of the `pydicom` test images (see: https://pydicom.github.io/pydicom/stable/reference/examples.html)
- When asked to implement tests for nifti/nrrd/mha images, use the following `nibabel` test image: https://github.com/yarikoptic/nitest-balls1/raw/2cd07d86e2cc2d3c612d5d4d659daccd7a58f126/NIFTI/T2.nii.gz
- When testing histopathology workflows, use https://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/CMU-1-Small-Region.svs as a test WSI
