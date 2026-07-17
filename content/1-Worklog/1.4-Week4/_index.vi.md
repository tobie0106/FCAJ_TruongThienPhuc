---
title: "Nhật ký công việc Tuần 4"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### 1. Mục tiêu và bối cảnh

Trong tuần thứ tư, em chuyển từ phần tìm hiểu tổng quan sang thực hành trực tiếp với ba dịch vụ nền tảng là **Amazon EC2, Amazon S3 và AWS Identity and Access Management (IAM)**. Mục tiêu chính là hiểu vai trò của từng dịch vụ trong một hệ thống đơn giản: EC2 cung cấp máy chủ để chạy ứng dụng, S3 lưu trữ dữ liệu hoặc tệp tĩnh, còn IAM kiểm soát người dùng và quyền truy cập.

Bài thực hành yêu cầu em tạo tài nguyên, kiểm tra trạng thái hoạt động, triển khai một ứng dụng cơ bản trên EC2 và xem xét các quyền cần thiết để người dùng hoặc dịch vụ có thể thao tác với tài nguyên AWS mà không được cấp quyền vượt quá nhu cầu.

### 2. Nhật ký thực hiện

| STT | Công việc | Bắt đầu | Kết thúc |
|-----|-----------|---------|----------|
| 1 | Ôn lại kiến thức về Amazon EC2, Amazon S3 và AWS IAM thông qua tài liệu và video hướng dẫn | 11/05/2026 | 17/05/2026 |
| 2 | Tạo và cấu hình một Amazon EC2 instance theo yêu cầu của bài thực hành | 11/05/2026 | 17/05/2026 |
| 3 | Kiểm tra trạng thái `Running`, status check, VPC, subnet và Security Group của EC2 instance | 11/05/2026 | 17/05/2026 |
| 4 | Kết nối vào máy chủ và triển khai một ứng dụng cơ bản trên Amazon EC2 | 11/05/2026 | 17/05/2026 |
| 5 | Kiểm tra khả năng truy cập ứng dụng thông qua địa chỉ của EC2 instance | 11/05/2026 | 17/05/2026 |
| 6 | Rà soát inbound rule của Security Group và chỉ giữ lại những cổng cần thiết | 11/05/2026 | 17/05/2026 |
| 7 | Tìm hiểu cấu trúc bucket, object và cách lưu trữ dữ liệu trên Amazon S3 | 11/05/2026 | 17/05/2026 |
| 8 | Phân biệt IAM User, IAM Policy và IAM Role trong việc kiểm soát quyền truy cập | 11/05/2026 | 17/05/2026 |
| 9 | Ghi lại quy trình kiểm tra trạng thái tài nguyên, ứng dụng, mạng và quyền truy cập | 11/05/2026 | 17/05/2026 |
| 10 | Tự học tại nhà và xem thêm video hướng dẫn về EC2, S3 và IAM | 11/05/2026 | 17/05/2026 |

### 3. Quy trình kỹ thuật đã thực hiện

#### Bước 1: Chuẩn bị và tạo EC2 instance

Em truy cập Amazon EC2 Console trong Region được sử dụng cho bài lab, chọn cấu hình máy phù hợp với yêu cầu thực hành và khởi tạo instance. Trong quá trình tạo, em kiểm tra các thành phần sau:

- Tên tài nguyên để dễ nhận biết và quản lý.
- Amazon Machine Image và cấu hình máy theo hướng dẫn của bài lab.
- VPC, subnet và địa chỉ mạng được gán cho instance.
- Security Group dùng để kiểm soát lưu lượng vào máy chủ.
- Phương thức kết nối để có thể cấu hình ứng dụng sau khi instance hoạt động.

Sau khi tạo, em chờ instance chuyển sang trạng thái `Running` và kiểm tra status check trước khi tiếp tục. Bước này giúp em hiểu rằng một instance xuất hiện trên Console chưa đồng nghĩa với việc máy đã sẵn sàng hoàn toàn; cần kiểm tra cả trạng thái hệ thống và trạng thái của instance.

#### Bước 2: Kết nối và triển khai ứng dụng

Khi EC2 đã sẵn sàng, em kết nối vào máy chủ bằng phương thức được bài lab cung cấp. Em thực hiện các thao tác cơ bản gồm cập nhật môi trường, cài thành phần cần thiết cho ứng dụng, đưa nội dung ứng dụng lên máy và khởi động dịch vụ.

Sau đó, em sử dụng địa chỉ truy cập của EC2 để kiểm tra. Nếu ứng dụng chưa truy cập được, các nội dung cần rà soát gồm trạng thái tiến trình, cổng ứng dụng, inbound rule của Security Group và địa chỉ dùng để kiểm tra. Qua bước này, em hiểu rõ hơn mối liên hệ giữa cấu hình bên trong máy chủ và cấu hình mạng trên AWS.

#### Bước 3: Rà soát Security Group

Em kiểm tra lại các inbound rule thay vì mở toàn bộ lưu lượng. Quyền truy cập quản trị chỉ nên giới hạn cho nguồn cần thiết, trong khi cổng phục vụ ứng dụng chỉ được mở theo đúng yêu cầu. Đây là nội dung giúp em liên hệ với nguyên tắc **least privilege**: không chỉ áp dụng cho IAM mà còn cần áp dụng khi cấu hình mạng.

#### Bước 4: Tìm hiểu S3 và IAM

Với Amazon S3, em xem lại cấu trúc bucket, object và cách tệp được lưu trữ. Với IAM, em phân biệt:

- **IAM User:** đại diện cho một người dùng hoặc tài khoản cần đăng nhập.
- **IAM Policy:** mô tả hành động nào được phép hoặc bị từ chối trên tài nguyên nào.
- **IAM Role:** cung cấp quyền tạm thời cho người dùng, ứng dụng hoặc dịch vụ AWS.

Em cũng nhận thấy việc dùng role cho dịch vụ thường an toàn hơn việc lưu access key trực tiếp trong mã nguồn hoặc trên máy chủ.

### 4. Nội dung kiểm tra và lưu ý kỹ thuật

Trong tuần này, em tập trung kiểm tra các điểm có thể làm hệ thống không hoạt động đúng:

- EC2 chưa hoàn tất status check nhưng đã tiến hành kết nối.
- Security Group chưa mở đúng cổng ứng dụng hoặc mở phạm vi quá rộng.
- Ứng dụng đã được chép lên máy nhưng dịch vụ chưa được khởi động.
- Sử dụng sai địa chỉ truy cập hoặc nhầm giữa địa chỉ private và public.
- Cấp quyền IAM rộng hơn yêu cầu thực tế.

Việc rà soát theo từng lớp — trạng thái tài nguyên, hệ điều hành, ứng dụng, mạng và phân quyền — giúp em kiểm tra lỗi có hệ thống thay vì thay đổi cấu hình ngẫu nhiên.

### 5. Kết quả và phần việc cá nhân

- Tạo và kiểm tra được một EC2 instance phục vụ bài thực hành.
- Thực hiện các bước triển khai ứng dụng cơ bản lên máy chủ.
- Kiểm tra khả năng truy cập ứng dụng thông qua cấu hình mạng phù hợp.
- Rà soát Security Group và hiểu lý do chỉ nên mở các cổng cần thiết.
- Củng cố kiến thức về bucket, object và lưu trữ dữ liệu trên Amazon S3.
- Phân biệt rõ hơn IAM User, IAM Policy và IAM Role.
- Ghi lại quy trình kiểm tra để có thể áp dụng cho những bài lab phức tạp hơn.

### 6. Bài học rút ra

Tuần 4 giúp em nhận ra rằng triển khai một ứng dụng trên cloud không chỉ là tạo máy chủ. Để hệ thống hoạt động, cần phối hợp nhiều lớp cấu hình gồm tài nguyên tính toán, hệ điều hành, ứng dụng, mạng và quyền truy cập. Nếu một lớp được cấu hình chưa đúng, người dùng có thể không truy cập được dù instance vẫn ở trạng thái hoạt động.

Em cũng hiểu rõ hơn vai trò của bảo mật ngay từ bước cấu hình đầu tiên. Security Group và IAM cần được thiết lập theo đúng mục đích sử dụng, tránh mở cổng hoặc cấp quyền quá rộng chỉ để bài lab hoạt động nhanh. Đây là nền tảng quan trọng cho các tuần tiếp theo khi em làm việc với Storage Gateway, CloudFront và các chính sách IAM chi tiết hơn.