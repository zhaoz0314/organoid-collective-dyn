# Plotting-data placement

Place the two deposited pickle files under `analysis/result/`:

```text
analysis/result/
├── PKL_PLACEMENT.md
├── mv_smry_s.pkl
└── figure_imshow.pkl
```

`mv_smry_s.pkl` contains the numerical analysis and statistical inputs used by the manuscript figures. `figure_imshow.pkl` contains the selected two-dimensional example frames and CNMF spatial arrays used by the figure `imshow` and contour calls; it does not contain full TIFF stacks.

With both files in these locations, run the population-dynamics notebook from the `# paper figures` section onward to regenerate the manuscript figures without rerunning the analysis. Running the complete notebook regenerates both pickle files immediately before plotting.
