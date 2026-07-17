---
title: "Nhật ký công việc Tuần 6"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### 1. Mục tiêu và bối cảnh

Tuần 6 tiếp tục phát triển từ bài thực hành S3 của tuần trước. Em tập trung vào ba nhóm nội dung: bảo vệ S3 origin bằng cách hạn chế truy cập công khai, phân phối nội dung qua **Amazon CloudFront**, và tăng khả năng bảo vệ dữ liệu bằng **S3 Versioning** cùng sao chép object giữa các Region.

Mục tiêu không chỉ là tạo tài nguyên mà còn phải kiểm tra luồng truy cập: người dùng lấy nội dung qua CloudFront, S3 giữ vai trò origin, các phiên bản object được lưu lại và dữ liệu có thể được sao chép sang bucket ở Region khác.

### 2. Nhật ký thực hiện

| STT | Công việc | Bắt đầu | Kết thúc |
|-----|-----------|---------|----------|
| 1 | Rà soát lại cấu hình Amazon S3 và bật Block All Public Access | 25/05/2026 | 31/05/2026 |
| 2 | Kiểm tra việc hạn chế truy cập trực tiếp vào S3 origin | 25/05/2026 | 31/05/2026 |
| 3 | Tạo Amazon CloudFront distribution sử dụng S3 làm origin | 25/05/2026 | 31/05/2026 |
| 4 | Kiểm tra origin, cache behavior, viewer protocol và distribution domain | 25/05/2026 | 31/05/2026 |
| 5 | Truy cập và kiểm tra nội dung thông qua CloudFront domain | 25/05/2026 | 31/05/2026 |
| 6 | Thay đổi nội dung tại S3 origin và kiểm tra ảnh hưởng của bộ nhớ đệm | 25/05/2026 | 31/05/2026 |
| 7 | Bật S3 Bucket Versioning cho bucket thực hành | 25/05/2026 | 31/05/2026 |
| 8 | Tải nhiều phiên bản của cùng một object và kiểm tra Version ID | 25/05/2026 | 31/05/2026 |
| 9 | Tìm hiểu cách khôi phục object bị ghi đè hoặc xóa nhầm | 25/05/2026 | 31/05/2026 |
| 10 | Tạo bucket đích tại Region khác và bật Versioning | 25/05/2026 | 31/05/2026 |
| 11 | Cấu hình replication rule và IAM Role cho Amazon S3 | 25/05/2026 | 31/05/2026 |
| 12 | Tải object mới lên bucket nguồn và kiểm tra kết quả tại bucket đích | 25/05/2026 | 31/05/2026 |

### 3. Quy trình kỹ thuật đã thực hiện

#### Bước 1: Hạn chế truy cập trực tiếp vào S3

Sau bài website tĩnh, em bật lại chế độ chặn truy cập công khai để hiểu mô hình trong đó S3 không còn được sử dụng như một website endpoint công khai. Mục tiêu là giảm khả năng người dùng đi thẳng đến origin và chuyển việc phân phối nội dung sang CloudFront.

Em kiểm tra các policy hoặc thiết lập public access trước đó để đảm bảo cấu hình mới không còn cấp quyền rộng ngoài yêu cầu. Qua đó, em thấy rằng cấu hình bảo mật cần thay đổi theo kiến trúc sử dụng: cùng một bucket nhưng cách cấp quyền sẽ khác khi dùng website endpoint và khi dùng CloudFront làm lớp phân phối.

#### Bước 2: Tạo CloudFront distribution

Em tạo CloudFront distribution và chọn S3 bucket làm origin. Trong quá trình cấu hình, em chú ý đến:

- Origin được sử dụng để lấy nội dung.
- Cách CloudFront truy cập origin.
- Cache behavior mặc định.
- Giao thức người dùng sử dụng khi truy cập distribution.
- Tên miền distribution dùng để kiểm tra.

Sau khi distribution hoàn tất triển khai, em truy cập nội dung bằng domain của CloudFront và xác nhận tệp được trả về. Em cũng kiểm tra lại S3 để bảo đảm việc hạn chế public access không làm mất khả năng CloudFront lấy nội dung theo cơ chế đã cấu hình.

#### Bước 3: Kiểm tra cache và cập nhật nội dung

Khi thay đổi một tệp ở origin, nội dung trả về từ CloudFront có thể chưa cập nhật ngay vì cache. Qua bước kiểm tra này, em hiểu rằng cần xem xét thời gian lưu cache, tên object hoặc cơ chế invalidation khi muốn đưa phiên bản mới đến người dùng nhanh hơn.

Điều này cho thấy CDN cải thiện hiệu năng bằng cách giảm số lần truy cập origin, nhưng người vận hành cũng cần quản lý vòng đời cache khi nội dung thay đổi.

#### Bước 4: Bật S3 Versioning

Em bật Versioning cho bucket và tải nhiều phiên bản của cùng một object. Trên S3 Console, em bật chế độ hiển thị version để so sánh Version ID, thời điểm cập nhật và trạng thái hiện tại của object.

Em cũng xem xét trường hợp object bị ghi đè hoặc xóa. Khi versioning được bật, phiên bản cũ vẫn có thể tồn tại, còn thao tác xóa có thể tạo delete marker thay vì xóa vĩnh viễn toàn bộ lịch sử ngay lập tức. Nội dung này giúp em hiểu rõ hơn cách S3 hỗ trợ phục hồi sau lỗi thao tác.

#### Bước 5: Cấu hình sao chép đa Region

Em tạo bucket đích tại Region khác và chuẩn bị replication rule từ bucket nguồn. Trước khi cấu hình, em kiểm tra các điều kiện quan trọng:

- Versioning phải được bật trên cả bucket nguồn và bucket đích.
- S3 cần một IAM Role có quyền đọc phiên bản ở nguồn và ghi object vào đích.
- Rule cần xác định phạm vi object được sao chép.
- Object mới sau khi rule có hiệu lực cần được dùng để kiểm tra.

Sau khi cấu hình, em tải object thử nghiệm lên bucket nguồn và kiểm tra bucket đích. Qua bài lab, em phân biệt được replication với thao tác copy thủ công: replication là cơ chế tự động dựa trên rule và quyền đã thiết lập.

### 4. Các điểm cần kiểm tra

- Distribution chưa triển khai xong nhưng đã kiểm tra domain.
- Origin hoặc đường dẫn object không đúng.
- Nội dung cũ còn trong cache của CloudFront.
- Versioning chỉ được bật ở một bucket khi cấu hình replication.
- IAM Role của replication thiếu quyền trên bucket nguồn hoặc đích.
- Kiểm tra object đã tồn tại trước khi rule có hiệu lực thay vì dùng object mới.

Việc ghi lại các điểm này giúp em hiểu tại sao một cấu hình có thể đúng về mặt giao diện nhưng chưa tạo ra kết quả như mong muốn.

### 5. Kết quả và phần việc cá nhân

- Chuyển mô hình truy cập từ S3 public sang phân phối nội dung qua CloudFront.
- Tạo và kiểm tra CloudFront distribution với S3 origin.
- Hiểu ảnh hưởng của cache khi cập nhật nội dung.
- Bật S3 Versioning và kiểm tra nhiều phiên bản của một object.
- Hiểu vai trò của Version ID và delete marker trong quá trình phục hồi.
- Tạo bucket tại Region khác và thực hành cấu hình replication.
- Kiểm tra các điều kiện về versioning và IAM Role trước khi sao chép dữ liệu.

### 6. Bài học rút ra

Tuần 6 giúp em hiểu rõ hơn cách kết hợp hiệu năng, bảo mật và khả năng phục hồi dữ liệu. CloudFront giúp nội dung được phân phối qua một lớp trung gian thay vì để người dùng truy cập trực tiếp origin. Versioning giúp giảm rủi ro khi object bị thay đổi hoặc xóa nhầm, còn replication mở rộng khả năng bảo vệ dữ liệu sang Region khác.

Em cũng học được rằng mỗi tính năng đều có điều kiện hoạt động cụ thể. Nếu bỏ qua trạng thái triển khai, cache, versioning hoặc IAM Role, kết quả kiểm tra có thể không đúng dù tài nguyên đã được tạo.