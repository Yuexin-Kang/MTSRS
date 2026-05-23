# Instance Generation

Author: Yuexin Kang

## Overview

Each computational instance represents a wave-level order fulfillment problem in a multi-tote storage and retrieval system with sequential picking stations. An instance contains warehouse layout information, required totes, released orders, ACRs, picking stations, order-tote requirement relationships, conveyor-side distances, station processing times, and robot-storage-side travel and service data.

The instance-generation protocol has two levels. The first level defines the instance scale, including the warehouse dimensions and the numbers of totes, orders, robots, and picking stations. The second level constructs the operational instance, including order-tote requirements, conveyor distances, station processing times, storage locations, robot task nodes, time-window bounds, and service times. Travel-distance and travel-time matrices are reconstructable from the exported map and node data.

## Instance Metadata

Each generated instance is identified by the following fields:

| Field | Description |
|---|---|
| `instance_id` | Instance identifier. |
| `scale_group` | Scale group, such as small, medium, large, sensitivity, or emergency-reservation. |
| `seed` | Random seed used in instance generation. |
| `w_num` | Width index of the warehouse layout. |
| `l_num` | Length index of the warehouse layout. |
| `n` | Number of required totes. |
| `orders_num` | Number of released orders. |
| `robot_num` | Number of ACRs. |
| `picking_station_num` | Number of sequential picking stations. |

## Warehouse Layout

The warehouse layout is determined by the width and length indices `w_num` and `l_num`. The rack width and rack length are computed from these indices using fixed block-size parameters. The rack height is fixed across experiments.

The layout information is used to generate storage locations, robot task nodes, travel distances, and travel times. Robot acceleration, congestion, collision avoidance, and deadlock prevention are not modeled in the instance-generation layer.

## Order-Tote Requirement Matrix

The order-tote requirement matrix is denoted by `IO`. The entry `IO[i][o]` equals 1 if tote `i` contains items required by order `o`, and 0 otherwise.

For each tote, the generator randomly assigns the tote to one or two released orders under a fixed random seed. The resulting matrix defines the deterministic order-tote requirement parameter used by the optimization model and the heuristic algorithms.

## Conveyor-Side Parameters

The conveyor-side parameters include:

| Field | Description |
|---|---|
| `Dip` | Distance from the conveyor inlet to each picking station. |
| `Dpi` | Distance from each picking station to the conveyor outlet. |
| `conveyor_speed` | Conveyor travel speed. |
| `distance_between_pickers` | Distance between adjacent picking stations. |
| `conveyor_length` | Total conveyor length used to compute downstream distances. |

The conveyor-side data are used to compute tote arrival times at sequential picking stations and tote arrival times at the conveyor outlet.

## Station Processing Times

Each tote has a station-level processing time `p_i`. The processing time is generated independently for each tote and affects station queues, tote completion times, order completion times, makespan, and total completion time.

The maximum station processing time `p_max` is used to define a conservative virtual waiting bound for station buffer congestion.

## Robot-Storage-Side Data

The robot-storage-side data include:

| Field | Description |
|---|---|
| `VSP` | Storage pickup task nodes. |
| `VIN` | Conveyor-inlet drop-off task nodes. |
| `VOUT` | Conveyor-outlet pickup task nodes. |
| `VSR` | Return-to-storage task nodes. |
| `W` | Robot start nodes. |
| `serviceTime` | Service time at robot task nodes. |
| `readyTime` | Earliest service-completion time at task nodes, if applicable. |
| `dueTime` | Latest service-completion time at task nodes, if applicable. |
| `disMatrix` | Travel-distance matrix. |
| `timeMatrix` | Travel-time matrix. |
| `demand` | Load-change vector for pickup and drop-off operations. |

These fields define the robot routing and scheduling layer of each instance.

## Generated Instance Files

Generated instance files are stored under `data/generated_instances/`. The instance records are provided in compact JSON format, with a manifest file that lists all exported instances. Each generated instance includes metadata, order-tote requirements, conveyor parameters, station processing times, and robot-storage-side node data. Large travel-distance and travel-time matrices are not stored directly, because they can be reconstructed from the exported map parameters and node locations.
