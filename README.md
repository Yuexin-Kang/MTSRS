# Data and Supplementary Materials for MTS/RS Order-Tote-Robot Coordination

Author: Yuexin Kang

This repository provides data files, tabulated numerical results, parameter settings, and implementation-oriented pseudocode for the study on order-tote-robot coordination in multi-tote storage and retrieval systems with sequential picking stations.

The repository is organized around the computational study. It includes instance scale tables, representative instance files, experimental result tables, and detailed pseudocode for the instance-generation and experimental workflows.

## Repository Structure

```text
data/
  instance_scales/
  representative_instances/

results/
  Overall Computational Results.xlsx

docs/
  instance_generation.md
  parameter_settings.md
  experimental_workflow.md

pseudocode/
  01_instance_generation.md
  02_integrated_milp_workflow.md
  03_drl_halns_workflow.md
  04_baseline_workflows.md
  05_numerical_experiment_workflow.md
  06_sensitivity_analysis_workflow.md
  07_emergency_reservation_workflow.md
```

## Data

The computational instances are synthetic instances generated under fixed random seeds. Each instance represents a wave-level order fulfillment problem in an MTS/RS with sequential picking stations. An instance is specified by the warehouse layout dimensions, the number of required totes, the number of released orders, the number of ACRs, and the number of picking stations.

The folder `data/instance_scales/` contains CSV files describing the instance scales and experiment grids used in the numerical study.

The folder `data/representative_instances/` provides representative instance files for the main comparison experiments. These files include order-tote requirement matrices, conveyor-side parameters, station processing times, system parameters, layout information, task records, robot records, and node records.

## Experimental Results

The file `results/Overall Computational Results.xlsx` provides the tabulated numerical results used in the computational study. The workbook includes the main comparison results, total-completion-time results, sensitivity analyses, and emergency-reservation results.

## Pseudocode and Experimental Workflow

The folder `pseudocode/` provides implementation-oriented pseudocode for the instance-generation procedure, integrated MILP workflow, DRL-HALNS workflow, baseline methods, main numerical experiments, sensitivity analyses, and emergency-reservation experiments.

These files describe the computational workflow at the level of data generation, algorithm execution, result recording, and result aggregation.

## Experimental Environment

The numerical experiments were conducted on a Windows operating system with a 2.90 GHz CPU and 16 GB RAM. The MILP model was solved using Gurobi 10.0.1. PPO training for operator selection used Stable Baselines3 PPO 2.2.1. Additional parameter settings are provided in `docs/parameter_settings.md`.
