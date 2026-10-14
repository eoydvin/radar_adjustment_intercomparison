# radar_adjustment_intercomparison

An intercomparison of radar adjustment methods, with focus on radar–CML merging, using open datasets.

This repository contains the code for the paper:

> Øydvin, E., Covi, E., Graf, M., and Chwala, C.: Improving weather radar rainfall
> estimates by merging with commercial microwave link data: a fully reproducible,
> large-scale method intercomparison, Hydrol. Earth Syst. Sci., accepted, 2026.

The discussion version (preprint) is available at
https://doi.org/10.5194/egusphere-2025-6371.

Radar–CML merging methods (additive, multiplicative and kriging with external drift,
using IDW, point kriging and block kriging) are compared on two open datasets:
OpenMRG (Gothenburg, Sweden) and OpenRainER (Emilia-Romagna, Italy). Merging is done
with [mergeplg](https://github.com/OpenSenseAction/mergeplg).

## Setup

```bash
git clone --recursive https://github.com/OpenSenseAction/radar_adjustment_intercomparison
cd radar_adjustment_intercomparison
conda env create -f environment.yml
conda activate radar_adjustment_intercomparison
```

`mergeplg`, `poligrain`, `pycomlink` and `OPENSENSE_sandbox` are included as git
submodules, pinned to the versions used in the paper. The notebooks import them
from the submodule folders, so nothing needs to be pip-installed.

## Notebooks

Run the notebooks in numerical order. The three `3_adjust_radar_*check` notebooks
are independent of each other and of `3_adjust_radar`.

| Notebook | Purpose | Output |
|---|---|---|
| `1_data_preparation` | Download and prepare OpenMRG and OpenRainER (CML, radar, gauges) | `data/andersson_2022_OpenMRG/`, `data/covi_2024_OpenRainER/` |
| `2_cml_processing` | CML quality control, wet/dry classification and rain rate retrieval | `data/processed_cml_*.nc` |
| `3_adjust_radar` | Merge radar and CML data with all methods | `data/adjusted_fields/` |
| `3_adjust_radar_nnearcheck` | Sensitivity analysis: number of neighbours | `data/metrics/` |
| `3_adjust_radar_rangecheck` | Sensitivity analysis: range checks | `data/metrics/` |
| `3_adjust_radar_variogramcheck` | Sensitivity analysis: variogram estimation | `data/metrics/` |
| `4_analysis` | Evaluation of radar and merged fields against rain gauges | `data/metrics/` |
| `5_paper_images` | Overview, performance and sensitivity figures | `figures/` |
| `6_case_study` | Case-study figures | `figures/` |
| `7_variogram_study` | Variogram parameter study | `figures/` |
| `8_compress_adjusted_fields` | Lossless compression and metadata for archiving the merged fields on Zenodo (not needed to reproduce the results) | `data/adjusted_fields_compressed/` |

## Reproducing the results

`data/metrics/` contains all evaluation results used in the paper, so most figures
can be reproduced without downloading data or running the merging. A full
reproduction runs all notebooks in order: notebook 1 downloads about 25 GB of
data, and merging (notebook 3 and its sensitivity checks) takes several hours
and about 15 GB of disk space.

The merged rainfall fields from `3_adjust_radar.ipynb` are available on Zenodo:
https://doi.org/10.5281/zenodo.23367571. To skip the merging, download the files
into `data/adjusted_fields/` instead of running `3_adjust_radar.ipynb`.

The figures fall into the following groups:

**Dataset overview** (`5_paper_images`)
Map of the CML networks, gauges and radar rainfall, CML length and frequency
distributions, and data availability.
Requires notebooks 1–2.

**Overall performance** (`5_paper_images`)
Metrics of radar and merged fields against rain gauges: overall, by rainfall
intensity, and by distance from the nearest CML.
Runs from the included `data/metrics/`.

**Sensitivity analyses** (`5_paper_images`)
Effect of variogram estimation strategy, range checks and number of neighbours
on merging performance.
Runs from the included `data/metrics/` (produced by the `3_adjust_radar_*check` notebooks).

**Variogram study** (`7_variogram_study`)
Variogram parameters estimated from CMLs, gauges and radar over time.
Requires notebooks 1–2.

**Case studies** (`6_case_study`)
Merged rainfall fields and their differences from radar and gauges for selected
time steps, for both datasets.
Requires notebooks 1–3.

## Data

Both datasets are downloaded automatically by `1_data_preparation`.

- OpenMRG: Andersson et al. (2022), version 1.1, https://zenodo.org/records/7107689 (CC BY-SA 4.0)
- OpenRainER: Covi and Roversi (2025), version 2.0.0, https://zenodo.org/records/14731404 (CC BY 4.0)
