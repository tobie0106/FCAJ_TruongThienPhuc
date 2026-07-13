---
title: "Workshop"
date: 2026-07-09
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai Hệ thống Quản lý Công việc trên kiến trúc Serverless của AWS

#### Tổng quan

Workshop này hướng dẫn quá trình triển khai và kiểm tra một **Hệ thống Quản lý Công việc – Task Management System** sử dụng kiến trúc serverless trên AWS. Hệ thống hỗ trợ xác thực người dùng, quản lý bảng công việc, theo dõi nhiệm vụ, cập nhật trạng thái và lưu trữ nhật ký hoạt động thông qua các dịch vụ được quản lý của AWS.

Kiến trúc sử dụng Amazon S3 để lưu trữ frontend tĩnh, Amazon Cognito để xác thực người dùng, AWS AppSync để cung cấp GraphQL API, AWS Lambda để xử lý logic nghiệp vụ backend, Amazon DynamoDB để lưu trữ dữ liệu, IAM để kiểm soát quyền truy cập và Amazon CloudWatch để lưu trữ nhật ký cũng như giám sát hệ thống.

#### Nội dung

1. [Tổng quan Workshop](5.1-Workshop-overview)
2. [Chuẩn bị môi trường và quyền IAM](5.2-Prerequiste/)
3. [Triển khai frontend với Amazon S3](5.3-S3-vpc/)
4. [Cấu hình xác thực, API và backend](5.4-S3-onprem/)
5. [Kiểm tra DynamoDB, chỉ mục và PITR](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)