# 02. Integrated MILP Workflow Pseudocode

Author: Yuexin Kang

This file describes the implementation workflow for constructing and solving the integrated mixed-integer linear programming model for order-tote-robot coordination in an MTS/RS with sequential picking stations.

## Procedure 1. Prepare Model Inputs

**Input**

- A full computational instance.
- Objective type, either `makespan` or `TCT`.
- Solver parameters, including time limit and optimality-gap tolerance if applicable.

**Output**

- Model-ready sets, parameters, and objective settings.

**Steps**

1. Read the order set `O`.
2. Read the picking-station set `P`.
3. Read the ACR set `K`.
4. Read tote-related task-node sets:
   1. storage pickup nodes,
   2. inlet drop-off nodes,
   3. outlet pickup nodes,
   4. return-to-storage nodes.
5. Read robot start nodes and the virtual sink node.
6. Read the order-tote requirement matrix `OT`.
7. Read station and conveyor parameters:
   1. inlet-to-station distances,
   2. station-to-outlet distances,
   3. conveyor speed,
   4. tote-level station processing times,
   5. order-cache capacity,
   6. station queue bound.
8. Read robot and storage-side parameters:
   1. travel times,
   2. service times,
   3. time-window bounds,
   4. robot capacity,
   5. node load increments.
9. Compute valid big-M values and horizon bounds according to the model structure.
10. Return all model-ready data.

## Procedure 2. Build the Order Assignment and Sequencing Layer

**Input**

- Order set.
- Picking-station set.
- Order-tote requirement matrix.
- Order-cache capacity.
- Time bound for big-M constraints.

**Output**

- Order-layer variables and constraints.

**Steps**

1. Create binary assignment variable `z[o,p]` for each order and station.
2. Create order start-time variable `T_start_order[o]` for each order.
3. Create order completion-time variable `T_end_order[o]` for each order.
4. Create order-overlap and sequencing variables for pairs of orders.
5. Create first-tote selection variables that identify the first required tote processed for each order at its assigned station.
6. Add assignment constraints requiring each order to be assigned to exactly one station.
7. Add sequencing constraints that define order start-time relations.
8. Add overlap constraints that identify whether two orders are active at the same station at the same time.
9. Add order-cache capacity constraints limiting the number of concurrently active orders at a station.
10. Link each order start time to the start time of its selected first required tote.
11. Define each order completion time as the maximum completion time among its required totes at the assigned station.
12. Add the makespan variable and link it to all order completion times.
13. Return the order-layer variables and constraints.

## Procedure 3. Build the Tote Assignment and Sequencing Layer

**Input**

- Tote set.
- Picking-station set.
- Order assignment variables.
- Order-tote requirement matrix.
- Conveyor distances and speed.
- Station processing times.
- Station queue bound.

**Output**

- Tote-layer variables and constraints.

**Steps**

1. Create binary variable `y[i,p]` indicating whether tote `i` is processed at station `p`.
2. Create conveyor-entry time variable `I[i]` for each tote.
3. Create tote arrival-time, service-start-time, and service-completion-time variables for each tote and station.
4. Create FCFS ordering variables for tote pairs at each station.
5. Link tote activation to order assignment:
   1. if an order assigned to station `p` requires tote `i`, then tote `i` must be processed at station `p`;
   2. tote `i` can be processed at station `p` only if at least one order assigned to station `p` requires it.
6. Link conveyor-entry time to completion of the inlet drop-off operation.
7. Propagate tote arrival times along sequential picking stations.
8. Enforce station service-start times and completion times.
9. Enforce that a tote passes through a station without artificial waiting if it is not processed there.
10. Enforce tote-level station processing time when `y[i,p] = 1`.
11. Enforce FCFS-consistent station processing using pairwise ordering variables.
12. Enforce the station queue waiting bound.
13. Return the tote-layer variables and constraints.

## Procedure 4. Build the ACR Task Assignment and Scheduling Layer

**Input**

- ACR set.
- Task-node sets.
- Robot start nodes.
- Virtual sink node.
- Travel times.
- Service times.
- Time-window bounds.
- Load increments.
- Robot capacity.

**Output**

- ACR-layer variables and constraints.

**Steps**

1. Create route arc variable `x[i,j]` for each admissible directed arc.
2. Create robot-visit variable `u[k,i]` for each robot and task node.
3. Create robot-use variable `r[k]` for each robot.
4. Create robot load variable for each robot and node.
5. Create node service-completion time variable for each task node and robot start node.
6. Create subtour-elimination ordering variables for task nodes.
7. Assign each task node to exactly one robot.
8. Enforce that each task node has exactly one predecessor and one successor under the open-route structure.
9. Link each robot start node to the corresponding robot-use variable.
10. Enforce robot identity consistency along selected arcs.
11. Enforce pickup-drop-off pairing and return-flow pairing for each tote.
12. Add subtour-elimination constraints.
13. Propagate robot load along selected arcs.
14. Enforce robot capacity limits.
15. Propagate completion times along selected arcs.
16. Enforce within-tote precedence:
    1. storage pickup before inlet drop-off,
    2. outlet pickup before return-to-storage.
17. Enforce task-node time windows.
18. Link conveyor completion to the outlet pickup operation.
19. Return the ACR-layer variables and constraints.

## Procedure 5. Set Objective and Solve the Integrated Model

**Input**

- Order-layer constraints.
- Tote-layer constraints.
- ACR-layer constraints.
- Objective type.
- Solver parameters.

**Output**

- Objective value.
- Runtime.
- Feasibility status.
- Optimality status.
- Solution information when available.

**Steps**

1. Combine all variables and constraints into one integrated MILP model.
2. If objective type is `makespan`:
   1. minimize the maximum order completion time.
3. If objective type is `TCT`:
   1. minimize the sum of order completion times, or the weighted sum if order weights are specified.
4. Set solver parameters, including time limit and tolerance if applicable.
5. Solve the MILP model.
6. If a feasible incumbent solution is found:
   1. extract objective value;
   2. extract order assignments;
   3. extract tote-station processing decisions;
   4. extract ACR routes and task times;
   5. extract runtime, upper bound, lower bound, and gap if available.
7. If no feasible incumbent solution is found:
   1. record infeasibility or time-limit status;
   2. record available lower bound if reported by the solver.
8. Return all solution and solver-status information.
