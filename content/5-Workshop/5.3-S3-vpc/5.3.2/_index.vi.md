---
title : "Kiểm tra kết quả tải frontend lên S3"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Mục tiêu

Xác minh rằng các tệp frontend đã tồn tại trong S3 bucket và sẵn sàng được tích hợp với Amazon CloudFront hoặc quy trình triển khai.

### Kiểm tra danh sách tệp

Trong tab **Files and folders**, xác nhận bucket chứa các tệp sau:

- `index.html`
- `favicon.svg`
- `icons.svg`
- `assets/index-*.js`
- `assets/index-*.css`

![Danh sách các tệp frontend trong S3 bucket](image.png)

### Ý nghĩa của các tệp

- `index.html`: điểm khởi đầu của ứng dụng web một trang.
- `assets/index-*.js`: tệp JavaScript đã được đóng gói, chứa logic giao diện người dùng và các lệnh gọi API.
- `assets/index-*.css`: tệp định dạng giao diện frontend.
- `favicon.svg` và `icons.svg`: các tài nguyên biểu tượng tĩnh.

### Kết luận

Frontend đã được tải lên thành công. Tiếp theo, chúng ta sẽ kiểm tra lớp xác thực, GraphQL API và hệ thống backend AWS Lambda được frontend sử dụng.