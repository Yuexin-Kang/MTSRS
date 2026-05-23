# Computational Records

Author: Yuexin Kang

This folder contains cleaned computational records organized by experiment type. The records provide machine-readable numerical details for the main comparison experiments, total-completion-time experiments, sensitivity analyses, and emergency-reservation experiments.

## Files

| File | Description |
|---|---|
| `makespan_computational_records.csv` | Cleaned instance-group records for the main comparison experiments under the makespan objective. |
| `tct_computational_records.csv` | Cleaned instance-group records for the main comparison experiments under the total-completion-time objective. |
| `sensitivity_computational_records.csv` | Cleaned scenario records for the sensitivity analyses under the makespan and total-completion-time objectives. |
| `emergency_reservation_computational_records.csv` | Cleaned configuration-level records for the emergency-reservation experiments. |

## Record Levels

The column `record_level` indicates the granularity of each file.

- `tabulated_instance_group_record`: one row corresponds to an instance group reported in an experimental table.
- `tabulated_scenario_record`: one row corresponds to a sensitivity-analysis scenario.
- `cleaned_configuration_record`: one row corresponds to a cleaned emergency-reservation configuration record.

The files are intended to support transparent inspection of the numerical experiments and to provide machine-readable inputs for further analysis.
