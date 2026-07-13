---
title : "Workshop overview"
date : 2026-07-09 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Goal
In this workshop, you will validate the main components of the Task Management System deployed on AWS:

- Static frontend assets are uploaded to Amazon S3.
- Users are managed by an Amazon Cognito User Pool.
- The main API uses AWS AppSync GraphQL API with Cognito as the primary authorization mode.
- Backend business logic runs on AWS Lambda.
- Board, task, user, notification, and activity log data are stored in Amazon DynamoDB.
- DynamoDB Point-in-Time Recovery (PITR) is enabled to protect data from accidental writes or deletes.
- Amazon CloudWatch collects logs for troubleshooting and operational visibility.

### High-level architecture

![alt text](image.png)

### Main request flow

1. The developer builds the frontend and uploads static files to the *taskmanager-frontend-dev-** S3 bucket.
2. Users sign in through the `taskmanager-users-dev` Cognito User Pool.
3. The frontend sends GraphQL queries, mutations, and subscriptions to the `TaskManagerAPI-dev` AppSync API.
4. AppSync invokes Lambda functions such as `userManager-dev`, `boardManager-dev`, `taskProcessor-dev`, and `streamProcessor-dev`.
5. Lambda reads and writes data in DynamoDB tables including `TaskManager-Boards-dev`, `TaskManager-Tasks-dev`, `TaskManager-Users-dev`, `TaskManager-Notifications-dev`, and `TaskManager-ActivityLogs-dev`.
6. CloudWatch stores logs for troubleshooting and observability.

### Expected outcome
After completing this workshop, you can explain each AWS service in the TaskManager system, validate deployed resources, inspect DynamoDB data, verify PITR, and review operational logs in CloudWatch.