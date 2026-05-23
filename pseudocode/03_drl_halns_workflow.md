# 03. DRL-HALNS Workflow Pseudocode

Author: Yuexin Kang

This file provides implementation-oriented pseudocode for the DRL-guided hybrid adaptive large neighborhood search framework. The workflow includes initial solution construction, operator-pair selection, destroy-and-repair moves, simulated annealing acceptance, best-solution updates, rollout generation, and PPO training.

## Procedure 1. Construct an Initial Solution

**Input**

- Full computational instance.
- Order set.
- Picking-station set.
- ACR set.
- Required totes.
- Travel distances and service times.
- Robot capacity.

**Output**

- Initial feasible solution `X0 = (X_pick, X_assign)`.

**Steps**

1. Construct the order-to-station assignment component `X_assign`:
   1. Select initial seed orders for picking stations.
   2. Initialize station loads and station tote sets.
   3. For each unassigned order:
      1. compute its tote-overlap similarity with each station tote set;
      2. identify feasible stations under order-cache constraints;
      3. assign the order to the station with the largest similarity;
      4. break ties by smaller station load;
      5. update station tote set and station load.
2. Construct the ACR route component `X_pick`:
   1. Initialize one route for each ACR at its start node.
   2. Initialize unserved pickup tasks.
   3. Initialize available delivery tasks as empty.
   4. Repeatedly select feasible next tasks for each ACR:
      1. allow a pickup if capacity is available;
      2. allow a delivery only if its paired pickup has already been visited by the same ACR;
      3. choose the nearest feasible next node;
      4. update route sequence and robot load;
      5. update unserved pickup and available delivery sets.
3. Combine `X_pick` and `X_assign` to form `X0`.
4. Evaluate `X0` under the selected objective.
5. Return `X0`.

## Procedure 2. Solve One Instance with DRL-HALNS

**Input**

- Full computational instance.
- Objective type.
- Feasible destroy-and-repair operator-pair set `A`.
- Trained policy `pi_theta`.
- Iteration limit `t_max`.
- Simulated annealing parameters.
- Flag `Record` indicating whether to store rollout transitions.

**Output**

- Best solution found.
- Best objective value.
- Runtime.
- Optional transition buffer.

**Steps**

1. Generate the initial solution using Procedure 1.
2. Set the incumbent solution `X` to the initial solution.
3. Set the best-so-far solution `X_best` to the initial solution.
4. Initialize simulated annealing temperature.
5. Initialize iteration counter.
6. Initialize transition buffer `B` as empty.
7. Repeat until the iteration counter reaches `t_max`:
   1. Construct the state vector from:
      1. recent incumbent objective change,
      2. objective gap between incumbent and best-so-far solutions,
      3. best-so-far objective value,
      4. current simulated annealing temperature,
      5. cooling rate,
      6. number of consecutive non-improving iterations,
      7. iteration index,
      8. incumbent-improvement indicator,
      9. one-hot encoding of the previous action.
   2. Use `pi_theta` to sample one action from the feasible operator-pair set.
   3. Decode the action into one destroy-and-repair operator pair.
   4. Apply the destroy operator to the incumbent solution.
   5. Apply the paired repair operator to construct a feasible candidate solution.
   6. If repair fails to produce a feasible candidate, discard the candidate and continue to the next iteration.
   7. Evaluate the candidate solution under the selected objective.
   8. Compute the objective change relative to the incumbent.
   9. Apply the simulated annealing acceptance rule:
      1. accept the candidate if it improves the incumbent;
      2. otherwise accept it with the probability determined by relative deterioration and current temperature.
   10. If `Record = 1`, compute the reward for the selected operator pair:
       1. assign the highest reward if the candidate improves the best-so-far solution;
       2. assign a positive reward if the candidate improves the incumbent;
       3. assign a small reward if the candidate is accepted but does not improve the incumbent;
       4. assign zero reward if the candidate is rejected.
   11. If the candidate improves the best-so-far solution, update `X_best`.
   12. If the candidate is accepted, update the incumbent solution `X`.
   13. Update the simulated annealing temperature according to the cooling schedule.
   14. If `Record = 1`, construct the next state and append `(state, action, reward, next_state)` to `B`.
   15. Update the iteration counter.
8. Return `X_best`, its objective value, runtime, and `B` if recorded.

## Procedure 3. Apply a Picking Destroy Operator

**Input**

- Current picking-route component `X_pick`.
- Destroy type, either random or greedy.
- Number of pickup-delivery pairs to remove.

**Output**

- Partial picking-route solution.
- Removed pickup-delivery pairs.

**Steps**

1. Identify removable pickup-delivery pairs.
2. If the destroy type is random:
   1. sample the required number of pairs uniformly at random.
3. If the destroy type is greedy:
   1. estimate the removal saving for each removable pair;
   2. rank candidate pairs by estimated saving;
   3. select the pairs with the largest estimated savings.
4. Remove the selected pairs from their current routes.
5. Return the partial route solution and removed pairs.

## Procedure 4. Apply a Picking Repair Operator

**Input**

- Partial picking-route solution.
- Removed pickup-delivery pairs.
- Repair type, either random or greedy.
- Robot capacity and feasibility rules.

**Output**

- Repaired picking-route solution.

**Steps**

1. For each removed pickup-delivery pair:
   1. Enumerate feasible routes and insertion positions.
   2. Retain only insertions satisfying:
      1. fixed route-start condition,
      2. pickup-before-delivery precedence,
      3. same-route pairing,
      4. robot capacity limits.
   3. If no feasible insertion exists, declare repair failure.
   4. If the repair type is random, sample one feasible insertion.
   5. If the repair type is greedy, choose the insertion with the smallest objective increase.
   6. Insert the pair into the selected route.
2. Return the repaired picking-route solution.

## Procedure 5. Apply an Order-to-Station Assignment Move

**Input**

- Current order-to-station assignment component `X_assign`.
- Number of orders to remove.
- Station feasibility rules.

**Output**

- Repaired order-to-station assignment component.

**Steps**

1. Select a subset of orders to remove from their current stations.
2. Temporarily mark these orders as unassigned.
3. For each removed order:
   1. identify feasible stations;
   2. assign the order to one feasible station using the assignment repair rule;
   3. update station loads and station tote sets.
4. Verify that each order is assigned to exactly one station.
5. Verify that station-capacity rules are respected where applicable.
6. Return the repaired assignment component.

## Procedure 6. Train the Operator-Selection Policy

**Input**

- Training instance set.
- Initial policy network.
- PPO parameters.
- DRL-HALNS parameters.

**Output**

- Trained operator-selection policy.

**Steps**

1. Initialize policy parameters.
2. For each training epoch:
   1. Initialize an empty rollout buffer.
   2. For each training instance:
      1. Run Procedure 2 with `Record = 1`.
      2. Append all recorded transitions to the rollout buffer.
   3. Compute returns and advantage estimates from the rollout buffer.
   4. Update policy parameters using the PPO clipped objective.
3. Return the trained policy.

## Procedure 7. Evaluate the Trained DRL-HALNS Policy

**Input**

- Evaluation instance set.
- Trained policy.
- Objective type.
- Iteration limit.

**Output**

- Evaluation results.

**Steps**

1. Initialize an empty result table.
2. For each evaluation instance:
   1. Run Procedure 2 with `Record = 0`.
   2. Record best objective value, runtime, instance metadata, and algorithm settings.
3. Aggregate results by instance ID where multiple seeds or replications are used.
4. Return the evaluation result table.
