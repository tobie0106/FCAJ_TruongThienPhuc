---
title: "Nhật ký công việc Tuần 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### 1. Mục tiêu và bối cảnh

Tuần 9 tiếp tục củng cố kiến thức IAM nhưng tập trung vào việc tổ chức người dùng theo nhóm và kiểm tra giới hạn quyền rõ ràng hơn. Em tạo một **Limited IAM User**, kiểm tra những hành động được phép và bị từ chối, sau đó thực hành IAM Group, Policy và Role. Phần cuối tuần giới thiệu **AWS Key Management Service (AWS KMS)** để hiểu vai trò của khóa mã hóa trong bảo vệ dữ liệu.

Mục tiêu của em là xây dựng được tư duy kiểm thử quyền theo kịch bản và hiểu rằng quản lý khóa mã hóa cũng cần policy, quyền sử dụng và trách nhiệm quản trị riêng.

### 2. Nhật ký thực hiện

| STT | Công việc | Bắt đầu | Kết thúc |
|-----|-----------|---------|----------|
| 1 | Tạo Limited IAM User phục vụ kiểm thử quyền | 15/06/2026 | 21/06/2026 |
| 2 | Đăng nhập bằng Limited IAM User và ghi nhận các chức năng được truy cập | 15/06/2026 | 21/06/2026 |
| 3 | Thử các hành động nằm trong phạm vi policy | 15/06/2026 | 21/06/2026 |
| 4 | Thử các hành động ngoài phạm vi và ghi nhận thông báo access denied | 15/06/2026 | 21/06/2026 |
| 5 | Tạo IAM Group và gắn policy cho nhóm | 15/06/2026 | 21/06/2026 |
| 6 | Thêm IAM User vào group và kiểm tra quyền hiệu lực | 15/06/2026 | 21/06/2026 |
| 7 | Tạo IAM Role và kiểm tra trust policy | 15/06/2026 | 21/06/2026 |
| 8 | Phân biệt trust policy và permission policy của IAM Role | 15/06/2026 | 21/06/2026 |
| 9 | Tạo khóa trong AWS Key Management Service | 15/06/2026 | 21/06/2026 |
| 10 | Cấu hình alias, key administrators và key users | 15/06/2026 | 21/06/2026 |
| 11 | Rà soát KMS key policy và quyền sử dụng khóa | 15/06/2026 | 21/06/2026 |
| 12 | So sánh quyền truy cập tài nguyên với quyền sử dụng khóa mã hóa | 15/06/2026 | 21/06/2026 |

### 3. Kiểm thử Limited IAM User

Sau khi tạo user, em đăng nhập bằng đường dẫn và thông tin dành cho IAM User. Em không dùng tài khoản quản trị để kiểm tra vì mục tiêu là quan sát đúng quyền của người dùng bị giới hạn.

Em xây dựng hai nhóm kịch bản:

- **Kịch bản hợp lệ:** thao tác nằm trong phạm vi policy phải thực hiện được.
- **Kịch bản không hợp lệ:** thao tác ngoài phạm vi hoặc nhạy cảm phải bị từ chối.

Khi gặp thông báo access denied, em ghi nhận dịch vụ, hành động đang thực hiện và policy liên quan. Điều này giúp em không xử lý bằng cách gắn ngay quyền quản trị, mà quay lại xác định chính xác action nào thực sự cần thiết.

### 4. Quản lý quyền bằng IAM Group

Em tạo IAM Group, gắn policy vào group và thêm user vào group. Sau đó, em đăng nhập lại để kiểm tra quyền mới.

Bài thực hành giúp em hiểu lợi ích của group:

- Giảm việc gắn cùng một policy cho nhiều user.
- Dễ cập nhật quyền cho một nhóm chức năng.
- Phân loại người dùng theo vai trò công việc.
- Giúp việc rà soát quyền rõ ràng hơn.

Em cũng lưu ý rằng quyền hiệu lực của user có thể đến từ nhiều nguồn như policy gắn trực tiếp, group policy hoặc permission boundary, vì vậy cần kiểm tra tổng thể khi phân tích kết quả.

### 5. IAM Role và trust relationship

Khi tạo role, em đọc lại trust policy để xác định dịch vụ hoặc principal nào được phép assume role. Sau đó, em xem permission policy để biết role có thể thực hiện hành động nào sau khi được assume.

Sự khác nhau giữa hai loại policy này là điểm quan trọng:

- Trust policy trả lời câu hỏi: **Ai có thể sử dụng role?**
- Permission policy trả lời câu hỏi: **Role có thể làm gì?**

Nếu trust policy sai, role không thể được assume dù permission policy đã chứa đầy đủ quyền.

### 6. Tìm hiểu AWS KMS

Em tạo khóa KMS theo hướng dẫn của bài lab và xem các thành phần:

- Loại khóa và phạm vi sử dụng.
- Alias để dễ nhận biết.
- Key administrators và key users.
- Key policy kiểm soát quyền quản trị và quyền sử dụng khóa.
- Khả năng tích hợp với các dịch vụ AWS để mã hóa dữ liệu.

Qua bước này, em hiểu rằng có quyền truy cập vào một object hoặc tài nguyên chưa chắc đồng nghĩa với việc có quyền sử dụng khóa mã hóa. Khi dữ liệu được bảo vệ bằng KMS, người dùng hoặc dịch vụ còn cần quyền phù hợp đối với key.

### 7. Bảng kiểm thử

| Kịch bản | Kết quả mong đợi | Nội dung xác minh |
|---------|------------------|-------------------|
| Limited user thực hiện hành động được cho phép | Thành công | Policy cấp đúng quyền |
| Limited user thử hành động ngoài phạm vi | Access denied | Giới hạn được thực thi |
| User được thêm vào IAM Group | Nhận quyền của group | Quản lý quyền tập trung hoạt động |
| Principal không nằm trong trust policy | Không assume được role | Trust relationship đúng |
| User không có quyền KMS cần thiết | Không sử dụng được key | Key policy và IAM cùng ảnh hưởng |

### 8. Kết quả và bài học

- Tạo và kiểm tra Limited IAM User bằng cả kịch bản thành công và thất bại.
- Sử dụng IAM Group để quản lý quyền cho user.
- Phân biệt permission policy với trust policy.
- Tạo và đọc các thành phần chính của KMS key.
- Hiểu rằng truy cập dữ liệu mã hóa phụ thuộc đồng thời vào quyền tài nguyên và quyền sử dụng key.
- Cải thiện kỹ năng đọc thông báo access denied và truy ngược về policy liên quan.

Tuần 9 giúp em thấy rằng bảo mật AWS là sự kết hợp của nhiều lớp. IAM kiểm soát danh tính và hành động, trong khi KMS bổ sung lớp kiểm soát đối với khóa mã hóa. Khi thiết kế hoặc xử lý lỗi, cần xem xét cả hai thay vì chỉ kiểm tra quyền trên dịch vụ lưu trữ.