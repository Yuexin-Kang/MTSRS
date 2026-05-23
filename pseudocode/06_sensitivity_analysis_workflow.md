# 06. Sensitivity Analysis Workflow Pseudocode

Author: Yuexin Kang

This file describes the workflow for sensitivity analyses on layout, workload, and resources under makespan and total-completion-time objectives.

## Procedure 1. General Sensitivity Analysis

**Input**

- Base instance configuration.
- Target factor.
- Candidate values of the target factor.
- Objective type.
- Algorithm parameters.

**Output**

- Sensitivity computational records.
- Sensitivity result table.

**Steps**

1. Initialize an empty computational record table.
2. For each candidate value of the target factor:
   1. Copy the base instance configuration.
   2. Replace the target factor with the candidate value.
   3. Keep all non-target factors fixed.
   4. Generate the corresponding computational instance.
   5. Solve the instance using DRL-HALNS under the selected objective.
   6. Record:
      1. objective type,
      2. sensitivity type,
      3. target factor,
      4. target value,
      5. instance metadata,
      6. objective value,
      7. runtime.
3. Aggregate records if multiple seeds or replications are used.
4. Export the sensitivity computational records.
5. Export the tabulated sensitivity result.

## Procedure 2. Layout Sensitivity Analysis

**Input**

- Width candidate values.
- Length candidate values.
- Fixed workload and resource parameters.
- Objective type.

**Output**

- Layout sensitivity table.

**Steps**

1. Initialize a two-dimensional table indexed by width and length.
2. For each width value:
   1. For each length value:
      1. Generate the computational instance using the selected width and length.
      2. Solve the instance using DRL-HALNS.
      3. Record the objective value.
3. Store the objective value in the corresponding width-length cell.
4. Export the layout sensitivity table.

## Procedure 3. Workload Sensitivity Analysis

**Input**

- Candidate values for the number of orders.
- Candidate values for the number of totes.
- Fixed layout and resource parameters.
- Objective type.

**Output**

- Workload sensitivity tables.

**Steps**

1. Analyze the number of orders:
   1. For each candidate order count:
      1. keep tote count and other parameters fixed;
      2. generate the instance;
      3. solve the instance using DRL-HALNS;
      4. record objective value and runtime.
2. Analyze the number of totes:
   1. For each candidate tote count:
      1. keep order count and other parameters fixed;
      2. generate the instance;
      3. solve the instance using DRL-HALNS;
      4. record objective value and runtime.
3. Export the workload sensitivity records and tables.

## Procedure 4. Resource Sensitivity Analysis

**Input**

- Candidate values for the number of ACRs.
- Candidate values for the number of picking stations.
- Fixed layout and workload parameters.
- Objective type.

**Output**

- Resource sensitivity tables.

**Steps**

1. Analyze the number of ACRs:
   1. For each candidate robot count:
      1. keep all other parameters fixed;
      2. generate the instance;
      3. solve the instance using DRL-HALNS;
      4. record objective value and runtime.
2. Analyze the number of picking stations:
   1. For each candidate station count:
      1. keep all other parameters fixed;
      2. generate the instance;
      3. solve the instance using DRL-HALNS;
      4. record objective value and runtime.
3. Export the resource sensitivity records and tables.

## Procedure 5. Sensitivity Analysis Under Both Objectives

**Input**

- Sensitivity grid.
- Objective set `{makespan, TCT}`.
- Algorithm parameters.

**Output**

- Combined sensitivity result tables.

**Steps**

1. For each objective type:
   1. run the layout sensitivity analysis;
   2. run the workload sensitivity analysis;
   3. run the resource sensitivity analysis.
2. Combine all sensitivity records into one computational record file.
3. Export one spreadsheet file containing:
   1. makespan sensitivity tables;
   2. TCT sensitivity tables.
4. Return the combined sensitivity output.
