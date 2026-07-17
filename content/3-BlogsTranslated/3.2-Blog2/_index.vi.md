---
title: "Blog 2"
date: 2026-07-16
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS Machine Learning Blog | Claude Apps Gateway for AWS – Điều mình học được sau khi đọc AWS Blog

> **Bài viết gốc:** *Introducing Claude apps gateway for AWS*
> **Tác giả:** Dani Mitchell, Ayan Ray, Sofian Hamiti và Harshetha Narayan
> **Ngày xuất bản:** 08/07/2026 trên AWS Machine Learning Blog

## Vì sao mình viết bài này?

Trong quá trình tìm hiểu các dịch vụ AI trên AWS, mình thường đọc AWS Machine Learning Blog để cập nhật công nghệ mới. Bài viết về Claude Apps Gateway giúp mình nhận ra rằng khi doanh nghiệp mở rộng việc sử dụng AI, câu hỏi quan trọng không chỉ là mô hình mạnh đến đâu mà còn là cách quản lý danh tính, quyền truy cập, chính sách, dữ liệu và chi phí. Mình viết bài này để ghi lại góc nhìn đó và liên hệ nó với những dịch vụ hạ tầng AWS mình đã tiếp cận trong kỳ thực tập.

## Ấn tượng ban đầu của mình

Ban đầu, mình nghĩ đây chỉ là một công cụ giúp kết nối Claude với AWS. Sau khi đọc xong, mình nhận ra AWS và Anthropic đang giới thiệu một lớp kiểm soát tập trung dành cho **Claude Code** và **Claude Desktop** trong doanh nghiệp.

Khi AI được sử dụng bởi nhiều nhóm, việc cấp một thông tin xác thực riêng cho từng người, cấu hình thủ công trên từng máy và theo dõi chi phí rời rạc sẽ nhanh chóng trở nên khó quản lý. Claude Apps Gateway giải quyết bài toán này bằng một điểm kiểm soát chung.

## Claude Apps Gateway mang lại điều gì?

Gateway hoạt động như một lớp trung gian giữa người dùng và dịch vụ AI. Yêu cầu không được gửi trực tiếp từ máy của lập trình viên đến mô hình mà đi qua Gateway để được xác thực, áp dụng chính sách, ghi nhận mức sử dụng và định tuyến đến **Amazon Bedrock** hoặc **Claude Platform on AWS**.

Năm trách nhiệm chính của Gateway gồm:

- **Identity:** kết nối với nhà cung cấp danh tính hỗ trợ OIDC và sử dụng đăng nhập SSO; phiên làm việc dùng token ngắn hạn nên không cần lưu secret dài hạn trên máy lập trình viên.
- **Policy:** quản trị viên cấu hình tập trung model được phép dùng, quyền của công cụ và các thiết lập mặc định theo nhóm người dùng.
- **Telemetry:** dữ liệu sử dụng có thể được chuyển qua OTLP đến Amazon CloudWatch, Amazon Managed Service for Prometheus hoặc nền tảng quan sát khác.
- **Routing:** Gateway giữ thông tin xác thực upstream và định tuyến yêu cầu đến Amazon Bedrock hoặc Claude Platform on AWS, đồng thời hỗ trợ nhiều Region hoặc nhiều tài khoản.
- **Spend caps:** có thể đặt giới hạn chi tiêu theo ngày, tuần hoặc tháng cho tổ chức, nhóm và từng người dùng.

Theo mình, cách tiếp cận này khá giống việc dùng API Gateway để quản lý API, nhưng được mở rộng cho cách các ứng dụng Claude hoạt động trong doanh nghiệp.

## Cách triển khai và đăng nhập

Gateway là một container stateless có thể chạy trong mạng riêng trên Amazon ECS, Amazon EKS hoặc Amazon EC2. Một cơ sở dữ liệu Amazon RDS for PostgreSQL lưu trạng thái đăng nhập ngắn hạn và bộ đếm giới hạn. Gateway có thể được đặt sau Application Load Balancer nội bộ với chứng chỉ TLS từ AWS Certificate Manager.

Cấu hình được đọc từ một tệp YAML, còn secret nằm trong biến môi trường. Khi dùng Amazon Bedrock, workload có thể sử dụng IAM role của container thay vì static credential.

Sau khi hệ thống được triển khai, lập trình viên chạy `claude /login`, đăng nhập bằng SSO của doanh nghiệp và tiếp tục sử dụng Claude Code như bình thường. Điểm khác biệt nằm phía sau: mọi yêu cầu đều qua Gateway, chỉ các model được cho phép mới xuất hiện, hoạt động được gắn với danh tính người dùng và chi phí được tính vào hạn mức tương ứng.

## Điều mình học được sau bài viết này

Điểm mình ấn tượng nhất là bài viết không chỉ tập trung vào khả năng của mô hình AI mà còn quan tâm đến việc vận hành AI trong môi trường doanh nghiệp. Khi số lượng người dùng tăng, quản lý quyền truy cập, theo dõi hoạt động và kiểm soát chi phí quan trọng không kém việc lựa chọn mô hình.

Trong kỳ thực tập tại AWS, mình chủ yếu tiếp cận các dịch vụ hạ tầng như Amazon EC2, Amazon S3, AWS Backup và Storage Gateway. Bài viết này giúp mình có thêm một góc nhìn mới: triển khai AI không dừng ở việc gọi được mô hình. Doanh nghiệp còn cần một control plane để quản lý người dùng, chính sách, định tuyến, quan sát và ngân sách một cách tập trung.

Mình cũng hiểu rõ hơn sự khác nhau giữa hai hướng triển khai được bài viết đề cập:

- Chọn **Amazon Bedrock** khi cần giữ dữ liệu trong ranh giới bảo mật AWS và sử dụng các cơ chế quản trị quen thuộc của tài khoản AWS.
- Chọn **Claude Platform on AWS** khi muốn trải nghiệm nền tảng Claude gốc nhưng vẫn dùng xác thực và thanh toán qua AWS.

## Kết luận của mình

Đối với mình, *Introducing Claude apps gateway for AWS* không chỉ giới thiệu một giải pháp mới mà còn mang đến góc nhìn thực tế về cách doanh nghiệp triển khai AI an toàn và có kiểm soát. Khi AI đi vào production, identity, policy, telemetry, routing và cost governance phải được thiết kế ngay từ đầu.

Cảm ơn mọi người đã đọc bài chia sẻ của mình. Mình rất mong được lắng nghe và trao đổi thêm với mọi người về việc vận hành các ứng dụng AI trên AWS.

Link bài viết tham khảo: [Introducing Claude apps gateway for AWS](https://aws.amazon.com/blogs/machine-learning/introducing-claude-apps-gateway-for-aws/)