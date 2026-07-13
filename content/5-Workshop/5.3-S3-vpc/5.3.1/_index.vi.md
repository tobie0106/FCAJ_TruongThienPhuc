---
title : "Tải tài nguyên frontend lên S3"
date : 2026-07-09
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

### Mục tiêu

Tải thư mục build của frontend TaskManager lên Amazon S3 để chuẩn bị cung cấp giao diện web cho người dùng.

### Các bước thực hiện

1. Build frontend trên máy cá nhân hoặc thông qua quy trình CI/CD.
2. Mở Amazon S3 Console tại khu vực `ap-southeast-1`.
3. Chọn bucket frontend có tên `taskmanager-frontend-dev-*`.
4. Chọn **Upload** và thêm các tệp trong thư mục build.
5. Xác nhận quá trình tải lên đã hoàn tất thành công.

![Kết quả tải tài nguyên frontend lên S3](image.png)

### Kết quả

Bucket chứa 5 tệp frontend:

- `index.html`
- JavaScript bundle trong thư mục `assets/`
- CSS bundle trong thư mục `assets/`
- `favicon.svg`
- `icons.svg`

Ảnh chụp màn hình hiển thị **Succeeded: 5 files, 611.4 KB (100%)**, xác nhận rằng các tài nguyên frontend đã được tải lên Amazon S3 thành công.

### Lưu ý bảo mật

Khi bucket này được kết nối với Amazon CloudFront, không nên cho phép truy cập đọc công khai. Hãy sử dụng CloudFront Origin Access Control hoặc Origin Access Identity để bảo đảm rằng chỉ CloudFront mới có thể đọc các đối tượng trong bucket.