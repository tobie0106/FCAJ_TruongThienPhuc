---
title : "Cấu hình anthentication, API và backend"
date : 2026-07-09 
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan

Phần này kiểm tra các lớp chính phía sau frontend của hệ thống TaskManager:

- **Amazon Cognito** quản lý người dùng và cấp token đăng nhập.
- **AWS AppSync** cung cấp GraphQL API cho frontend.
- **AWS Lambda** thực thi logic nghiệp vụ liên quan đến người dùng, bảng công việc, nhiệm vụ và xử lý luồng dữ liệu.
- **Amazon CloudWatch** lưu trữ nhật ký để hỗ trợ xử lý sự cố backend.

### Luồng xác thực và API

1. Người dùng đăng nhập vào frontend.
2. Cognito xác thực người dùng và trả về JWT token.
3. Frontend gửi các yêu cầu GraphQL đến AppSync kèm theo token.
4. AppSync xác thực token và gọi các Lambda resolver.
5. Lambda đọc và ghi dữ liệu trong DynamoDB.
6. CloudWatch lưu trữ nhật ký để hỗ trợ xử lý sự cố.