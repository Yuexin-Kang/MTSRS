# Repository File Index

Author: Yuexin Kang

This file summarizes the repository structure and the contents of each folder.

## Root Files

| File | Description |
|---|---|
| `README.md` | Overview of the repository, data contents, result files, pseudocode files, and experimental environment. |
| `DATA_AND_CODE_AVAILABILITY.md` | Data and code availability statement. |

## Documentation Files

| File | Description |
|---|---|
| `docs/instance_generation.md` | Description of the instance-generation protocol and generated instance fields. |
| `docs/parameter_settings.md` | System parameters, solver settings, DRL-HALNS settings, and emergency-reservation settings. |
| `docs/experimental_workflow.md` | Overview of the main experiments, TCT experiments, sensitivity analyses, and emergency-reservation experiments. |
| `docs/repository_file_index.md` | File index for the repository. |

## Data Files

| Folder or File | Description |
|---|---|
| `data/instance_scales/` | Instance scale tables and experimental grids. |
| `data/generated_instances/README.md` | Description of the generated instance file format and folder structure. |
| `data/generated_instances/generated_instances_manifest.csv` | Manifest of all exported generated instance files. |
| `data/generated_instances/main_experiments/` | Generated instance data for the main comparison experiments. |
| `data/generated_instances/drl_alns_comparison/` | Generated instance data for the DRL-HALNS and traditional ALNS comparison. |
| `data/generated_instances/sensitivity_experiments/` | Generated instance data for the sensitivity analyses. |
| `data/generated_instances/emergency_reservation/` | Generated instance data for the emergency-reservation experiments. |

Instance scale files:

| File | Description |
|---|---|
| `data/instance_scales/main_instances.csv` | Instance scales for the main small-, medium-, and large-scale experiments. |
| `data/instance_scales/main_instances.xlsx` | Spreadsheet version of the main instance scales. |
| `data/instance_scales/drl_alns_comparison_instances.csv` | Instance scales used for the DRL-HALNS and traditional ALNS comparison. |
| `data/instance_scales/drl_alns_comparison_instances.xlsx` | Spreadsheet version of the DRL-HALNS and traditional ALNS comparison instance scales. |
| `data/instance_scales/sensitivity_grids.csv` | Parameter grids used in sensitivity analyses. |
| `data/instance_scales/sensitivity_grids.xlsx` | Spreadsheet version of the sensitivity-analysis grids. |
| `data/instance_scales/emergency_experiment_grid.csv` | Parameter grid used in emergency-reservation experiments. |
| `data/instance_scales/emergency_experiment_grid.xlsx` | Spreadsheet version of the emergency-reservation experiment grid. |

## Result Files

| Folder or File | Description |
|---|---|
| `results/experimental_tables/` | Tabulated experimental results from the numerical study. |
| `results/computational_records/` | Cleaned computational records organized by experiment type. |

Experimental table files:

| File | Description |
|---|---|
| `results/experimental_tables/main_makespan_results.xlsx` | Main comparison results under the makespan objective. |
| `results/experimental_tables/main_tct_results.xlsx` | Main comparison results under the total-completion-time objective. |
| `results/experimental_tables/sensitivity_results.xlsx` | Sensitivity analysis results under makespan and TCT objectives. |
| `results/experimental_tables/emergency_reservation_results.xlsx` | Emergency-reservation experiment results. |

Computational record files:

| File | Description |
|---|---|
| `results/computational_records/README.md` | Description of the computational record files and record levels. |
| `results/computational_records/makespan_computational_records.csv` | Cleaned computational records for makespan experiments. |
| `results/computational_records/tct_computational_records.csv` | Cleaned computational records for total-completion-time experiments. |
| `results/computational_records/sensitivity_computational_records.csv` | Cleaned computational records for sensitivity analyses. |
| `results/computational_records/emergency_reservation_computational_records.csv` | Cleaned computational records for emergency-reservation experiments. |

## Pseudocode Files

| File | Description |
|---|---|
| `pseudocode/01_instance_generation.md` | Instance-generation and instance-export procedures. |
| `pseudocode/02_integrated_milp_workflow.md` | Integrated MILP workflow. |
| `pseudocode/03_drl_halns_workflow.md` | DRL-HALNS solution and PPO training workflows. |
| `pseudocode/04_baseline_workflows.md` | Rule-based and greedy baseline workflows. |
| `pseudocode/05_numerical_experiment_workflow.md` | Main makespan and TCT experiment workflows. |
| `pseudocode/06_sensitivity_analysis_workflow.md` | Sensitivity analysis workflow. |
| `pseudocode/07_emergency_reservation_workflow.md` | Emergency-reservation experiment workflow. |
