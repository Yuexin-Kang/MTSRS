# Representative Instances

Author: Yuexin Kang

This folder provides instance files for the main comparison experiments. The files are organized by scale group: small, medium, and large.

Each instance file contains the order-tote requirement matrix, conveyor-side parameters, station processing times, system parameters, layout information, task records, robot records, and node records. The files are provided in JSON format for readability and machine-readable use.

The files are intended to support understanding and reuse of the computational setting for the main comparison experiments.

## Folder Structure

```text
representative_instances/
  manifest.csv
  small/
  medium/
  large/
```

## Files

- `manifest.csv` lists the instance files and their main parameters.
- `small/`, `medium/`, and `large/` contain instance files for the main comparison experiments.

## Instance Fields

Each JSON file includes the following sections:

| Section | Description |
|---|---|
| `metadata` | Instance identifier, scale group, seed, system size, and main system parameters. |
| `map` | Warehouse layout information. |
| `node_sets` | Task-node sets used in the order-tote-robot coordination model. |
| `tasks` | Tote-level storage pickup task records. |
| `robots` | ACR start-position records. |
| `nodes` | Robot task-node records. |
| `order_tote_requirements` | Order-tote requirement matrix and source order-index information. |
| `conveyor` | Conveyor-side distance parameters. |
| `station_processing` | Tote-level station processing times. |
| `system_parameters` | Key system parameters used in the experiments. |
| `reconstruction_notes` | Notes on reconstructing travel-distance and travel-time matrices from map and node data. |

Only order indices associated with at least one required tote are included in the displayed order-tote matrix. The field `source_scale_orders_num` records the order-index count specified by the instance scale, and `orders_num_in_file` records the number of orders included in the displayed matrix.
