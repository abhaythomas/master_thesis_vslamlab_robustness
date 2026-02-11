# Master Thesis VSLAM Lab Robustness Artifacts

This repository is an **artifact and results collection** for evaluating visual SLAM robustness, with a strong focus on KITTI corruption studies and cross-method comparisons.

It stores:
- experiment logs exported from VSLAM-LAB runs,
- per-sequence trajectory/error outputs,
- consolidated CSV tables used for analysis,
- plots and media used in reports/presentations.

> Note: This repository currently contains data/results, not a standalone training or inference codebase.

## Repository layout

### `VSLAM-LAB-Evaluation/`
Experiment-level logs (`vslamlab_exp_log.csv`) grouped by campaign/folder.

Typical columns include method, dataset, sequence, status, resource usage (RAM/SWAP/GPU), run time, and comments.

### `Results_File/`
Sequence-level and condition-level outputs, including:
- trajectory files (e.g., `00000_KeyFrameTrajectory.txt`),
- run logs (`system_output_00000.txt`, `log_run_sequence_time.csv`),
- summary visualizations (`trajectory_comparison.png`, `rpe_plot.png`),
- metric snapshots (`metrics.txt`).

### `Results_Consolidated/`
Merged analysis outputs and supervisor-facing tables.

- `CONSOLIDATION_SUMMARY.txt` tracks availability state (OK / EMPTY / MISSING) by dataset-method pair.
- `tables_for_supervisor/` contains aggregate performance tables (e.g., clean/2D/3D/mixed KITTI and 4Seasons).

### `coverage_plots/`
Trajectory coverage diagnostics and overlay plots (e.g., common-key coverage in the XZ plane).

### `corruption_samples_for_thesis/`
Representative image samples for clean/corrupted conditions across corruption families and severities.

### `corruption_check_report.csv`
Frame-level corruption sanity-check summary per sequence/corruption/severity.

### Other artifact folders/files
- `final_plots/`, `presentation_error_plots/`, `trajectory_error_plots/`, `plots_honest_kitti00_per_method/`:
  report-ready figures.
- `logs/`: scheduler/runtime logs from longer experiment runs.
- `ape_plot.pdf`, `rpe_plot.pdf`, zipped result bundles: exported evaluation assets.

## Data conventions observed

- Methods frequently appearing in tables/logs: **DPVO**, **DROID**, **ORB** (plus additional campaigns such as MonoGS / MASt3RSLAM in evaluation logs).
- Key evaluation metrics: **APE RMSE**, **RPE RMSE**, and **coverage percentage**.
- Corruption studies include categories such as brightness/contrast/fog/blur/noise and are often indexed by severity 1-5.

## How to use this repository

### 1) Quick inspection
You can browse summary artifacts directly:
- `Results_Consolidated/CONSOLIDATION_SUMMARY.txt`
- `Results_Consolidated/tables_for_supervisor/*.csv`
- `coverage_plots/00_coverage_summary.txt`

### 2) Programmatic analysis
Load CSV files with Python/pandas for custom aggregation:

```python
import pandas as pd

kitti = pd.read_csv(
    "Results_Consolidated/tables_for_supervisor/"
    "table_kitti_all_clean_2d_3d_mixed_all_severities.csv"
)

print(kitti.groupby(["category", "method"])["ape_rmse"].mean().sort_values())
```

### 3) Reproducibility notes
This repo does not currently include the full experiment-launch scripts/pipelines.
To reproduce runs end-to-end, use your VSLAM-LAB experiment definitions and treat this repository as the output/analysis companion.

## Suggested additions (optional)

If you want this to become fully self-contained, add:
1. dataset preparation instructions,
2. experiment launch commands/config files,
3. evaluation scripts used to produce consolidated tables,
4. a short data dictionary for each key CSV schema.

## License

No explicit license file is currently present.
If this repository will be shared publicly, consider adding a `LICENSE` and citation guidance.
