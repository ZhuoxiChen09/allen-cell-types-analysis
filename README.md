# Allen Cell Types Analysis

Exploratory analysis and classification of neuronal cell types using electrophysiology and morphology data from the [Allen Brain Atlas Cell Types Database](https://celltypes.brain-map.org/).

## Overview

The Allen Cell Types Database provides a standardized survey of cellular properties of neurons across the mouse and human brain. This project explores the dataset to characterize how neurons differ across:

- **Species** (mouse vs. human)
- **Brain region** (e.g., primary visual cortex, frontal lobe)
- **Cortical layer**
- **Dendrite type** (spiny vs. aspiny vs. sparsely spiny)
- **Cre line** (transgenic mouse lines marking specific neuron populations)

## Data

The dataset is sourced from the Allen Institute via the [AllenSDK](https://allensdk.readthedocs.io/) and includes:

| File | Description |
| ---- | ----------- |
| `cells.json` | Per-cell metadata: donor, species, brain structure, layer, dendrite type, summary ephys/morphology features |
| `ephys_features.csv` | Full intracellular electrophysiology feature table (rheobase, input resistance, AP threshold/peak/trough, upstroke/downstroke ratio, sag, tau, adaptation, ISI, F-I curve slope, etc.) |
| `manifest.json` | AllenSDK manifest describing file locations for per-cell NWB recordings, SWC reconstructions, and sweep metadata |

Per-specimen raw data (NWB electrophysiology traces, SWC morphology reconstructions) are downloaded on demand into `specimen_<id>/` directories and are excluded from version control.

## Feature highlights

Electrophysiology features analyzed include:

- **Subthreshold:** input resistance, membrane time constant (`tau`), resting potential (`vrest`), sag
- **Threshold / rheobase:** `threshold_i_long_square`, `threshold_v_long_square`, rheobase sweep
- **Action potential shape:** peak voltage, fast/slow trough, upstroke/downstroke ratio
- **Firing dynamics:** F-I curve slope, average firing rate, average ISI, adaptation index, burst/pause/delay flags

## Goals

1. Reproducible exploratory data analysis (EDA) of the cell types dataset
2. Visualization of ephys feature distributions across species, layers, and dendrite types
3. Dimensionality reduction (PCA / UMAP) over ephys feature space
4. Unsupervised clustering of cells and comparison against known transgenic line / dendrite-type labels
5. Supervised classification of dendrite type and species from ephys features alone

## Repository layout

```
allen-cell-types-analysis/
├── cells.json              # cell-level metadata + summary features
├── ephys_features.csv      # full ephys feature table
├── manifest.json           # AllenSDK manifest
├── notebooks/              # analysis notebooks (EDA, clustering, classification)
├── requirements.txt
└── README.md
```

## Getting started

```bash
git clone https://github.com/ZhuoxiChen09/allen-cell-types-analysis.git
cd allen-cell-types-analysis

python -m venv .venv
# Windows:  .venv\Scripts\activate
# macOS/Linux:  source .venv/bin/activate

pip install -r requirements.txt
```

To re-download the raw data via the AllenSDK:

```python
from allensdk.core.cell_types_cache import CellTypesCache

ctc = CellTypesCache(manifest_file="manifest.json")
cells = ctc.get_cells()
ephys_features = ctc.get_ephys_features()
```

## References

- Allen Institute for Brain Science. *Allen Cell Types Database.* https://celltypes.brain-map.org/
- AllenSDK documentation: https://allensdk.readthedocs.io/
- Gouwens, N. W., et al. (2019). *Classification of electrophysiological and morphological neuron types in the mouse visual cortex.* Nature Neuroscience, 22(7), 1182–1195.

## License

Analysis code in this repository is released under the MIT License. The underlying Allen Cell Types data is provided by the Allen Institute under their [Terms of Use](https://alleninstitute.org/legal/terms-use/).
