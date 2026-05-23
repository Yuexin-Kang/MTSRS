# 07. Emergency Reservation Workflow Pseudocode

Author: Yuexin Kang

This file describes the workflow for evaluating resource reservation for emergency orders in an MTS/RS with sequential picking stations. The experiment compares a no-reserve baseline with reserved-resource emergency handling.

## Procedure 1. Generate Regular and Emergency Inputs

**Input**

- Regular instance scale records.
- Regular seed list.
- Emergency seed list or emergency replication count.
- Emergency tote count.
- Emergency batch-size values.

**Output**

- Regular-order instances.
- Emergency-order data.

**Steps**

1. For each regular instance scale record:
   1. For each regular seed:
      1. generate the regular-order computational instance;
      2. store the regular instance metadata and generated data.
2. For each emergency replication:
   1. generate emergency totes or emergency order-tote requirements;
   2. set emergency batch size;
   3. store emergency instance metadata.
3. Return regular and emergency inputs.

## Procedure 2. Solve the No-Reserve Regular Plan

**Input**

- Regular-order instance.
- Full robot fleet.
- Full station-buffer capacity.
- Objective type.

**Output**

- No-reserve regular schedule.
- Regular makespan and TCT.

**Steps**

1. Use all available robots.
2. Use the full station-buffer capacity.
3. Solve the regular-order scheduling problem.
4. Record:
   1. regular makespan under no reservation;
   2. regular TCT under no reservation;
   3. station schedule tail times;
   4. robot schedule information if needed for evaluation.
5. Return the no-reserve regular schedule.

## Procedure 3. Solve the Reserve Regular Plan

**Input**

- Regular-order instance.
- Total number of robots.
- Total station-buffer capacity.
- Number of reserved robots.
- Reserved station-buffer capacity.
- Objective type.

**Output**

- Reserve regular schedule.
- Regular cost of reservation.

**Steps**

1. Compute regular robot count as total robots minus reserved robots.
2. Compute regular station-buffer capacity as total buffer capacity minus reserved buffer capacity.
3. Solve the regular-order scheduling problem using only regular resources.
4. Record:
   1. regular makespan under reservation;
   2. regular TCT under reservation.
5. Compute regular makespan cost:
   1. subtract no-reserve regular makespan from reserve regular makespan.
6. Compute regular TCT cost if TCT is evaluated:
   1. subtract no-reserve regular TCT from reserve regular TCT.
7. Return the reserve regular schedule and regular cost metrics.

## Procedure 4. Evaluate the Append-to-Station Emergency Baseline

**Input**

- No-reserve regular schedule.
- Emergency-order data.
- Emergency arrival time.

**Output**

- Baseline response delay.
- Baseline completion time.
- Baseline waiting indicator.

**Steps**

1. Identify the station to which each emergency order is appended.
2. Find the tail time of the selected station in the no-reserve regular schedule.
3. Set the emergency start time as the maximum of:
   1. emergency arrival time;
   2. selected station tail time.
4. Compute baseline response delay as emergency start time minus emergency arrival time.
5. Insert emergency orders at the end of the station schedule.
6. Estimate or compute emergency completion time under the append-to-station rule.
7. Set the baseline waiting indicator to 1 if the emergency arrival time is earlier than the selected station tail time; otherwise set it to 0.
8. Return baseline emergency performance metrics.

## Procedure 5. Evaluate Reserved-Resource Emergency Handling

**Input**

- Reserve resource setting.
- Emergency-order data.
- Emergency arrival time.
- Reserved robots.
- Reserved station-buffer capacity.

**Output**

- Reserved response delay.
- Reserved completion time.

**Steps**

1. Allow emergency processing to start at the emergency arrival time if reserved resources are available.
2. Use only reserved robots for emergency tote retrieval and return.
3. Use only reserved station-buffer capacity for emergency station processing.
4. Construct an emergency service schedule for the emergency batch.
5. Compute reserved response delay.
6. Compute reserved completion time.
7. Return reserved emergency performance metrics.

## Procedure 6. Run the Full Emergency-Reservation Experiment

**Input**

- Regular instance records.
- Regular seed list.
- Reserved robot values.
- Reserved buffer values.
- Emergency tote count.
- Emergency batch-size values.
- Emergency arrival-time ratios.
- Emergency replication setting.

**Output**

- Emergency-reservation computational records.
- Emergency-reservation experimental tables.

**Steps**

1. Initialize an empty computational record table.
2. For each regular instance and regular seed:
   1. generate the regular-order instance;
   2. solve the no-reserve regular plan using Procedure 2.
3. For each reserved-resource setting:
   1. solve the reserve regular plan using Procedure 3.
4. For each emergency arrival-time ratio:
   1. compute emergency arrival time as the selected ratio times the no-reserve regular makespan.
5. For each emergency batch size:
   1. generate or read emergency-order data;
   2. evaluate the append-to-station baseline using Procedure 4;
   3. evaluate reserved-resource handling using Procedure 5;
   4. compute response benefit as baseline response delay minus reserved response delay;
   5. compute completion benefit as baseline completion time minus reserved completion time;
   6. create a computational record with all regular, emergency, and reservation metrics;
   7. append the record to the table.
6. Aggregate records by experimental factors to produce summary tables.
7. Export computational records and tabulated results.

## Procedure 7. Analyze When Reservation Helps

**Input**

- Emergency-reservation computational records.

**Output**

- Conditional summary table.

**Steps**

1. Split records by baseline waiting indicator:
   1. no baseline station-queue waiting;
   2. positive baseline station-queue waiting.
2. For each group:
   1. compute the share of records in the group;
   2. compute mean baseline waiting time;
   3. compute mean response benefit;
   4. compute mean completion benefit;
   5. compute mean baseline response delay;
   6. compute mean reserved response delay;
   7. compute mean baseline completion time;
   8. compute mean reserved completion time;
   9. count the number of records.
3. Export the conditional summary table.

## Procedure 8. Generate Emergency-Reservation Result Tables

**Input**

- Emergency-reservation computational records.

**Output**

- Emergency-reservation experimental tables.

**Steps**

1. Compute overall expected regular cost and emergency benefit.
2. Compute the effect of reserved robots.
3. Compute the effect of reserved station-buffer capacity.
4. Compute the effect of emergency arrival time.
5. Compute the effect of emergency batch size.
6. Compute joint effects between:
   1. reserved robots and emergency arrival time;
   2. reserved robots and emergency batch size;
   3. emergency arrival time and emergency batch size.
7. Compute conditional results by baseline station-queue waiting.
8. Export all summary tables to the emergency-reservation result workbook.
