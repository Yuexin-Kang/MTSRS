# 01. Instance Generation Pseudocode

Author: Yuexin Kang

This file provides implementation-oriented pseudocode for generating the computational instances used in the numerical study. The procedures describe how to construct instance scales, generate a full MTS/RS instance, and export generated data in machine-readable form.

## Procedure 1. Generate Main Instance Scale Tables

**Input**

- Scale group `G`, where `G` is one of `small`, `medium`, or `large`.
- Instance definitions for the selected scale group.

**Output**

- A table of instance-scale records.

**Steps**

1. Initialize an empty table `ScaleTable`.
2. For each instance ID in the selected scale group:
   1. Set the warehouse layout indices `w_num` and `l_num`.
   2. Set the number of required totes `n`.
   3. Set the number of released orders `orders_num`.
   4. Set the number of available ACRs `robot_num`.
   5. Set the number of picking stations `picking_station_num`.
   6. Create one record with the fields:
      - `instance_id`,
      - `scale_group`,
      - `w_num`,
      - `l_num`,
      - `n`,
      - `orders_num`,
      - `robot_num`,
      - `picking_station_num`.
   7. Append the record to `ScaleTable`.
3. Return `ScaleTable`.

## Procedure 2. Generate One Full Computational Instance

**Input**

- Warehouse layout indices `w_num` and `l_num`.
- Number of required totes `n`.
- Number of released orders `orders_num`.
- Number of available ACRs `robot_num`.
- Number of picking stations `picking_station_num`.
- Random seed `seed`.
- System parameters:
  - robot speed,
  - robot tote capacity,
  - conveyor speed,
  - order-cache capacity,
  - station queue bound,
  - storage handling-time rule,
  - inlet and outlet handling times,
  - station processing-time distribution.

**Output**

- A full computational instance containing layout, order, tote, station, conveyor, and ACR information.

**Steps**

1. Set the random seed for all random components.
2. Construct the warehouse layout:
   1. Compute rack width from `w_num`.
   2. Compute rack length from `l_num`.
   3. Set rack height.
   4. Generate storage locations and storage levels for required totes.
3. Construct the tote-related task-node sets:
   1. For each tote `i`, create a storage pickup node.
   2. Create the corresponding conveyor-inlet drop-off node.
   3. Create the corresponding conveyor-outlet pickup node.
   4. Create the corresponding return-to-storage node.
4. Construct the ACR set:
   1. Create `robot_num` ACRs.
   2. Assign each ACR an initial start node.
   3. Set the robot tote capacity.
   4. Set the robot travel speed.
5. Generate robot-storage-side parameters:
   1. Compute the travel distance between all admissible node pairs.
   2. Convert each travel distance to travel time using the robot speed.
   3. Generate service times for storage pickup, inlet drop-off, outlet pickup, and return-to-storage operations.
   4. Generate ready times and time-window bounds where applicable.
   5. Generate load-change values for pickup and drop-off nodes.
6. Generate the order-tote requirement matrix:
   1. Initialize an `n × orders_num` binary matrix `IO` with all entries equal to 0.
   2. For each tote `i = 1, ..., n`:
      1. Randomly sample the number of associated orders `c_i` from `{1, 2}`.
      2. Repeat `c_i` times:
         1. Randomly sample an order `o` from the released order set.
         2. Set `IO[i, o] = 1`.
   3. Identify the active order indices associated with at least one required tote.
   4. Record the active order indices as `source_order_indices` for export.
   5. Compute `sumIO` as the total number of nonzero entries in `IO`.
7. Generate conveyor-side parameters:
   1. Set the distance between consecutive picking stations.
   2. Set conveyor length.
   3. For each picking station `p`:
      1. Compute the distance from the conveyor inlet to station `p`.
      2. Compute the distance from station `p` to the conveyor outlet.
   4. Store the inlet-to-station distances in `Dip`.
   5. Store the station-to-outlet distances in `Dpi`.
8. Generate station processing times:
   1. For each tote `i`, sample a station processing time from the specified distribution.
   2. Round the sampled value.
   3. Enforce a positive lower bound.
   4. Store the result as `p_i`.
   5. Compute `p_max = max_i p_i`.
9. Assemble the full instance with the following components:
   1. metadata,
   2. warehouse layout,
   3. task-node sets,
   4. order-tote requirement matrix,
   5. conveyor-side parameters,
   6. station processing times,
   7. robot-storage-side travel and service data.
10. Return the full computational instance.

## Procedure 3. Generate Instances for One Experiment

**Input**

- Instance scale table.
- Seed list.
- Experiment type.

**Output**

- A collection of generated instances.

**Steps**

1. Initialize an empty instance collection `InstanceSet`.
2. For each row in the instance scale table:
   1. Read `instance_id`, `w_num`, `l_num`, `n`, `orders_num`, `robot_num`, and `picking_station_num`.
   2. For each seed in the seed list:
      1. Generate one full computational instance using Procedure 2.
      2. Assign a unique identifier to the generated instance.
      3. Append the instance to `InstanceSet`.
3. Return `InstanceSet`.

## Procedure 4. Export Instance Files

**Input**

- Generated instance collection.
- Output folder.

**Output**

- Machine-readable exported instance files.

**Steps**

1. For each generated instance:
   1. Create a folder or file name based on `instance_id` and `seed`.
   2. Export metadata, including scale group, layout indices, number of required totes, source-scale order count, number of active orders included in the file, number of ACRs, number of picking stations, and seed.
   3. Export the active-order order-tote requirement matrix `IO`, together with `source_order_indices`.
   4. Export the conveyor-distance matrices `Dip` and `Dpi`.
   5. Export station processing times `p_i` and `p_max`.
   6. Export robot-storage-side node and service data, including task nodes, robot start nodes, service times, ready times, time-window bounds where applicable, and load-change values.
   7. Record reconstruction notes for large travel-distance and travel-time matrices that can be reconstructed from the exported map parameters, node locations, robot speed, and service-time information.
   8. Export parameter settings used to generate the instance.
2. Store the exported instance files under the folder associated with the experiment type.
3. Return the list of exported files.
