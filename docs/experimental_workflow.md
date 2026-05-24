# Experimental Workflow

Author: Yuexin Kang

## Overview

The numerical study evaluates integrated order-tote-robot coordination in MTS/RSs with sequential picking stations. The experiments are organized into four groups:

1. Main comparison experiments under the makespan objective.
2. Total-completion-time experiments.
3. Sensitivity analyses on layout, workload, and resource parameters.
4. Emergency-reservation experiments.

All experiments use synthetically generated instances under fixed random seeds. Algorithms within the same experiment group are evaluated under the same instance-generation protocol and parameter settings.

## Main Comparison Experiments

The main experiments compare the integrated decision method with fast stage-wise baselines. The integrated method is evaluated using:

- Gurobi for the integrated MILP model, when applicable.
- DRL-HALNS for scalable integrated scheduling.

The fast stage-wise baselines include:

- A rule-based baseline.
- A greedy baseline.

The main performance metrics are objective value, runtime, feasibility status, optimality status when applicable, and improvement ratios over baseline methods.

Representative instance files for the main comparison experiments are provided in `data/representative_instances/`.

## Total-Completion-Time Experiments

The total-completion-time experiments use the same modeling framework but replace the makespan objective with the sum of order completion times. The experiments evaluate whether the integrated decision approach remains effective under a cumulative order-completion metric.

The output files include tabulated TCT results.

## Sensitivity Analyses

The sensitivity analyses evaluate the effects of key system factors:

- Warehouse width and length.
- Number of orders.
- Number of required totes.
- Number of ACRs.
- Number of picking stations.

For each sensitivity analysis, one target factor or a pair of target factors is varied while other parameters remain fixed. The resulting objective values are recorded and summarized in tabulated experimental results.

Both makespan and total completion time are evaluated in the sensitivity analyses.

## Emergency-Reservation Experiments

The emergency-reservation experiments evaluate whether reserving robots and station-buffer capacity improves emergency-order responsiveness. Each experiment compares two settings:

1. No reservation: all robots and station-buffer units are used for regular orders, and emergency orders are appended to the regular station schedule.
2. Reservation: a subset of robots and station-buffer units is reserved for emergency orders, and regular orders are scheduled with the remaining resources.

Emergency orders arrive at a time determined by the emergency arrival-time ratio. The emergency performance metrics include response delay, completion time, response benefit, completion benefit, regular makespan cost, and baseline waiting indicators.

## Result Recording and Aggregation

The numerical results are organized in `results/Overall Computational Results.xlsx`. The workbook contains the tabulated results for the main comparison experiments, total-completion-time experiments, sensitivity analyses, and emergency-reservation experiments.
