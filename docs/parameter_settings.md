# Parameter Settings

Author: Yuexin Kang

## System Parameters

The numerical experiments use normalized travel and service-time units. The following parameters define the MTS/RS layout, robot operations, conveyor movement, station processing, and algorithmic settings.

| Category | Parameter | Symbol or Field | Value or Rule |
|---|---|---|---|
| Warehouse layout | Width index | `w_num` | Varies by instance. |
| Warehouse layout | Length index | `l_num` | Varies by instance. |
| Warehouse layout | Rack height | `H_SR` | 10 levels. |
| Robot travel | Robot speed | `v_x` | 1 distance unit per time unit. |
| Conveyor travel | Conveyor speed | `v` | 0.25 distance units per time unit. |
| Conveyor layout | Distance between adjacent picking stations | `distance_between_pickers` | 5 distance units. |
| Conveyor layout | Conveyor length | `conveyor_length` | 105 distance units. |
| Station capacity | Maximum active orders per station | `b1` | 5. |
| Station buffer | Virtual FCFS waiting bound parameter | `b2` | 8. |
| Robot capacity | Tote capacity per ACR | `Q` | 8. |
| Entry handling | Conveyor-entry handling time | `t_io` | 1 time unit. |
| Exit handling | Conveyor-exit handling time | `t_io` | 1 time unit. |
| Storage handling | Storage pickup and return handling | `t_sto(h)` | Proportional to storage level `h`. |
| Station processing | Tote-level processing time | `p_i` | Generated independently for each tote. |

## Station Processing Time Generation

For each tote `i`, a station-level processing time is generated as:

```text
p_i = max(1, round(X_i)),  X_i ~ Normal(10, 2^2).
```

The generated vector `p_i` affects tote processing, station queues, order completion times, makespan, and total completion time.

## Objective Functions

Two objectives are evaluated:

| Objective | Description |
|---|---|
| Makespan | Minimize the maximum order completion time. |
| Total completion time | Minimize the sum of order completion times. |

The main experiments use makespan as the baseline objective. Additional experiments evaluate total completion time using the same modeling and experimental framework.

## MILP Solver Environment

| Item | Setting |
|---|---|
| Operating system | Windows. |
| CPU | 2.90 GHz. |
| Memory | 16 GB RAM. |
| MILP solver | Gurobi 10.0.1. |

## DRL-HALNS Settings

| Item | Setting |
|---|---|
| Search framework | DRL-guided hybrid adaptive large neighborhood search. |
| Operator-selection model | Multilayer perceptron policy. |
| Reinforcement learning algorithm | Proximal policy optimization. |
| PPO implementation | Stable Baselines3 PPO 2.2.1. |
| Action space | Five feasible destroy-and-repair operator pairs. |
| State dimension | 13. |
| Reward weights | New best: 10; incumbent improvement: 1; accepted non-improving move: 0.01; rejected move: 0. |
| Initial SA temperature | 0.01. |
| Cooling interval | 30 iterations. |
| Cooling rate | 0.97. |
| Minimum temperature | 1e-10. |
| Training iteration budget | 10000. |
| Evaluation iteration budget | 10000. |
| Sensitivity analysis iteration budget | 100000. |

## Emergency-Reservation Settings

The emergency-reservation experiments evaluate the impact of reserving robots and station-buffer capacity for emergency orders. The key parameters include:

| Parameter | Values |
|---|---|
| Reserved robots | 0, 1, 2. |
| Reserved station-buffer capacity | 0, 1, 2. |
| Emergency tote count | 5. |
| Emergency batch size | 1, 3, 5, 10. |
| Emergency arrival-time ratio | 0.10, 0.30, 0.50. |
| Objective | Makespan. |
