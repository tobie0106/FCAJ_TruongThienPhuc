---
title: "Nhật ký công việc Tuần 5"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### 1. Mục tiêu và bối cảnh

Nội dung trọng tâm của tuần 5 là xây dựng luồng lưu trữ kết hợp giữa môi trường tại chỗ và AWS thông qua **AWS Storage Gateway**, đồng thời thực hành lưu trữ website tĩnh bằng **Amazon S3**. Em cần hiểu dữ liệu được tạo từ máy trong môi trường On-premises đi qua File Share và được lưu thành object trong S3 như thế nào.

Bên cạnh đó, em thực hành cấu hình quyền truy cập công khai cho website tĩnh. Phần này giúp em phân biệt giữa việc công khai toàn bộ bucket và chỉ cho phép đọc các object cần thiết để website hoạt động.

### 2. Nhật ký thực hiện

| STT | Công việc | Bắt đầu | Kết thúc |
|-----|-----------|---------|----------|
| 1 | Tìm hiểu kiến trúc và luồng dữ liệu của AWS Storage Gateway | 18/05/2026 | 24/05/2026 |
| 2 | Chuẩn bị máy gateway, Amazon S3 bucket và máy client cho bài thực hành | 18/05/2026 | 24/05/2026 |
| 3 | Tạo và kích hoạt AWS Storage Gateway trên AWS Console | 18/05/2026 | 24/05/2026 |
| 4 | Kiểm tra trạng thái hoạt động và khả năng kết nối của gateway | 18/05/2026 | 24/05/2026 |
| 5 | Tạo File Share liên kết với Amazon S3 bucket | 18/05/2026 | 24/05/2026 |
| 6 | Cấu hình quyền truy cập và thông tin máy client được phép sử dụng File Share | 18/05/2026 | 24/05/2026 |
| 7 | Mount File Share trên máy On-premises và sao chép tệp thử nghiệm | 18/05/2026 | 24/05/2026 |
| 8 | Kiểm tra tệp được đồng bộ và xuất hiện dưới dạng object trong S3 bucket | 18/05/2026 | 24/05/2026 |
| 9 | Tạo S3 bucket và tải các tệp của website tĩnh lên bucket | 18/05/2026 | 24/05/2026 |
| 10 | Bật Static website hosting và cấu hình quyền đọc object | 18/05/2026 | 24/05/2026 |
| 11 | Kiểm tra website endpoint và rà soát Block Public Access cùng bucket policy | 18/05/2026 | 24/05/2026 |
| 12 | Ghi lại kết quả, khó khăn và quy trình kiểm tra bài lab | 18/05/2026 | 24/05/2026 |

### 3. Quy trình kỹ thuật đã thực hiện

#### Bước 1: Chuẩn bị luồng Storage Gateway

Trước khi tạo tài nguyên, em xác định các thành phần của bài lab:

- Một gateway đóng vai trò cầu nối giữa môi trường On-premises và AWS.
- Một File Share để máy client truy cập theo giao thức tệp.
- Một Amazon S3 bucket làm nơi lưu dữ liệu phía sau.
- Quyền IAM để Storage Gateway có thể thao tác với bucket theo yêu cầu.

Việc xác định trước luồng dữ liệu giúp em hiểu rõ mục tiêu của từng bước thay vì chỉ thực hiện tuần tự theo hướng dẫn.

#### Bước 2: Tạo và kích hoạt gateway

Em tạo Storage Gateway theo quy trình của bài lab, cung cấp thông tin mạng cần thiết và hoàn tất bước kích hoạt trên AWS Console. Sau khi kích hoạt, em kiểm tra trạng thái của gateway để đảm bảo dịch vụ đã có thể giao tiếp với AWS.

Điểm quan trọng trong bước này là gateway phải có kết nối mạng ổn định, truy cập được các endpoint cần thiết và được gắn đúng cấu hình lưu trữ. Nếu gateway chưa ở trạng thái hoạt động, việc tạo File Share hoặc đồng bộ dữ liệu sẽ không thể kiểm tra chính xác.

#### Bước 3: Tạo File Share liên kết với S3

Sau khi gateway sẵn sàng, em tạo File Share và chọn S3 bucket làm nơi lưu trữ dữ liệu. Em kiểm tra các thông tin liên quan đến quyền truy cập, đường dẫn share và danh sách máy client được phép sử dụng.

Từ máy On-premises, em mount File Share theo thông tin được cung cấp. Em tạo hoặc sao chép một tệp thử nghiệm vào thư mục đã mount, sau đó mở S3 Console để xác nhận object tương ứng xuất hiện trong bucket. Bài kiểm tra này chứng minh rằng dữ liệu từ phía client đã đi qua gateway và được lưu trên AWS.

#### Bước 4: Tạo website tĩnh trên S3

Em tạo một S3 bucket phục vụ website, sau đó tải các tệp nội dung lên đúng cấu trúc. Tiếp theo, em bật tính năng **Static website hosting**, xác định tài liệu trang chủ và kiểm tra website endpoint.

Khi cấu hình quyền, em xem xét hai lớp kiểm soát:

- Thiết lập **Block Public Access** của bucket.
- Bucket policy hoặc quyền đọc object cần thiết cho website.

Em không coi việc tắt chặn truy cập công khai là đủ. Website chỉ hoạt động khi object được phép đọc theo đúng policy, còn các hành động ghi, xóa hoặc quản trị vẫn phải được bảo vệ.

### 4. Kiểm tra và xử lý theo từng lớp

Đối với Storage Gateway, em kiểm tra lần lượt:

1. Trạng thái gateway trên Console.
2. Kết nối mạng giữa client và gateway.
3. Thông tin mount của File Share.
4. Quyền truy cập vào S3 bucket.
5. Sự xuất hiện của object sau khi chép tệp.

Đối với website S3, em kiểm tra:

1. Tên và vị trí của tệp trang chủ.
2. Cấu hình static website hosting.
3. Block Public Access.
4. Bucket policy và quyền `GetObject`.
5. Website endpoint được cung cấp bởi S3.

Cách kiểm tra này giúp em tránh nhầm lẫn giữa lỗi lưu trữ, lỗi quyền và lỗi đường dẫn tệp.

### 5. Kết quả và phần việc cá nhân

- Tạo và kích hoạt được Storage Gateway theo bài lab.
- Tạo File Share liên kết với Amazon S3.
- Mount File Share từ máy On-premises và thực hiện kiểm tra bằng tệp mẫu.
- Xác nhận dữ liệu được ghi từ File Share xuất hiện trong S3 bucket.
- Tạo S3 bucket và tải nội dung website tĩnh.
- Bật static website hosting và kiểm tra website endpoint.
- Hiểu rõ hơn sự khác nhau giữa Block Public Access và quyền đọc object.
- Ghi lại quy trình kiểm tra để sử dụng cho các bài lab hybrid storage sau này.

### 6. Bài học rút ra

Tuần 5 giúp em hiểu Storage Gateway không chỉ là một dịch vụ lưu trữ độc lập mà là lớp kết nối giữa cách truy cập tệp quen thuộc trong môi trường On-premises và mô hình object storage của Amazon S3. Việc kiểm tra bằng một tệp thực tế giúp em nhìn thấy rõ luồng dữ liệu thay vì chỉ hiểu kiến trúc trên lý thuyết.

Phần website tĩnh cũng giúp em nhận thức rõ hơn về bảo mật S3. Công khai website cần được thực hiện có kiểm soát; không nên cấp quyền rộng cho toàn bộ bucket nếu chỉ cần cho phép người dùng đọc một nhóm object cụ thể.