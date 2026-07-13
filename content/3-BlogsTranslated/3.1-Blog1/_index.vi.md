---
title: "Blog 1"
date: 2026-06-17
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# S&P Global xây dựng chiến lược Disaster Recovery với Amazon FSx for NetApp ONTAP

Trong lúc tìm hiểu thêm về AWS, mình đọc được một bài blog khá hay trên AWS Architecture Blog nói về cách S&P Global Market Intelligence xây dựng chiến lược Disaster Recovery (DR) bằng Amazon FSx for NetApp ONTAP.

Trước đây mình thường hiểu Disaster Recovery đơn giản là backup dữ liệu để khi có sự cố thì khôi phục lại. Nhưng sau khi đọc bài này, mình nhận ra trong các hệ thống lớn, đặc biệt là hệ thống tài chính, DR không chỉ là sao lưu dữ liệu. DR còn là cách thiết kế để hệ thống vẫn có thể tiếp tục phục vụ người dùng trong thời gian ngắn nhất khi một Region hoặc một môi trường chính gặp sự cố.

![alt text](image-2.png)

## Tổng quan bài viết
Bài viết mô tả cách S&P Global Market Intelligence triển khai giải pháp DR cho nền tảng Capital IQ. Đây là hệ thống phục vụ dữ liệu tài chính cho khách hàng toàn cầu, nên yêu cầu về tính sẵn sàng, tính nhất quán dữ liệu, RTO và RPO rất quan trọng.

Điểm nổi bật của giải pháp là hệ thống có thể chuyển sang môi trường DR ở chế độ read-only trong vòng dưới 15 phút. Sau đó, nếu cần vận hành đầy đủ, đội ngũ có thể tiếp tục chuyển môi trường DR sang chế độ read-write theo quy trình được kiểm soát.

## Kiến trúc chính
Giải pháp trong bài được triển khai theo mô hình đa Region:

- **US-East-1** đóng vai trò Primary Region.
- **US-West-2** đóng vai trò DR Region.
- **Các SQL Server** chạy trên Amazon EC2 và được tổ chức theo mô hình Windows Server Failover Cluster.
- **Mỗi Region** có một hệ thống Amazon FSx for NetApp ONTAP.
- **Dữ liệu** được đồng bộ từ Region chính sang Region DR bằng SnapMirror replication.
- **FlexClone** được dùng để tạo volume từ snapshot mới nhất ở DR Region, giúp hệ thống có bản dữ liệu sẵn sàng cho truy cập read-only.

## Các thành phần quan trọng

### Amazon FSx for NetApp ONTAP
Amazon FSx for NetApp ONTAP là dịch vụ file storage được quản lý bởi AWS, hỗ trợ các tính năng quen thuộc của NetApp ONTAP như snapshot, replication và clone dữ liệu. Trong bài này, FSx for ONTAP đóng vai trò lớp lưu trữ chính cho dữ liệu SQL Server cần được bảo vệ.

### Snapshot
Snapshot lưu lại trạng thái dữ liệu tại một thời điểm cụ thể. Đây là nền tảng để hệ thống có thể tạo bản sao dữ liệu phục vụ khôi phục hoặc kiểm thử mà không cần copy toàn bộ dữ liệu theo cách thủ công.

### SnapMirror
SnapMirror được dùng để replication dữ liệu từ Primary Region sang DR Region. Theo bài viết, quá trình replication được cấu hình theo chu kỳ 15 phút, giúp DR Region luôn có bản dữ liệu gần với production.

### FlexClone
FlexClone là điểm mình thấy thú vị nhất trong bài. Thay vì phải dựng lại toàn bộ môi trường từ đầu, hệ thống có thể tạo một clone từ snapshot đã được replication sang DR Region. Clone này hoạt động độc lập với quá trình replication, nên vừa có thể phục vụ truy cập read-only, vừa không làm gián đoạn việc đồng bộ dữ liệu tiếp theo.

## Quy trình khôi phục
Bài viết chia hướng tiếp cận DR thành hai giai đoạn:

1. **Failover nhanh sang read-only**: sử dụng snapshot và FlexClone để cung cấp môi trường DR có thể truy cập dữ liệu trong thời gian ngắn.
2. **Chuyển sang read-write khi cần**: dừng ghi ở Primary Region, cập nhật SnapMirror lần cuối, break replication relationship, rồi chuyển SQL Server resources sang DR Region.
Cách làm này cho phép người dùng vẫn có thể truy cập dữ liệu quan trọng trong lúc đội ngũ kỹ thuật xử lý quá trình khôi phục đầy đủ.

## Bài học rút ra

Với mình là một sinh viên mới học AWS, bài này giúp mình hiểu rõ hơn rằng khi thiết kế hệ thống trên cloud, mình không chỉ cần quan tâm đến việc deploy được ứng dụng. Mình còn cần nghĩ đến các tình huống như:

- Khi hệ thống gặp sự cố thì xử lý thế nào?
- Dữ liệu có thể mất bao nhiêu trong trường hợp xấu nhất?
- Người dùng có còn truy cập được dữ liệu quan trọng không?
- Hệ thống cần khôi phục trong bao lâu?
- Quy trình failover và fallback có được kiểm thử trước không?
- Bài viết cũng giúp mình hiểu rằng Disaster Recovery là sự kết hợp giữa kiến trúc, dữ liệu, vận hành và quy trình. Backup là một phần quan trọng, nhưng chưa đủ để tạo ra một chiến lược DR hoàn chỉnh.

## Kết luận

Mình vẫn chưa hiểu hết toàn bộ các kỹ thuật trong bài, đặc biệt là phần Amazon FSx for NetApp ONTAP, SnapMirror và FlexClone. Tuy vậy, đây là một case study rất đáng đọc vì nó cho thấy cách một hệ thống lớn thiết kế DR không chỉ để khôi phục dữ liệu, mà còn để giảm downtime và duy trì khả năng truy cập cho người dùng.

Link bài viết tham khảo: https://aws.amazon.com/vi/blogs/architecture/sp-globals-innovative-disaster-recovery-strategy-using-amazon-fsx-for-netapp-ontap-snapshots/
