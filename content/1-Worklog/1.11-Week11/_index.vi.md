---
title: "Nhật ký công việc Tuần 11"
date: 2026-06-29
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### 1. Mục tiêu và bối cảnh

Tuần 11 là giai đoạn tích hợp và kiểm thử project nhóm theo kiến trúc đã thống nhất. Các tài nguyên chính gồm **Amazon EC2, Amazon S3, IAM Role và AWS Storage Gateway**. Thay vì chỉ xác nhận tài nguyên đã được tạo, nhóm cần kiểm tra quyền truy cập, kết nối, luồng dữ liệu, trạng thái dịch vụ và khả năng dọn dẹp sau khi hoàn thành.

Phần việc em tập trung ghi nhận gồm rà soát cấu hình, thực hiện các kịch bản kiểm thử, đối chiếu kết quả và tổng hợp tài liệu phục vụ báo cáo cuối kỳ.

### 2. Kiến trúc và luồng kiểm thử

Luồng hệ thống được kiểm tra theo thứ tự:

1. EC2 hoặc máy On-premises có kết nối mạng phù hợp.
2. Storage Gateway ở trạng thái hoạt động.
3. File Share có thể được mount từ client được cho phép.
4. IAM Role cho phép gateway truy cập đúng S3 bucket.
5. Tệp được tạo từ client xuất hiện thành object trong S3.
6. Các thao tác ngoài phạm vi bị từ chối.
7. Tài nguyên không còn sử dụng được nhận diện và xóa.

Việc kiểm tra theo luồng giúp nhóm xác định lỗi nằm ở lớp nào: compute, network, gateway, share, IAM hay S3.

### 3. Nhật ký thực hiện

| STT | Công việc | Bắt đầu | Kết thúc |
|-----|-----------|---------|----------|
| 1 | Kiểm tra trạng thái của Amazon EC2, Amazon S3 và AWS Storage Gateway | 29/06/2026 | 05/07/2026 |
| 2 | Rà soát cấu hình mạng giữa EC2, máy On-premises và gateway | 29/06/2026 | 05/07/2026 |
| 3 | Kiểm tra trust relationship và permission policy của IAM Role | 29/06/2026 | 05/07/2026 |
| 4 | Đối chiếu quyền IAM Role với bucket và prefix được project sử dụng | 29/06/2026 | 05/07/2026 |
| 5 | Mount File Share từ máy client được phép | 29/06/2026 | 05/07/2026 |
| 6 | Liệt kê thư mục, tạo tệp, ghi dữ liệu và đọc lại nội dung | 29/06/2026 | 05/07/2026 |
| 7 | Kiểm tra tệp được đồng bộ thành object trong Amazon S3 | 29/06/2026 | 05/07/2026 |
| 8 | Đối chiếu tên, kích thước, thời gian cập nhật và object key | 29/06/2026 | 05/07/2026 |
| 9 | Thực hiện kịch bản client không hợp lệ hoặc role thiếu quyền | 29/06/2026 | 05/07/2026 |
| 10 | Ghi nhận kết quả của trường hợp thành công và trường hợp bị từ chối | 29/06/2026 | 05/07/2026 |
| 11 | Cập nhật sơ đồ kiến trúc, quy trình kiểm thử và báo cáo project | 29/06/2026 | 05/07/2026 |
| 12 | Kiểm kê, dừng hoặc xóa các tài nguyên không còn cần thiết | 29/06/2026 | 05/07/2026 |

### 4. Kiểm thử chi tiết

#### Kiểm thử 1: Trạng thái tài nguyên

Em kiểm tra EC2 instance, gateway và bucket trước khi thực hiện kết nối. Nếu instance hoặc gateway chưa sẵn sàng, kết quả mount không thể dùng để đánh giá policy hoặc File Share.

#### Kiểm thử 2: Quyền IAM Role

Em xem policy gắn với role và đối chiếu với bucket được sử dụng. Các nội dung kiểm tra gồm:

- Role có được đúng dịch vụ assume hay không.
- Quyền đọc/ghi có giới hạn đúng bucket hoặc prefix cần thiết hay không.
- Có hành động quản trị không cần thiết trong policy hay không.
- Khi thiếu quyền, thông báo lỗi xuất hiện tại bước nào.

Mục tiêu là để hệ thống hoạt động mà không phải sử dụng policy quản trị toàn phần.

#### Kiểm thử 3: Mount và thao tác tệp

Từ client, em mount File Share và thực hiện:

1. Liệt kê nội dung thư mục.
2. Tạo một tệp mới.
3. Ghi dữ liệu vào tệp.
4. Đọc lại nội dung.
5. Kiểm tra object tương ứng trên S3.

Kịch bản này xác minh cả kết nối lẫn quyền thao tác dữ liệu.

#### Kiểm thử 4: Luồng dữ liệu end-to-end

Em đối chiếu tên tệp, kích thước, thời gian cập nhật và key của object trên S3. Nếu dữ liệu không xuất hiện đúng, em kiểm tra từ client về gateway thay vì chỉ tải lại S3 Console.

#### Kiểm thử 5: Trường hợp bị từ chối

Nhóm xem xét các trường hợp như client không nằm trong phạm vi cho phép, role thiếu quyền hoặc thao tác ngoài policy. Mục tiêu của kiểm thử âm là xác nhận hệ thống chặn đúng chứ không chỉ hoạt động trong trường hợp thuận lợi.

### 5. Bảng kết quả kiểm thử

| Thành phần | Kịch bản | Kết quả cần đạt |
|------------|----------|-----------------|
| EC2/Client | Kiểm tra kết nối đến gateway | Kết nối được theo phạm vi mạng đã cấu hình |
| Storage Gateway | Kiểm tra trạng thái | Gateway hoạt động và có thể phục vụ File Share |
| File Share | Mount và đọc thư mục | Client hợp lệ truy cập được |
| IAM Role | Ghi object vào bucket chỉ định | Cho phép đúng bucket, từ chối ngoài phạm vi |
| S3 | Đối chiếu tệp và object | Tên, kích thước và đường dẫn phù hợp |
| Cleanup | Kiểm kê tài nguyên | Không giữ lại tài nguyên không cần thiết |

### 6. Phần việc cá nhân

- Rà soát trạng thái và cấu hình của các tài nguyên tham gia luồng kiểm thử.
- Kiểm tra policy của IAM Role và đối chiếu với S3 bucket.
- Thực hiện mount, tạo tệp và xác minh object trên S3.
- Ghi lại kết quả của kịch bản thành công và trường hợp bị từ chối.
- Cập nhật nội dung mô tả kiến trúc và quy trình kiểm thử.
- Tham gia cleanup và rà soát tài nguyên có thể phát sinh chi phí.

### 7. Tối ưu chi phí và cleanup

Em lập danh sách tài nguyên đã sử dụng và kiểm tra trạng thái trước khi kết thúc:

- Dừng hoặc xóa EC2 instance không còn cần thiết.
- Xóa gateway hoặc File Share sau khi xác nhận không còn dùng.
- Kiểm tra S3 object và bucket trước khi xóa.
- Rà soát các role/policy chỉ được tạo cho bài lab.
- Xác nhận không còn tác vụ hoặc tài nguyên đang chạy ngoài kế hoạch.

Cleanup được xem là một phần của bài thực hành, không phải bước phụ. Trong môi trường cloud, tài nguyên còn tồn tại có thể tiếp tục phát sinh chi phí ngay cả khi người dùng không truy cập.

### 8. Bài học rút ra

Tuần 11 giúp em hiểu sự khác nhau giữa “tạo xong tài nguyên” và “hoàn thành hệ thống”. Một hệ thống chỉ được xem là hoàn thành khi luồng dữ liệu được kiểm tra end-to-end, quyền đúng phạm vi, trường hợp sai bị chặn và tài nguyên được quản lý sau sử dụng.

Việc ghi lại test case và kết quả cũng giúp báo cáo thể hiện rõ phần việc thực tế hơn, đồng thời hỗ trợ nhóm xác định nguyên nhân khi cấu hình thay đổi.