# Neural organoid calcium-imaging analysis

This repository contains the processing and analysis notebooks for the accompanying neural-organoid manuscript. The released workflow starts from standardized TIFF recordings, performs motion correction and CNMF segmentation, and then analyzes collective population dynamics and generates the manuscript figures.

## Directory layout

```text
.
├── README.md
├── tif/
│   ├── DATA_PLACEMENT.md
│   └── *.tif
├── mcorr/                         # created by notebook 1
├── cnmf/                          # created by notebook 2
├── mesmerize-batch/               # created by notebooks 1 and 2
└── analysis/
    ├── 1 motion correction.ipynb
    ├── 2 cnmf.ipynb
    ├── 3 rmt clean.ipynb
    └── result/                     # created by notebook 3
        ├── multi/
        ├── single/
        └── none/
```

Extract the standardized recording archive so that the TIFF files sit directly in `tif/`. See [`tif/DATA_PLACEMENT.md`](tif/DATA_PLACEMENT.md) for the filename structure and the provenance of the layered-organoid identifiers.

Each notebook defines `main_path`. Set it to the repository root on the local system before running the notebook.

## Workflow

The notebooks are intended to be run in numerical order. In notebooks 1 and 2, set `tsu_type` to `"org-"` or `"cab-"` and run the notebook separately for the two recording types.

### 1. Motion correction

`analysis/1 motion correction.ipynb` reads standardized recordings from `tif/`, performs motion correction with Mesmerize/CaImAn, and writes the analysis-ready movies to `mcorr/`.

The notebook stores Mesmerize batch state in `mesmerize-batch/<tsu_type>mcorr.pickle`. On the first run for a recording type, initialize that batch with:

```python
df = mc.create_batch(batch_path)
```

On later runs, load the existing batch with:

```python
df = mc.load_batch(batch_path)
```

The notebook creates both `mesmerize-batch/` and `mcorr/` if they do not exist.

### 2. CNMF segmentation

`analysis/2 cnmf.ipynb` reads the motion-corrected TIFF files from `mcorr/`, performs CNMF segmentation, and writes one `cnmf/<recording>.npz` file per recording. Each NPZ contains the extracted activity trajectories, component labels, component centers, spatial components, and spatial and temporal background components used by the analysis.

CNMF batch state is stored in `mesmerize-batch/<tsu_type>cnmf.pickle`. As in notebook 1, use `mc.create_batch(batch_path)` on the first run and `mc.load_batch(batch_path)` on later runs. The notebook creates both `mesmerize-batch/` and `cnmf/` if they do not exist.

### 3. Population-dynamics analysis and figures

`analysis/3 rmt clean.ipynb` is the clean release copy of the population-dynamics analysis. It matches recordings by basename across `mcorr/` and `cnmf/`, performs the reported analyses and statistical comparisons, and generates the manuscript figures.

The notebook creates `analysis/result/multi/`, `analysis/result/single/`, and `analysis/result/none/` for per-recording diagnostics. It also writes the consolidated analysis output `analysis/result/mv_smry_s.pkl` and the main and supplementary figure PDFs to `analysis/result/`.

## Software

The preprocessing notebooks use Mesmerize Core and CaImAn together with NumPy, pandas, Matplotlib, Fastplotlib, tifffile, IPyWidgets, and IPyMPL. The population-dynamics notebook additionally uses SciPy, HDBSCAN, scikit-learn, and Pingouin.

Explicitly stochastic steps in the population-dynamics analysis use fixed random seeds.
