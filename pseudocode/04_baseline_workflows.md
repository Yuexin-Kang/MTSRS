# 04. Baseline Workflow Pseudocode

Author: Yuexin Kang

This file describes the rule-based and greedy stage-wise baseline workflows used for comparison with the integrated decision method. The purpose of these baselines is to evaluate the value of joint order-tote-robot coordination relative to sequential or decoupled decision rules.

## Procedure 1. Rule-Based Stage-Wise Baseline

**Input**

- Full computational instance.
- Objective type.
- Fixed dispatching and assignment rules.

**Output**

- Feasible stage-wise solution.
- Objective value.
- Runtime.

**Steps**

1. Initialize an empty solution.
2. Stage 1: assign orders to picking stations using a fixed rule:
   1. process orders in a predetermined order;
   2. select a feasible station according to the rule;
   3. update station load and assigned order set.
3. Stage 2: determine tote activation and station visits:
   1. for each assigned order, identify required totes from the order-tote matrix;
   2. activate each required tote at the station to which the order is assigned;
   3. remove unnecessary tote-station visits.
4. Stage 3: construct ACR routes and schedules:
   1. create pickup and delivery task pairs for each required tote;
   2. assign task pairs to ACRs according to the fixed dispatching rule;
   3. enforce pickup-before-delivery precedence;
   4. enforce robot capacity constraints;
   5. compute task completion times.
5. Evaluate station processing and conveyor movement:
   1. compute conveyor-entry times;
   2. propagate tote arrival times along sequential picking stations;
   3. compute station service start and completion times under FCFS processing.
6. Compute order completion times.
7. Evaluate the objective value.
8. Record runtime and return the result.

## Procedure 2. Greedy Stage-Wise Baseline

**Input**

- Full computational instance.
- Objective type.
- Greedy assignment and routing rules.

**Output**

- Feasible greedy solution.
- Objective value.
- Runtime.

**Steps**

1. Initialize an empty solution.
2. Stage 1: greedily assign orders to stations:
   1. maintain the tote set currently associated with each station;
   2. for each unassigned order, compute its overlap with each station tote set;
   3. choose the feasible station with the largest overlap or smallest incremental burden;
   4. update the station assignment and tote set.
3. Stage 2: greedily activate tote-station visits:
   1. for each station, identify all totes required by assigned orders;
   2. activate only those tote-station visits that serve at least one order;
   3. determine a preliminary tote sequence according to greedy timing or station-load information.
4. Stage 3: greedily build ACR routes:
   1. initialize each ACR route at its start node;
   2. maintain unserved pickup tasks and available delivery tasks;
   3. at each step, choose the feasible next task with the smallest immediate travel-time or completion-time increase;
   4. update robot load and route state;
   5. repeat until all pickup and delivery tasks are assigned.
5. Propagate tote and order timing through the conveyor and station layers.
6. Evaluate the objective value.
7. Record runtime and return the result.

## Procedure 3. Compare Integrated and Stage-Wise Methods

**Input**

- Instance set.
- Objective type.
- Integrated method results.
- Baseline method set.

**Output**

- Baseline comparison table.

**Steps**

1. Initialize an empty comparison table.
2. For each generated instance:
   1. Run or read the result of the integrated method.
   2. Run the rule-based baseline.
   3. Run the greedy baseline.
   4. Record objective values and runtimes.
3. For each baseline method:
   1. compute the improvement ratio of the integrated method relative to the baseline;
   2. store the improvement ratio in the comparison table.
4. If multiple seeds or replications are used:
   1. aggregate objective values and runtimes by instance ID;
   2. aggregate improvement ratios by instance ID.
5. Export the comparison table.

## Procedure 4. Record Baseline Results

**Input**

- Baseline solution.
- Instance metadata.
- Objective type.

**Output**

- One computational record.

**Steps**

1. Record instance metadata:
   1. instance ID,
   2. scale group,
   3. seed,
   4. warehouse layout,
   5. number of totes,
   6. number of orders,
   7. number of ACRs,
   8. number of picking stations.
2. Record algorithm metadata:
   1. method name,
   2. objective type,
   3. baseline type.
3. Record numerical outcomes:
   1. objective value,
   2. runtime,
   3. feasibility status.
4. Return the computational record.
