# 05. Numerical Experiment Workflow Pseudocode

Author: Yuexin Kang

This file describes the workflow for conducting the main numerical experiments under the makespan and total-completion-time objectives. The workflow is organized according to the experimental structure of the study rather than any internal implementation directory.

## Procedure 1. Run the Main Comparison Experiments

**Input**

- Main instance scale table.
- Seed list.
- Objective type, either `makespan` or `TCT`.
- Method set:
  - integrated MILP solved by Gurobi when applicable,
  - DRL-HALNS,
  - rule-based stage-wise baseline,
  - greedy stage-wise baseline.
- Algorithm and solver parameters.

**Output**

- Cleaned computational records.
- Tabulated experimental results.

**Steps**

1. Initialize an empty computational record table.
2. For each scale group:
   1. load the corresponding instance scale records.
3. For each instance scale record:
   1. For each seed in the seed list:
      1. Generate the full computational instance.
      2. If the integrated MILP is evaluated for this instance:
         1. build the integrated MILP model;
         2. solve the model under the selected objective;
         3. record upper bound, lower bound, runtime, gap, feasibility status, and optimality status.
      3. Run DRL-HALNS on the same generated instance.
      4. Run the rule-based stage-wise baseline on the same generated instance.
      5. Run the greedy stage-wise baseline on the same generated instance.
      6. Record all objective values and runtimes.
4. Aggregate records by instance ID:
   1. compute average objective values;
   2. compute average runtimes;
   3. compute average upper and lower bounds when available;
   4. count the number of feasible solver runs;
   5. count the number of optimal solver runs;
   6. compute improvement ratios relative to baselines.
5. Export the cleaned computational records.
6. Export the tabulated experimental results.

## Procedure 2. Makespan Experiment Workflow

**Input**

- Main instance scale table.
- Seed list.
- Method set.
- Algorithm and solver parameters.

**Output**

- Makespan computational records.
- Makespan experimental tables.

**Steps**

1. Set objective type to `makespan`.
2. Define the makespan as the maximum order completion time over all orders.
3. Run Procedure 1 under the makespan objective.
4. Separate the resulting records by scale group:
   1. small-scale instances,
   2. medium-scale instances,
   3. large-scale instances.
5. Generate tables for:
   1. Gurobi versus DRL-HALNS for small-scale instances;
   2. DRL-HALNS versus stage-wise baselines for small-scale instances;
   3. DRL-HALNS versus stage-wise baselines for medium-scale instances;
   4. DRL-HALNS versus stage-wise baselines for large-scale instances.
6. Export the makespan computational records and experimental tables.

## Procedure 3. Total-Completion-Time Experiment Workflow

**Input**

- Main instance scale table.
- Seed list.
- Method set.
- Algorithm and solver parameters.

**Output**

- TCT computational records.
- TCT experimental tables.

**Steps**

1. Set objective type to `TCT`.
2. Define TCT as the sum of order completion times, or as a weighted sum if order weights are specified.
3. Run Procedure 1 under the TCT objective.
4. Separate the resulting records by scale group.
5. Generate tables for:
   1. Gurobi versus DRL-HALNS for small-scale instances under TCT;
   2. DRL-HALNS versus stage-wise baselines for small-scale instances under TCT;
   3. DRL-HALNS versus stage-wise baselines for medium-scale instances under TCT;
   4. DRL-HALNS versus stage-wise baselines for large-scale instances under TCT.
6. Export the TCT computational records and experimental tables.

## Procedure 4. Aggregate Solver Records

**Input**

- Computational records for one experiment type.
- Grouping key, usually `instance_id`.

**Output**

- Aggregated result table.

**Steps**

1. Group all records by instance ID.
2. For each instance group:
   1. compute average objective value for each method;
   2. compute average runtime for each method;
   3. compute average upper bound and lower bound for Gurobi if applicable;
   4. compute the number of feasible Gurobi runs;
   5. compute the number of optimal Gurobi runs;
   6. compute the number of successful DRL-HALNS runs;
   7. compute improvement ratios relative to baselines.
3. Add an average row across all instance groups.
4. Return the aggregated result table.

## Procedure 5. Export Experimental Tables

**Input**

- Aggregated result tables.
- Output folder.

**Output**

- Spreadsheet files containing tabulated experimental results.

**Steps**

1. Create one workbook for the makespan experiments.
2. Add each makespan result table as a separate sheet.
3. Create one workbook for the TCT experiments.
4. Add each TCT result table as a separate sheet.
5. Use clear sheet names that identify the experiment and scale group.
6. Export workbooks to the experimental table folder.
