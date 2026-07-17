---
title: "Nhật ký công việc Tuần 8"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### 1. Mục tiêu và bối cảnh

Trong tuần 8, em kết hợp hai nhóm công việc: rà soát kiến trúc AWS cho project nhóm và tìm hiểu quy trình di chuyển máy ảo từ môi trường On-premises lên AWS. Phần thực hành sử dụng **VMware Workstation** để chuẩn bị máy ảo và làm quen với các bước export, tải tệp máy ảo lên AWS và chuẩn bị cho quá trình import.

Mục tiêu của tuần là hiểu rằng migration không chỉ là sao chép một tệp. Cần xem xét định dạng máy ảo, hệ điều hành, dung lượng, nơi lưu tệp, IAM Role và khả năng tương thích của image trước khi có thể chạy workload trên cloud.

### 2. Nhật ký thực hiện

| STT | Công việc | Bắt đầu | Kết thúc |
|-----|-----------|---------|----------|
| 1 | Tổng hợp kiến thức đã học về compute, storage, security và migration | 08/06/2026 | 14/06/2026 |
| 2 | Họp nhóm và rà soát mục tiêu, thành phần và luồng dữ liệu của project | 08/06/2026 | 14/06/2026 |
| 3 | Xác định các thành phần On-premises và các thành phần triển khai trên AWS | 08/06/2026 | 14/06/2026 |
| 4 | Cài đặt và làm quen với VMware Workstation | 08/06/2026 | 14/06/2026 |
| 5 | Khởi động và kiểm tra hệ điều hành của máy ảo | 08/06/2026 | 14/06/2026 |
| 6 | Rà soát CPU, RAM, dung lượng đĩa và cấu hình mạng của máy ảo | 08/06/2026 | 14/06/2026 |
| 7 | Tắt máy ảo đúng cách và chuẩn bị cho quá trình export | 08/06/2026 | 14/06/2026 |
| 8 | Export máy ảo theo định dạng được bài lab hướng dẫn | 08/06/2026 | 14/06/2026 |
| 9 | Kiểm tra các tệp đầu ra, định dạng và tổng dung lượng | 08/06/2026 | 14/06/2026 |
| 10 | Tìm hiểu cách tải tệp máy ảo lên Amazon S3 | 08/06/2026 | 14/06/2026 |
| 11 | Tìm hiểu IAM Role, trust policy và quyền S3 cần thiết cho VM Import/Export | 08/06/2026 | 14/06/2026 |
| 12 | Ghi lại các điều kiện tương thích và bước theo dõi trạng thái import | 08/06/2026 | 14/06/2026 |

### 3. Rà soát kiến trúc project

Trong buổi làm việc nhóm, em cùng các thành viên xác định lại:

- Bài toán project cần giải quyết.
- Thành phần nào nằm ở môi trường On-premises.
- Thành phần nào được triển khai trên AWS.
- Dữ liệu di chuyển theo hướng nào.
- Dịch vụ nào chịu trách nhiệm về compute, storage và quyền truy cập.
- Cách kiểm tra hệ thống sau khi triển khai.

Việc thảo luận theo luồng dữ liệu giúp nhóm tránh lựa chọn dịch vụ chỉ vì đã từng học qua. Mỗi dịch vụ cần có vai trò cụ thể trong kiến trúc.

### 4. Quy trình chuẩn bị và export máy ảo

#### Bước 1: Kiểm tra môi trường VMware

Em cài đặt VMware Workstation, mở máy ảo được sử dụng cho bài thực hành và kiểm tra:

- Hệ điều hành có khởi động ổn định hay không.
- Dung lượng đĩa và dung lượng tệp máy ảo.
- Cấu hình CPU, RAM và mạng.
- Những tệp cấu hình và tệp đĩa đi kèm.

Trước khi export, em tắt máy ảo đúng cách để hạn chế nguy cơ tệp đĩa ở trạng thái không nhất quán.

#### Bước 2: Export máy ảo

Em sử dụng chức năng export của VMware để tạo bộ tệp máy ảo ở định dạng di động theo hướng dẫn bài lab. Sau khi hoàn tất, em kiểm tra vị trí lưu, tổng dung lượng và các tệp được tạo.

Bước kiểm tra này quan trọng vì upload một bộ tệp thiếu hoặc tệp bị lỗi có thể làm tác vụ import thất bại sau đó.

#### Bước 3: Chuẩn bị upload lên AWS

Em tìm hiểu quy trình sử dụng S3 để lưu tệp máy ảo trước khi import. Các thành phần cần quan tâm gồm:

- S3 bucket chứa image hoặc tệp đĩa.
- IAM Role cho dịch vụ import/export đọc object trong bucket và tạo tài nguyên cần thiết.
- Tệp mô tả container hoặc thông tin nguồn dùng trong lệnh/tác vụ import.
- Region nơi bucket và tác vụ import được thực hiện.

Em hiểu rằng việc tải tệp lên S3 mới chỉ là bước chuẩn bị; cần theo dõi trạng thái import và kiểm tra image sau khi tác vụ hoàn tất.

### 5. Các điểm kỹ thuật cần lưu ý

- Không export khi máy ảo vẫn đang chạy hoặc chưa tắt đúng cách.
- Kiểm tra định dạng và dung lượng trước khi upload.
- Đảm bảo bucket và tác vụ import nằm trong Region phù hợp.
- IAM Role phải có trust policy và quyền đọc S3 đúng phạm vi.
- Hệ điều hành hoặc cấu hình máy ảo cần nằm trong phạm vi được hỗ trợ.
- Upload thành công không đồng nghĩa với import thành công; cần xem trạng thái tác vụ.

### 6. Kết quả và phần việc cá nhân

- Tổng hợp lại kiến thức từ các buổi chia sẻ và bài lab trước đó.
- Tham gia rà soát kiến trúc và luồng dữ liệu của project nhóm.
- Cài đặt và sử dụng VMware Workstation để chạy môi trường mô phỏng On-premises.
- Kiểm tra cấu hình máy ảo trước khi export.
- Tạo bộ tệp máy ảo phục vụ bước upload.
- Hiểu vai trò của S3 và IAM trong quy trình VM Import/Export.
- Ghi lại những điều kiện cần kiểm tra trước khi import workload lên AWS.

### 7. Bài học rút ra

Tuần 8 giúp em hiểu migration là một quy trình có nhiều bước phụ thuộc lẫn nhau. Chỉ cần định dạng không phù hợp, tệp thiếu, quyền IAM sai hoặc cấu hình hệ điều hành không được hỗ trợ thì quá trình import có thể không thành công.

Em cũng cải thiện cách trao đổi kiến trúc trong nhóm. Thay vì liệt kê nhiều dịch vụ, nhóm cần mô tả rõ mỗi thành phần giải quyết vấn đề gì và dữ liệu đi qua hệ thống như thế nào.