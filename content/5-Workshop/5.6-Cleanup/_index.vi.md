---
title : "Dọn dẹp tài nguyên"
date : 2026-07-09
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Tổng kết workshop

Chúc mừng bạn đã hoàn thành workshop TaskManager. Trong workshop này, bạn đã kiểm tra:

- IAM dashboard và các khuyến nghị bảo mật cơ bản.
- S3 bucket chứa frontend build output.
- Cognito User Pool `taskmanager-users-dev`.
- AppSync GraphQL API `TaskManagerAPI-dev`.
- Lambda functions xử lý backend.
- DynamoDB tables, Global Secondary Index và PITR.
- CloudWatch log groups cho Lambda backend.

#### Dọn dẹp tài nguyên

Nếu không còn sử dụng môi trường TaskManager, hãy dọn tài nguyên để tránh phát sinh chi phí.

1. Xóa frontend objects trong S3 bucket `taskmanager-frontend-dev-*`.

![alt text](image-1.png)

2. Xóa AppSync API `TaskManagerAPI-dev`.

![alt text](image-2.png)

3. Xóa Lambda functions `userManager-dev`, `boardManager-dev`, `taskProcessor-dev`, `streamProcessor-dev`.

![alt text](image-3.png)

4. Xóa DynamoDB tables `TaskManager-*` nếu không cần giữ dữ liệu.

![alt text](image-4.png)

5. Xóa Cognito User Pool `taskmanager-users-dev`.

![alt text](image-5.png)

6. Xóa CloudWatch log groups `/aws/lambda/*Manager-dev`, `/aws/lambda/taskProcessor-dev` và `/aws/lambda/streamProcessor-dev` nếu không cần audit log.

![alt text](image-6.png)

7. Xóa IAM roles/policies được tạo riêng cho project nếu không còn dùng.

![alt text](image-7.png)

{{% notice warning %}}
Trước khi xóa DynamoDB tables hoặc Cognito User Pool, hãy chắc chắn rằng dữ liệu không còn cần thiết. PITR chỉ giúp khôi phục trong khoảng thời gian được cấu hình khi bảng còn tồn tại hoặc khi quy trình restore được thực hiện đúng cách.
{{% /notice %}}

#### Kết luận

TaskManager là một ví dụ hoàn chỉnh về serverless application trên AWS: frontend tĩnh, authentication được quản lý, GraphQL API, compute bằng Lambda, database bằng DynamoDB và observability bằng CloudWatch.