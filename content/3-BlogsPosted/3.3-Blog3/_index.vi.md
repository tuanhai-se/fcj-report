---
title: "Blog 3"
date: 2026-08-06
weight: 3
chapter: false
pre: "<b>3.3.</b>"
description: "Kiến trúc bảo mật nhiều lớp của EduFlow với VPC, ALB, ECS Fargate, RDS, Secrets Manager, CloudWatch và CI/CD."
---

# EduFlow trên AWS: Xây dựng kiến trúc Defense in Depth với VPC và ECS Fargate

Khi thiết kế hạ tầng Cloud cho các ứng dụng Web hoặc Microservices, bảo mật cần được xem là một phần của kiến trúc bên cạnh hiệu năng và chi phí. Mô hình triển khai của **EduFlow** áp dụng hai nguyên tắc **Defense in Depth** và **Least Privilege** nhằm giảm bề mặt tấn công và bảo vệ dữ liệu người dùng.

{{< figure
src="/fcj-report/images/3-BlogsPosted/blog3-facebook-post-1.png"
alt="Phần đầu bài viết Facebook về kiến trúc bảo mật EduFlow trên AWS"
title="Blog 3 trên Facebook - Phần 1">}}

## I. Bảo vệ mạng – Cô lập bằng Amazon VPC

Kiến trúc triển khai của EduFlow tách riêng ba lớp: điểm truy cập công khai, tầng ứng dụng và tầng dữ liệu.

- **Application Load Balancer là điểm truy cập duy nhất từ Internet:** ALB tiếp nhận toàn bộ lưu lượng từ bên ngoài và chuyển tiếp các request hợp lệ đến frontend hoặc backend. Security Group chỉ cho phép các cổng cần thiết giữa các tầng.
- **Frontend và Backend chạy trong Private Subnet:** hai dịch vụ `FE_EduFlow` và `BE_EduFlow` được triển khai trên Amazon ECS Fargate mà không công khai trực tiếp cổng ứng dụng ra Internet. Trong mô hình private hoàn chỉnh, các task không có Public IP và truy cập Internet thông qua NAT Gateway hoặc VPC Endpoint.
- **Amazon RDS nằm trong Private Data Subnet:** cơ sở dữ liệu không có đường kết nối trực tiếp từ Internet. Security Group của RDS chỉ cho phép backend kết nối thông qua cổng MySQL.

---

## II. Bảo vệ ứng dụng và dữ liệu – Container Security

- **Tách riêng Frontend và Backend:** `FE_EduFlow` chạy trên cổng **8080**, còn `BE_EduFlow` chạy trên **8888**. Hai dịch vụ sử dụng image Docker và Task Definition riêng giúp việc triển khai độc lập và giao tiếp giữa các dịch vụ rõ ràng hơn.
- **Cơ sở dữ liệu được cô lập:** Amazon RDS for MySQL lưu trữ cơ sở dữ liệu `eduflow_db` trong private subnet và từ chối mọi kết nối trực tiếp từ Internet.
- **Không hard-code thông tin nhạy cảm:** mật khẩu database, JWT Secret, SMTP và VNPay Credentials được lưu trong **AWS Secrets Manager**. ECS Task Definition chỉ tham chiếu đến từng khóa (JSON key) và truyền giá trị vào container khi khởi động.
- **IAM theo nguyên tắc Least Privilege:** ECS Execution Role chỉ được cấp các quyền tối thiểu cần thiết để pull image từ Amazon ECR, ghi log lên CloudWatch và đọc secret từ Secrets Manager.

{{< figure
src="/fcj-report/images/3-BlogsPosted/blog3-facebook-post-2.png"
alt="Phần hai bài viết Facebook về Secrets Manager và CloudWatch"
title="Blog 3 trên Facebook - Phần 2">}}

---

## III. Giám sát và khôi phục tự động – Observability và Health Check

Bảo mật không chỉ là ngăn chặn tấn công mà còn phải nhanh chóng phát hiện và xử lý khi hệ thống có dấu hiệu bất thường.

- **Amazon CloudWatch Logs:** frontend và backend sử dụng driver `awslogs` để gửi log container về các CloudWatch Log Group riêng biệt. Việc tập trung log giúp hỗ trợ giám sát, điều tra sự cố và thiết lập cảnh báo.
- **Actuator Health Check:** Backend được ALB kiểm tra thông qua endpoint `/actuator/health`, trong khi frontend được kiểm tra tại `/`. Nếu một container không vượt qua Health Check, ALB sẽ tự động ngừng chuyển request đến container đó và ECS Service Scheduler sẽ tạo task mới để duy trì `desired_count`.
- **Không cần SSH vào máy chủ:** ECS Fargate là dịch vụ managed compute, vì vậy việc vận hành tập trung vào Docker Image, Task Definition, Logs và Metrics thay vì truy cập trực tiếp vào máy chủ.

---

## IV. Triển khai an toàn với CI/CD Pipeline

EduFlow sử dụng quy trình triển khai:

**GitHub → Docker Build → Amazon ECR → Amazon ECS**

Pipeline bao gồm các bước:

- GitHub Actions chạy Maven Test cho frontend và backend, đồng thời thực hiện `terraform validate` trước khi triển khai.
- Build hai Docker Image, gắn tag theo Commit SHA và `latest`, sau đó push lên Amazon ECR.
- Amazon ECS tự động tạo Deployment mới mà không cần đăng nhập vào máy chủ.
- Quy trình triển khai nhất quán giúp dễ dàng theo dõi phiên bản và rollback khi cần. Trong tương lai có thể bổ sung Image Scanning và Policy Checking để tăng cường bảo mật.

{{< figure
src="/fcj-report/images/3-BlogsPosted/blog3-facebook-post-3.png"
alt="Phần cuối bài viết Facebook và sơ đồ triển khai EduFlow trên AWS"
title="Blog 3 trên Facebook - Phần 3">}}

---

## Kết luận

EduFlow kết hợp nhiều lớp bảo vệ khác nhau:

- **Application Load Balancer** và **Security Group** bảo vệ lớp mạng.
- **Container** và **Amazon RDS** được cô lập để giảm bề mặt tấn công.
- **AWS Secrets Manager** quản lý toàn bộ thông tin nhạy cảm.
- **CloudWatch Logs** và **Health Check** giúp phát hiện và xử lý sự cố nhanh chóng.
- **GitHub Actions** và **Amazon ECS** đảm bảo quy trình triển khai lặp lại, nhất quán và an toàn.

Kiến trúc **Defense in Depth** không phụ thuộc vào một dịch vụ duy nhất mà hình thành từ nhiều lớp bảo vệ bổ sung cho nhau, giúp tăng khả năng chống chịu và giảm rủi ro cho toàn bộ hệ thống.

---

## Tài liệu tham khảo

- Amazon ECS – Connect Applications to the Internet
- Amazon ECS – Pass Secrets Manager Secrets Through Environment Variables
- Amazon ECS – LogConfiguration
- Amazon ECS – Services and Unhealthy Task Replacement
