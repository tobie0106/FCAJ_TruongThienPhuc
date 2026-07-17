---
title: "Nhật ký công việc Tuần 7"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### 1. Mục tiêu và bối cảnh

Tuần 7 tập trung sâu hơn vào **AWS Identity and Access Management**. Khác với những tuần trước chỉ tìm hiểu khái niệm, tuần này em thực hành tạo IAM User, Policy và Role, sau đó kiểm tra quyền trên Amazon EC2. Phần quan trọng của bài lab là sử dụng điều kiện liên quan đến **tag** để kiểm soát việc tạo hoặc thao tác với EC2 instance.

Mục tiêu của em là không chỉ làm cho thao tác được phép hoạt động mà còn phải kiểm tra trường hợp bị từ chối, từ đó xác nhận policy thực sự thực thi đúng yêu cầu.

### 2. Nhật ký thực hiện

| STT | Công việc | Bắt đầu | Kết thúc |
|-----|-----------|---------|----------|
| 1 | Tìm hiểu sâu hơn về IAM User, IAM Policy và IAM Role | 01/06/2026 | 07/06/2026 |
| 2 | Tạo IAM User riêng để kiểm tra quyền truy cập giới hạn | 01/06/2026 | 07/06/2026 |
| 3 | Đăng nhập bằng IAM User mới và kiểm tra khả năng truy cập AWS Console | 01/06/2026 | 07/06/2026 |
| 4 | Tạo IAM Policy và phân tích các thành phần `Action`, `Resource`, `Effect` và `Condition` | 01/06/2026 | 07/06/2026 |
| 5 | Tạo IAM Role và kiểm tra trust relationship | 01/06/2026 | 07/06/2026 |
| 6 | Truy cập Amazon EC2 trong Region được chỉ định bằng user giới hạn | 01/06/2026 | 07/06/2026 |
| 7 | Thử thao tác EC2 khi không có tag bắt buộc | 01/06/2026 | 07/06/2026 |
| 8 | Thử thao tác EC2 với tag sai key hoặc value | 01/06/2026 | 07/06/2026 |
| 9 | Thử thao tác EC2 với tag hợp lệ và đối chiếu kết quả | 01/06/2026 | 07/06/2026 |
| 10 | Tạo hoặc gắn Restriction Policy để giới hạn hành động nhạy cảm | 01/06/2026 | 07/06/2026 |
| 11 | Ghi nhận kết quả `Allow`, `Deny` và thông báo access denied | 01/06/2026 | 07/06/2026 |
| 12 | Xóa các EC2 instance và tài nguyên không còn cần thiết sau bài lab | 01/06/2026 | 07/06/2026 |

### 3. Quy trình kỹ thuật đã thực hiện

#### Bước 1: Tạo IAM User giới hạn

Em tạo một IAM User dành riêng cho bài lab thay vì sử dụng tài khoản có quyền quản trị. Sau khi tạo thông tin đăng nhập, em đăng xuất khỏi phiên hiện tại và đăng nhập bằng người dùng mới để kiểm tra từ góc nhìn của người dùng bị giới hạn.

Cách kiểm tra này quan trọng vì việc xem policy từ tài khoản quản trị không phản ánh chính xác trải nghiệm của người dùng thực tế.

#### Bước 2: Phân tích cấu trúc IAM Policy

Khi tạo policy, em tập trung vào ba thành phần chính:

- **Action:** hành động AWS API nào được phép hoặc bị từ chối.
- **Resource:** tài nguyên nào nằm trong phạm vi của policy.
- **Condition:** điều kiện bổ sung, trong bài lab là thông tin tag của tài nguyên hoặc request.

Em hiểu rằng policy có thể cho phép một nhóm hành động nhưng vẫn từ chối request nếu điều kiện tag không thỏa mãn. Đây là cách kiểm soát chi tiết hơn so với việc chỉ cấp quyền theo tên dịch vụ.

#### Bước 3: Tạo và kiểm tra IAM Role

Em tạo IAM Role theo yêu cầu và xem lại trust relationship để xác định đối tượng nào có thể sử dụng role. Sau đó, em kiểm tra các policy được gắn với role và so sánh role với IAM User.

IAM User có thông tin đăng nhập riêng và thường đại diện cho một người dùng, còn role được assume để nhận credential tạm thời. Điều này giúp giảm nhu cầu sử dụng access key dài hạn.

#### Bước 4: Kiểm tra EC2 với điều kiện tag

Em truy cập EC2 Console trong Region được chỉ định và thực hiện các kịch bản kiểm tra:

1. Thao tác khi chưa cung cấp tag bắt buộc.
2. Thao tác với tag không đúng key hoặc value theo yêu cầu.
3. Thao tác với tag phù hợp.
4. Kiểm tra khả năng xem, tạo hoặc quản lý instance sau khi policy được áp dụng.

Kết quả allow hoặc deny được dùng để đối chiếu với logic của policy. Khi một thao tác bị từ chối, em xem thông báo quyền truy cập và quay lại kiểm tra action, resource và condition thay vì cấp thêm quyền rộng ngay lập tức.

#### Bước 5: Áp dụng Restriction Policy

Em tạo hoặc gắn policy nhằm giới hạn những thao tác không được phép trong môi trường lab. Phần này giúp em nhận thấy `Deny` tường minh có mức ưu tiên cao và có thể chặn thao tác ngay cả khi một policy khác cấp `Allow`.

Sau khi hoàn thành kiểm thử, em xóa các instance và tài nguyên không còn cần thiết để tránh phát sinh chi phí.

### 4. Bảng kiểm thử quyền

| Kịch bản | Kết quả mong đợi | Ý nghĩa |
|---------|------------------|--------|
| IAM User đăng nhập và mở EC2 Console | Chỉ thấy hoặc dùng chức năng được cấp | Xác nhận quyền cơ bản |
| Request không có tag bắt buộc | Bị từ chối | Condition đang được thực thi |
| Request có tag không hợp lệ | Bị từ chối | Không thể vượt policy bằng tag sai |
| Request có tag hợp lệ | Được phép trong phạm vi policy | Xác nhận trường hợp đúng |
| Thử hành động nằm trong Restriction Policy | Bị từ chối | Xác nhận giới hạn bảo mật |

### 5. Kết quả và phần việc cá nhân

- Tạo IAM User phục vụ kiểm thử quyền giới hạn.
- Tạo và đọc cấu trúc của IAM Policy.
- Tạo IAM Role và hiểu trust relationship.
- Kiểm tra quyền truy cập EC2 bằng chính người dùng bị giới hạn.
- Thực hiện nhiều kịch bản liên quan đến tag thay vì chỉ kiểm tra trường hợp thành công.
- Hiểu rõ hơn tác động của `Allow`, `Deny` và `Condition`.
- Dọn dẹp tài nguyên sau bài lab.
- Trao đổi với nhóm về cách áp dụng phân quyền vào project.

### 6. Bài học rút ra

Tuần 7 giúp em thay đổi cách nhìn về IAM. Phân quyền không nên được kiểm tra bằng câu hỏi “người dùng có làm được không” mà cần kiểm tra cả “người dùng có bị chặn đúng trong trường hợp không hợp lệ hay không”. Một policy tốt phải đáp ứng đồng thời hai mục tiêu: cho phép công việc cần thiết và ngăn các thao tác ngoài phạm vi.

Việc sử dụng tag trong condition cũng cho em thấy tag không chỉ phục vụ tìm kiếm hoặc quản lý chi phí mà còn có thể trở thành một phần của cơ chế kiểm soát truy cập.