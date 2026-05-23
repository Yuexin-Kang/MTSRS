# Generated Instances

Author: Yuexin Kang

This folder contains compact generated instance records for the numerical study. The instance files are organized by experiment type and stored in JSON format. Each JSON file contains the instance metadata, warehouse layout parameters, task and node records, robot start positions, order-tote requirement matrix, conveyor-side distances, and tote-level station processing times.

## Folder Structure

```text
main_experiments/
  small/
  medium/
  large/

drl_alns_comparison/

sensitivity_experiments/

emergency_reservation/
  regular_instances/
```

## File Format

Each generated instance file contains the following sections:

| Section | Description |
|---|---|
| `metadata` | Instance ID, scale group, seed, system size, station capacity, robot capacity, robot speed, and conveyor speed. |
| `map` | Warehouse layout parameters and counts of location types. |
| `node_sets` | Node-index sets for storage pickup, conveyor-inlet drop-off, conveyor-outlet pickup, return-to-storage, and robot start nodes. |
| `tasks` | Tote storage locations, storage levels, ready times, and due times. |
| `robots` | Initial robot start locations. |
| `nodes` | Node-level records with node type, location, time-window bounds, demand, and service time. |
| `order_tote_requirements` | The order-tote requirement matrix `IO`, where `IO[i][o] = 1` means tote `i` is required by order `o`. |
| `conveyor` | Inlet-to-station and station-to-outlet distance arrays. |
| `station_processing` | Tote-level station processing times and their maximum value. |

## Travel Distances and Travel Times

The full travel-distance and travel-time matrices are not stored in the JSON files to keep the repository compact. They can be reconstructed from the warehouse map parameters and node positions using the distance rule described in `pseudocode/01_instance_generation.md`. Travel time is obtained by dividing travel distance by robot speed.

## Manifest

The file `generated_instances_manifest.csv` lists all exported generated instance files and their main parameters.
