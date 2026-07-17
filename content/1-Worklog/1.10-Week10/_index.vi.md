---
title: "Nhật ký công việc Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### 1. Mục tiêu và bối cảnh

Tuần 10 tập trung đưa các bài lab riêng lẻ vào một luồng kiến trúc có tính ứng dụng hơn. Nhóm tiếp tục hoàn thiện ý tưởng project, tham khảo góp ý của các anh/chị và rà soát vai trò của EC2, S3, IAM và Storage Gateway. Em đồng thời thực hành lại quy trình export máy ảo, tạo Storage Gateway, tạo File Share và mount từ máy On-premises.

Mục tiêu là kiểm tra một luồng hybrid storage hoàn chỉnh: máy On-premises truy cập File Share, gateway chuyển dữ liệu lên S3, còn quyền truy cập được kiểm soát bằng IAM.

### 2. Nhật ký thực hiện

| STT | Công việc | Bắt đầu | Kết thúc |
|-----|-----------|---------|----------|
| 1 | Rà soát yêu cầu, mục tiêu và kiến trúc của project nhóm | 22/06/2026 | 28/06/2026 |
| 2 | Tiếp nhận góp ý kỹ thuật và điều chỉnh vai trò của các dịch vụ AWS | 22/06/2026 | 28/06/2026 |
| 3 | Mô tả lại luồng dữ liệu từ môi trường On-premises đến Amazon S3 | 22/06/2026 | 28/06/2026 |
| 4 | Thực hành lại quy trình export máy ảo hoặc workload | 22/06/2026 | 28/06/2026 |
| 5 | Chuẩn bị gateway host, S3 bucket và IAM Role | 22/06/2026 | 28/06/2026 |
| 6 | Tạo và kích hoạt AWS Storage Gateway | 22/06/2026 | 28/06/2026 |
| 7 | Kiểm tra trạng thái gateway trước khi tạo File Share | 22/06/2026 | 28/06/2026 |
| 8 | Tạo File Share liên kết với S3 bucket đích | 22/06/2026 | 28/06/2026 |
| 9 | Cấu hình quyền truy cập và lấy thông tin mount | 22/06/2026 | 28/06/2026 |
| 10 | Mount File Share từ máy On-premises và kiểm tra thao tác đọc/ghi | 22/06/2026 | 28/06/2026 |
| 11 | Kiểm tra object, prefix, kích thước và thời gian cập nhật trên S3 | 22/06/2026 | 28/06/2026 |
| 12 | Rà soát IAM Role và dọn dẹp tài nguyên không còn sử dụng | 22/06/2026 | 28/06/2026 |

### 3. Hoàn thiện ý tưởng và kiến trúc project

Trong quá trình trao đổi, nhóm không chỉ liệt kê dịch vụ mà mô tả luồng hoạt động:

1. Người dùng hoặc máy client làm việc trong môi trường On-premises.
2. Client truy cập một File Share bằng giao thức tệp quen thuộc.
3. Storage Gateway tiếp nhận thao tác và đồng bộ dữ liệu lên Amazon S3.
4. IAM Role cho phép gateway truy cập bucket trong phạm vi cần thiết.
5. EC2 hoặc các thành phần khác xử lý workload theo nội dung project.
6. Nhóm thực hiện kiểm thử quyền, kết nối và dữ liệu.

Các góp ý kỹ thuật giúp nhóm rà soát xem dịch vụ nào thực sự cần thiết, quyền nào nên giới hạn và bước kiểm thử nào cần bổ sung.

### 4. Quy trình Storage Gateway và File Share

#### Bước 1: Chuẩn bị tài nguyên

Em kiểm tra gateway host, kết nối mạng, bucket đích và IAM Role. Trước khi tạo File Share, các thành phần này phải sẵn sàng để tránh lỗi chỉ xuất hiện ở bước mount hoặc ghi dữ liệu.

#### Bước 2: Tạo và kích hoạt gateway

Em thực hiện quy trình kích hoạt, đặt tên và kiểm tra trạng thái gateway. Sau khi Console hiển thị gateway hoạt động, em mới tiếp tục tạo File Share.

#### Bước 3: Tạo File Share

Em chọn S3 bucket đích, cấu hình quyền và lấy thông tin mount. Em xem lại phạm vi client được phép truy cập và các tùy chọn liên quan đến cách object được tạo trong S3.

#### Bước 4: Mount từ On-premises

Trên máy client, em sử dụng lệnh hoặc chức năng mount phù hợp với giao thức File Share. Sau khi mount, em kiểm tra thư mục, tạo tệp thử nghiệm và đọc lại tệp để xác nhận thao tác tệp hoạt động.

#### Bước 5: Xác minh trên S3

Em mở S3 Console và kiểm tra:

- Object có xuất hiện trong bucket hay không.
- Đường dẫn/prefix có đúng với cấu trúc mong muốn hay không.
- Kích thước và thời gian cập nhật của object.
- Quyền của gateway có bị cấp rộng hơn phạm vi bucket cần thiết hay không.

### 5. Kiểm thử và xử lý theo nguyên nhân

| Vấn đề cần kiểm tra | Nguyên nhân có thể | Cách xác minh |
|--------------------|--------------------|--------------|
| Không mount được share | Sai đường dẫn, mạng hoặc client chưa được phép | Kiểm tra mount command, kết nối và cấu hình share |
| Mount được nhưng không ghi được | IAM Role hoặc quyền share chưa đủ | Kiểm tra policy và thông báo lỗi |
| Tệp chưa xuất hiện trên S3 | Gateway chưa đồng bộ hoặc sai bucket/prefix | Kiểm tra trạng thái gateway và vị trí object |
| Object xuất hiện sai vị trí | Cấu trúc thư mục hoặc prefix chưa đúng | So sánh đường dẫn client với key trong S3 |
| Tài nguyên phát sinh chi phí | Instance/gateway/bucket vẫn còn sau lab | Lập danh sách và cleanup theo thứ tự |

### 6. Phần việc cá nhân và kết quả

- Tham gia rà soát kiến trúc và luồng dữ liệu của project.
- Ghi nhận góp ý kỹ thuật và cập nhật nội dung mô tả kiến trúc.
- Chuẩn bị và kiểm tra các thành phần cần thiết cho Storage Gateway.
- Tạo File Share và thực hiện mount từ máy On-premises.
- Kiểm tra thao tác ghi/đọc và đối chiếu object trên S3.
- Rà soát IAM Role theo nguyên tắc chỉ cấp quyền cần thiết.
- Dọn dẹp tài nguyên không còn sử dụng.

### 7. Bài học rút ra

Tuần 10 giúp em kết nối kiến thức của nhiều tuần thành một luồng thực tế. Storage Gateway chỉ hoạt động đúng khi mạng, gateway, File Share, S3 và IAM đều được cấu hình tương thích. Kiểm tra từng tài nguyên riêng lẻ là chưa đủ; cần kiểm tra toàn bộ đường đi của dữ liệu từ client đến S3.

Em cũng hiểu rõ hơn giá trị của việc nhận góp ý kiến trúc. Một sơ đồ có nhiều dịch vụ chưa chắc đã tốt; thiết kế tốt cần giải thích được lý do sử dụng, quyền truy cập, cách kiểm thử và chi phí vận hành.