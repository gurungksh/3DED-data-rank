# 3DED Data Quality Ranking from PETS2 Output

## Overview
This project provides a **Jupyter Notebook** for analyzing and ranking **3D Electron Diffraction (3DED)** datasets generated using the **PETS2 software**. 
The notebook reads the CSV file produced by the **PETS2 Project Explorer** function, classifies datasets by **crystal (lattice) type**, and ranks data quality based on commonly used crystallographic metrics.

⚠️ **Important:**  
All datasets must be **fully processed in PETS2** before using this notebook. This tool is intended to help the crystallographer crudely pick which crystal dataset(s) to use for
further processing for the structure solution and refinement.

---

## Features
- Reads PETS2-generated CSV output
- Sorts datasets by **lattice parameters / crystal type**
- Ranks data quality using:
  | Metric           | Direction        | Notes                                                        |
| ---------------- | ---------------- | ------------------------------------------------------------ |
| Completeness (%) | Higher is better | Fraction of theoretically possible reflections measured      |
| Resolution (Å)   | Lower is better  | Smaller Å → higher resolution → better quality               |
| Mosaicity (°)    | Lower is better  | Lower mosaicity → better crystal order                       |
| Rint(obs) (%)    | Lower is better  | Internal consistency of observed reflections                 |
| Rint(all) (%)    | Lower is better  | Internal consistency including symmetry-related reflections  |
| CC1/2 (%)        | Higher is better | Correlation between half datasets → high = high signal/noise |

Each quality metric is normalized using a robust Z-score normalization, which reduces sensitivity to outliers and ensures comparability across datasets with different numerical scales.
For each metric, the normalized value is calculated as:

Z = (x - median(x)) / MAD(x)
where:
x is the value of the metric,
median(x) is the median value of that metric across all datasets,
MAD(x) is the median absolute deviation.

This produces dimensionless normalized values, allowing metrics with different units and ranges to be combined meaningfully.

**Metric Weighting**

Because different metrics contribute unequally to overall crystallographic data quality, a weighted scoring scheme is applied. In particular, resolution is weighted more heavily, as it strongly correlates with several other indicators of data quality.
The weights used in the final composite score are:

Metric      	Weight
Resolution	  0.30
Completeness	0.20
CC1/2	        0.20
Rint(obs)	    0.10
Rint(all)	    0.10
Mosaicity	    0.10

**Composite Quality Score**

The final quality score is calculated as a weighted sum of the normalized metrics:

Q = sum(w_i * Z_i)
where:
w_i is the weight assigned to metric i,
Z_i is the normalized value of metric i.
Higher values of Q indicate higher overall data quality.

**Notes**

All metrics are transformed so that higher values consistently represent better data quality
Rankings are performed within each crystal (lattice) type
Weighting values can be adjusted depending on experimental priorities
- Provides an objective comparison of multiple datasets
- Designed for **academic and research use**

---

## Requirements
- Python 3.x
- Jupyter Notebook
- Required Python packages:
  - `pandas`
  - `numpy`

(Additional libraries may be required depending on visualization or extensions.)

---

## Repository Structure

