---
title : "Validate DynamoDB, indexes, and PITR"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

### Goal
This section validates TaskManager DynamoDB tables, query indexes, and Point-in-Time Recovery (PITR) configuration.

### Check DynamoDB tables
1. Open the Amazon DynamoDB console.
2. Choose Tables.
3. Review the tables with the `TaskManager` prefix.

![alt text](image.png)

Main tables:

- `TaskManager-ActivityLogs-dev`
- `TaskManager-Boards-dev`
- `TaskManager-Notifications-dev`
- `TaskManager-Tasks-dev`
- `TaskManager-Users-dev`

The tables are **Active** and use **On-demand** capacity mode, which is suitable for small or unpredictable workloads because read/write capacity does not need to be configured manually.

### Check Global Secondary Index

Open the `TaskManager-Users-dev` table and choose the **Indexes** tab.

![alt text](image-1.png)

The `TaskManager-Users-dev` table has this GSI:

- Index name: `EmailIndex`
- Partition key: `email`
- Status: `Active`
- Capacity: On-demand
This index allows the system to find users by email efficiently without scanning the entire table.

### Check PITR on important tables

Point-in-Time Recovery allows DynamoDB to keep continuous backups for up to 35 days. This is useful when data is accidentally updated or deleted.

*Enable PITR*

![alt text](image-2.png)

The **Edit point-in-time recovery** settings page shows PITR enabled with a 35-day backup recovery period.

*Board table*

![alt text](image-3.png)

`TaskManager-Boards-dev`:

- Partition key: `boardId`
- Capacity mode: On-demand
- Table status: Active
- PITR: On

*Task table*

![alt text](image-4.png)

`TaskManager-Tasks-dev`:

- Partition key: `boardId`
- Sort key: `taskId`
- Capacity mode: On-demand
- Table status: Active
- PITR: On

*User table*

![alt text](image-5.png)

`TaskManager-Users-dev`:

- Partition key: `userId`
- Capacity mode: On-demand
- Table status: Active
- PITR: On

### Conclusion

DynamoDB is configured appropriately for TaskManager: tables are separated by domain, an email lookup index is available, on-demand capacity is enabled, and PITR protects important data tables.

