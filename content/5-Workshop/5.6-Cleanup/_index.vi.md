---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

### Tổng kết workshop

Chúc mừng, bạn đã hoàn thành workshop TaskManager. Trong workshop này, bạn đã kiểm tra:

- Bảng điều khiển IAM và các khuyến nghị bảo mật cơ bản.
- S3 bucket chứa thư mục build của frontend.
- Cognito User Pool `taskmanager-users-dev`.
- AppSync GraphQL API `TaskManagerAPI-dev`.
- Các hàm Lambda xử lý logic backend.
- Các bảng DynamoDB, Global Secondary Index và PITR.
- Các CloudWatch log group của hệ thống backend Lambda.

### Dọn dẹp tài nguyên

Nếu không còn sử dụng môi trường TaskManager, hãy dọn dẹp các tài nguyên để tránh phát sinh thêm chi phí.

1. Xóa các tệp frontend trong S3 bucket `taskmanager-frontend-dev-*`.
2. Xóa AppSync API `TaskManagerAPI-dev`.
3. Xóa các hàm Lambda `userManager-dev`, `boardManager-dev`, `taskProcessor-dev` và `streamProcessor-dev`.
4. Xóa các bảng DynamoDB có tên `TaskManager-*` nếu dữ liệu không còn cần thiết.
5. Xóa Cognito User Pool `taskmanager-users-dev`.
6. Xóa các CloudWatch log group `/aws/lambda/*Manager-dev`, `/aws/lambda/taskProcessor-dev` và `/aws/lambda/streamProcessor-dev`.
7. Xóa các IAM role hoặc policy chỉ được tạo cho dự án này nếu chúng không còn được sử dụng.

{{% notice warning %}}
Trước khi xóa các bảng DynamoDB hoặc Cognito User Pool, hãy đảm bảo rằng dữ liệu không còn cần thiết. PITR chỉ hỗ trợ khôi phục trong khoảng thời gian đã được cấu hình và quá trình khôi phục phải được thực hiện đúng cách.
{{% /notice %}}

### Kết luận

TaskManager là một ví dụ hoàn chỉnh về ứng dụng serverless trên AWS, bao gồm lưu trữ frontend tĩnh, xác thực được quản lý, GraphQL API, xử lý bằng Lambda, cơ sở dữ liệu DynamoDB và khả năng giám sát bằng CloudWatch.