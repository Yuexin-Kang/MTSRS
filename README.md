# Data and Supplementary Computational Records for MTS/RS Order-Tote-Robot Coordination

Author: Yuexin Kang

This repository provides data, experimental tables, cleaned computational records, and implementation-oriented pseudocode for the numerical study on order-tote-robot coordination in multi-tote storage and retrieval systems with sequential picking stations.

The repository is organized around the experimental design of the study. It includes instance scale tables, generated instance data, parameter settings, tabulated experimental results, cleaned computational records, and detailed pseudocode for implementing the instance-generation and experimental workflows.

## Repository Structure

```text
data/
  instance_scales/
  generated_instances/

results/
  experimental_tables/
  computational_records/

docs/
  instance_generation.md
  parameter_settings.md
  experimental_workflow.md
  repository_file_index.md

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

The folder `data/instance_scales/` contains instance scale tables for the main experiments, DRL-ALNS comparison, sensitivity analyses, and emergency-reservation experiments.

The folder `data/generated_instances/` contains compact generated instance data, including instance metadata, order-tote requirement matrices, conveyor-side parameters, station processing times, robot-storage-side node information, and a manifest file for all exported instances.

## Experimental Results

The folder `results/experimental_tables/` contains the tabulated experimental results from the numerical study, including the main comparison experiments, total-completion-time experiments, sensitivity analyses, and emergency-reservation experiments.

The folder `results/computational_records/` contains cleaned computational records organized by experiment type. These records provide additional numerical details for the reported experiments.

## Pseudocode and Experimental Workflow

The folder `pseudocode/` provides detailed implementation-oriented pseudocode for the instance-generation procedure, integrated MILP workflow, DRL-HALNS workflow, baseline methods, main numerical experiments, sensitivity analyses, and emergency-reservation experiments.

These files describe the computational workflow at the level of data generation, algorithm execution, result recording, and result aggregation.

## Experimental Environment

The numerical experiments were conducted on a Windows operating system with a 2.90 GHz CPU and 16 GB RAM. The MILP model was solved using Gurobi 10.0.1. PPO training for operator selection used Stable Baselines3 PPO 2.2.1. Additional parameter settings are provided in `docs/parameter_settings.md`.

## File Index

The file `docs/repository_file_index.md` provides an overview of the repository files and their contents.
