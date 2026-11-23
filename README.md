# Allen Cell Types Analysis

Data snapshot and environment setup for analyzing neuronal electrophysiology from the [Allen Brain Atlas Cell Types Database](https://celltypes.brain-map.org/). The repository currently holds the downloaded metadata and feature tables. No analysis notebooks or results are committed yet.

## What is in this repo

| File | Contents |
| ---- | -------- |
| `cells.json` | Metadata for 2,333 cells (1,920 mouse, 413 human): donor, brain structure, cortical layer, dendrite type, Cre line, and summary electrophysiology and morphology fields |
| `ephys_features.csv` | Electrophysiology feature table, one row per cell (input resistance, tau, resting potential, AP threshold, peak and trough values, upstroke/downstroke ratio, F-I curve slope, adaptation, ISI, burst/pause/delay flags) |
| `manifest.json` | AllenSDK manifest that maps file keys to local paths |
| `requirements.txt` | Python dependencies (AllenSDK, pandas, scikit-learn, UMAP, JupyterLab, plotting libraries) |

Per-specimen NWB recordings and SWC reconstructions are not committed; `.gitignore` excludes them.

## Setup

Use Python 3.11 (AllenSDK does not support newer NumPy/pandas).

```bash
git clone https://github.com/ZhuoxiChen09/allen-cell-types-analysis.git
cd allen-cell-types-analysis
python -m venv .venv
# Windows:  .venv\Scripts\activate
# macOS/Linux:  source .venv/bin/activate
pip install -r requirements.txt
```

To reload the tables with the AllenSDK:

```python
from allensdk.core.cell_types_cache import CellTypesCache

ctc = CellTypesCache(manifest_file="manifest.json")
cells = ctc.get_cells()
ephys_features = ctc.get_ephys_features()
```

## Planned analysis

Exploratory plots of feature distributions by species, layer, and dendrite type; PCA/UMAP on the ephys features; clustering compared against Cre line and dendrite-type labels; and classification of dendrite type from ephys features.

## License

Code in this repository is under the MIT License (see `LICENSE`). The Allen Cell Types data is provided by the Allen Institute under its [Terms of Use](https://alleninstitute.org/legal/terms-use/).
