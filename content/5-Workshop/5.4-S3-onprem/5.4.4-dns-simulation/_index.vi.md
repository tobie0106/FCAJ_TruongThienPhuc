---
title : "Kiểm tra nhật ký CloudWatch"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

### Mục tiêu

Kiểm tra các nhóm nhật ký trên Amazon CloudWatch để xác nhận rằng hệ thống backend đã có nơi lưu trữ nhật ký hoạt động.

### Kiểm tra Log group

1. Mở Amazon CloudWatch Console.
2. Đi đến **Logs > Log groups**.
3. Tìm kiếm các log group liên quan đến TaskManager.

![Danh sách các log group của TaskManager](image.png)

Các log group quan trọng trong ảnh chụp màn hình bao gồm:

- `/aws/lambda/userManager-dev`
- `/aws/lambda/boardManager-dev`
- `/aws/lambda/taskProcessor-dev`
- `/aws/lambda/streamProcessor-dev`

### Ý nghĩa

CloudWatch hỗ trợ bạn:

- Kiểm tra lỗi khi AppSync gọi các hàm Lambda.
- Theo dõi các yêu cầu được gửi đến backend.
- Gỡ lỗi logic tạo hoặc cập nhật công việc.
- Kiểm tra các log stream của từng lần thực thi hàm Lambda.
- Cấu hình thời gian lưu trữ nhật ký để kiểm soát chi phí lưu trữ.

### Kết luận

CloudWatch chứa các log group của hệ thống backend Lambda. Đây là nền tảng để giám sát, xử lý sự cố và vận hành hệ thống TaskManager.