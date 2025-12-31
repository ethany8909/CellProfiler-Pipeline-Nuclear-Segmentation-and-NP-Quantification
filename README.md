# CellProfiler Pipeline: Nuclear Segmentation and NP Quantification

## Overview
This CellProfiler pipeline segments nuclei using DAPI staining, measures nuclear protein (NP) intensity, calculates NP-positive rates per image, and exports results as CSV files. The pipeline is designed for high-throughput image-based quantification at the single-cell level.

---

## Pipeline Objectives
- Segment nuclei based on DAPI fluorescence to obtain total cell counts
- Measure NP fluorescence intensity within each nucleus
- Classify nuclei as NP-positive or NP-negative based on intensity thresholds
- Calculate NP-positive rate for each image
- Export per-cell and per-image metrics as CSV files
- Save all results in the same folder as input images

---

## Input Requirements
- Fluorescence microscopy images
- Channels:
  - DAPI (nuclear stain)
  - NP (nuclear protein of interest)
- Supported formats: TIFF
- Filename pattern expected by the pipeline:
^(?P<Cond>.+)Plate(?P<Plate>[A-Z])p\d+\d+_(?P<WellRow>[A-Z])(?P<WellNum>\d+)f\d+d(?P<ImageIdx>[0-3]).TIF$

Where:
- `Cond` = experimental condition
- `Plate` = plate identifier
- `WellRow` and `WellNum` = well coordinates
- `ImageIdx` = channel index (0–3)

This ensures correct channel assignment and grouping for analysis.

---

## Pipeline Logic
1. **RescaleIntensity**  
   Normalize DAPI and NP channels to enhance segmentation accuracy.

2. **IdentifyPrimaryObjects**  
   Segment nuclei from the rescaled DAPI image, with manual thresholding and object size constraints.

3. **MeasureObjectIntensity**  
   Quantify NP intensity within each nucleus.

4. **CalculateMath**  
   Compute per-image NP-positive rates.

5. **ExportToSpreadsheet**  
   Export all per-cell and per-image measurements as CSV files to the same folder as input images.

---

## Outputs
- Per-cell metrics:
  - Nuclear area
  - NP intensity
  - NP-positive classification
- Per-image metrics:
  - Total nuclei
  - NP-positive nuclei
  - NP-positive rate
- Format: CSV
- Location: Same directory as input images

---

## Use Cases
- Quantification of nuclear protein expression
- Estimation of NP-positive cell frequency
- High-throughput phenotypic analysis
- Quality control of fluorescence imaging experiments

---

## Notes
- Thresholds may require adjustment depending on imaging conditions.
- Objects touching image borders or outside diameter ranges can be excluded.
- Pipeline is intended for visualization and quantification, not image modification.
