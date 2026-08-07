---
title: "Blog 2"
date: 2026-08-05
weight: 2
chapter: false
pre: "<b>3.2.</b>"
description: "Ba chiến lược tối ưu chi phí hạ tầng AWS: compute, lưu trữ và tài nguyên Dev/Test."
---

# Giảm đến 60% chi phí hạ tầng AWS cho Start-up và Doanh nghiệp

Khi triển khai và vận hành hệ thống trên AWS, **tối ưu chi phí** luôn là một trong những ưu tiên hàng đầu đối với Cloud Engineer cũng như doanh nghiệp.

Bài viết này giới thiệu ba chiến lược thực tế để tối ưu chi phí hạ tầng AWS: **tối ưu tài nguyên tính toán**, **quản lý vòng đời lưu trữ**, và **tự động bật/tắt tài nguyên Dev/Test**.

{{< figure
src="/fcj-report/images/3-BlogsPosted/blog2-aws-cost-optimization.png"
alt="Ba chiến lược tối ưu chi phí hạ tầng AWS"
title="Ba chiến lược tối ưu chi phí hạ tầng AWS">}}

> Các tỷ lệ tiết kiệm trong bài viết là giá trị tối đa hoặc ước tính trong từng trường hợp cụ thể. Hiệu quả thực tế còn phụ thuộc vào workload, Region, cấu hình, mô hình thanh toán và thời gian sử dụng.

---

## 1. Tối ưu Compute với AWS Graviton và Savings Plans

Các dịch vụ tính toán như **Amazon EC2, Amazon ECS và Amazon EKS** thường chiếm phần lớn chi phí trên hóa đơn AWS. Hai phương pháp phổ biến là:

- **Chuyển sang AWS Graviton:** Các dòng máy chủ ARM như **T4g, C6g và M6g** sử dụng bộ xử lý do AWS thiết kế. Theo AWS, Graviton có thể mang lại hiệu năng cao hơn tới **40%** và giảm chi phí tới **20%** so với các máy chủ x86 tương đương đối với một số workload. Trước khi triển khai production cần kiểm tra khả năng tương thích ARM, các thư viện native và hiệu năng của ứng dụng.
- **Sử dụng Savings Plans hoặc Reserved Instances:** Đối với các workload chạy ổn định 24/7, có thể cam kết sử dụng trong **1 hoặc 3 năm**. Compute Savings Plans giúp giảm chi phí tới **66%** so với On-Demand Pricing nhưng vẫn linh hoạt khi sử dụng EC2, ECS Fargate và AWS Lambda theo điều kiện của gói.

---

## 2. Quản lý lưu trữ với Amazon S3 Lifecycle

Nếu không phân loại dữ liệu theo thời gian sử dụng, chi phí Amazon S3 sẽ tăng dần theo thời gian.

S3 Lifecycle Rules cho phép tự động chuyển dữ liệu sang lớp lưu trữ phù hợp:

- **S3 Standard:** dành cho dữ liệu mới hoặc thường xuyên truy cập.
- **S3 Standard-IA / One Zone-IA:** dành cho dữ liệu ít truy cập nhưng vẫn yêu cầu thời gian phản hồi tính bằng mili giây. Cần cân nhắc thời gian lưu trữ tối thiểu, phí truy xuất và yêu cầu về tính sẵn sàng trước khi chuyển dữ liệu.
- **S3 Glacier Flexible Retrieval / Glacier Deep Archive:** phù hợp với backup, log và dữ liệu lưu trữ dài hạn không cần truy cập tức thời. Chi phí Glacier Deep Archive có thể chỉ khoảng **0.00099 USD/GB/tháng** tại một số Region, tuy nhiên mức giá và phí truy xuất sẽ khác nhau giữa các Region.

Nếu chưa xác định được quy luật truy cập dữ liệu, có thể sử dụng **S3 Intelligent-Tiering**. Dịch vụ này sẽ tự động theo dõi tần suất truy cập và chuyển dữ liệu giữa các lớp lưu trữ phù hợp, đi kèm một khoản phí nhỏ cho việc giám sát và tự động hóa.

---

## 3. Tự động bật/tắt tài nguyên Dev/Test

Các môi trường Development hoặc Testing thường chỉ được sử dụng trong giờ làm việc.

Có thể sử dụng **AWS Instance Scheduler** hoặc **AWS Lambda kết hợp Amazon EventBridge** để tự động:

- Dừng EC2 và RDS lúc **19:00** mỗi ngày.
- Khởi động lại lúc **08:00** sáng hôm sau.

Nếu môi trường chỉ hoạt động khoảng **8 giờ/ngày và 5 ngày/tuần**, phương pháp này có thể giúp giảm khoảng **65–70%** chi phí compute của môi trường không phải production.

Tuy nhiên, cần lưu ý rằng các chi phí như **Storage, Backup, Elastic IP/Public IPv4** và một số tài nguyên liên quan vẫn tiếp tục được tính phí. Đối với Amazon RDS, một DB Instance chỉ có thể dừng tối đa **7 ngày**, sau đó AWS sẽ tự động khởi động lại.

---

## Kết luận

Tối ưu chi phí AWS không phải là công việc thực hiện một lần mà là quá trình liên tục.

Để kiểm soát ngân sách hiệu quả, nên kết hợp:

- **AWS Cost Explorer** để phân tích xu hướng chi tiêu.
- **AWS Budgets** để thiết lập cảnh báo khi vượt ngân sách.
- **Amazon CloudWatch** để theo dõi hiệu năng và thực hiện right-sizing tài nguyên.

---

## Tài liệu tham khảo

- AWS Graviton Fast Start
- AWS Compute Savings Plans
- Amazon S3 User Guide – Lifecycle Transitions
- Amazon S3 User Guide – Intelligent-Tiering
- Amazon S3 Pricing
- Amazon RDS User Guide – Stopping a DB Instance

---

## Bài viết đã đăng

{{% button href="https://www.facebook.com/groups/awsstudygroupfcj/permalink/2234073587357601/" icon="fab fa-facebook" %}}
Xem Blog 2 trên AWS Study Group VN
{{% /button %}}
