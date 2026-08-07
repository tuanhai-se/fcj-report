---
title: "Đề xuất"
date: 2026-08-05
weight: 2
chapter: false
pre: "<b>2.</b>"
---

# Nền tảng học trực tuyến EduFlow

<h2 class="proposal-subtitle">Giải pháp quản lý học tập Full-Stack triển khai trên AWS</h2>

### 1. Tóm tắt

EduFlow là nền tảng học trực tuyến quản lý toàn bộ vòng đời của khóa học dành cho **học viên**, **giảng viên** và **quản trị viên**. Hệ thống hỗ trợ tạo khóa học, khám phá danh mục, thanh toán qua VNPay, học trực tuyến và theo dõi tiến độ học tập. Hai ứng dụng Spring Boot được đóng gói thành container, triển khai trên Amazon ECS Fargate và được tự động cung cấp hạ tầng bằng Terraform.

---

### 2. Bài toán

#### Vấn đề

Nội dung khóa học, đơn hàng, tiến độ học tập và hoạt động của giảng viên thường được quản lý trên nhiều hệ thống khác nhau. Việc sử dụng dữ liệu giả hoặc các trang tĩnh làm trải nghiệm người dùng thiếu nhất quán, trong khi triển khai thủ công dễ gây sai lệch cấu hình và kéo dài thời gian khôi phục hệ thống. Ngoài ra, quyền truy cập giữa các vai trò cũng cần được đồng bộ trên cả giao diện web và REST API.

#### Giải pháp

EduFlow tập trung toàn bộ vòng đời khóa học trên một nền tảng duy nhất. Giao diện Thymeleaf chịu trách nhiệm hiển thị theo từng vai trò và quản lý phiên đăng nhập của trình duyệt, trong khi Spring Boot REST Backend xử lý nghiệp vụ, truy cập dữ liệu, xác thực JWT, OTP và thanh toán. Dữ liệu được lưu trên MySQL, còn AWS cung cấp khả năng định tuyến, tính toán, lưu trữ, quản lý bí mật, ghi log và triển khai lặp lại.

#### Lợi ích và giá trị mang lại

- Một quy trình thống nhất từ tạo khóa học, mua khóa học đến học tập và theo dõi tiến độ.
- Dashboard theo từng vai trò giúp giảm thao tác quản trị thủ công.
- Terraform và GitHub Actions giúp hạ tầng và quá trình triển khai có thể tái sử dụng.
- Kiểm thử tự động, health check và k6 giúp giảm rủi ro khi triển khai.
- Báo cáo Hugo song ngữ giúp chia sẻ kết quả dự án và phục vụ đánh giá kỹ thuật.

---

### 3. Kiến trúc giải pháp

EduFlow tách riêng tầng giao diện và tầng nghiệp vụ để frontend và backend có thể phát triển, kiểm thử và triển khai độc lập. Toàn bộ lưu lượng truy cập được tiếp nhận thông qua một Application Load Balancer.

#### Kiến trúc ứng dụng

{{< mermaid >}}
graph LR
USERS[Học viên, Giảng viên, Quản trị viên] --> ALB[Application Load Balancer]
ALB -->|Default route| FE[Frontend Spring Boot trên ECS 8080]
ALB -->|API route| BE[Backend Spring Boot REST trên ECS 8888]
FE -->|JWT và REST| BE
BE --> RDS[Amazon RDS MySQL 8]
FE --> MEDIA[Cloudinary và Media]
BE --> SMTP[SMTP và OTP]
BE --> VNPAY[VNPay Sandbox]
{{< /mermaid >}}

#### Các dịch vụ AWS sử dụng

- **Amazon VPC:** tạo ranh giới mạng và tổ chức các subnet public/private.
- **Application Load Balancer:** định tuyến truy cập tới frontend và `/api/*` tới backend.
- **Amazon ECS Fargate:** chạy hai container frontend và backend.
- **Amazon ECR:** lưu trữ image của hai ứng dụng.
- **Amazon RDS for MySQL:** lưu dữ liệu người dùng, khóa học, bài học, đơn hàng và tiến độ.
- **Amazon S3:** lưu trữ đối tượng theo hạ tầng Terraform.
- **AWS Secrets Manager:** quản lý thông tin nhạy cảm khi chạy ứng dụng.
- **Amazon CloudWatch Logs:** thu thập log của container.

#### Thiết kế thành phần

| Lớp         | Thành phần                            | Trách nhiệm                                                          |
| ----------- | ------------------------------------- | -------------------------------------------------------------------- |
| Web         | Spring Boot, Thymeleaf, i18n          | Trang công khai, dashboard theo vai trò, biểu mẫu và phiên đăng nhập |
| API         | Spring Boot REST, Security, JWT       | Quản lý tài khoản, khóa học, bài học, đơn hàng, OTP và tiến độ       |
| Dữ liệu     | MySQL 8, Spring Data JPA              | Người dùng, danh mục, nội dung, đơn hàng và tiến độ học tập          |
| Tích hợp    | VNPay, SMTP, Cloudinary/media storage | Thanh toán, OTP/email và nội dung đa phương tiện                     |
| Hạ tầng     | Docker, ALB, ECS, ECR, RDS, S3        | Đóng gói, định tuyến, tính toán, cơ sở dữ liệu và lưu trữ            |
| Tự động hóa | Terraform, GitHub Actions             | Kiểm tra hạ tầng, kiểm thử và triển khai                             |

#### Kiến trúc triển khai

![Deployment Architecture](/fcj-report/images/eduflow-deployment-architecture.png)

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn thực hiện

1. Thiết kế mô hình dữ liệu, REST API và ba vai trò người dùng.
2. Xây dựng chức năng quản lý khóa học, danh mục, đơn hàng, bài học và tiến độ.
3. Tích hợp JWT, OTP, VNPay, lưu trữ media và hỗ trợ song ngữ.
4. Bổ sung kiểm thử frontend/backend, browser smoke test, k6 và tăng cường bảo mật.
5. Đóng gói Docker, triển khai AWS bằng Terraform và tự động hóa với GitHub Actions.

#### Yêu cầu kỹ thuật

- **Runtime:** Java 17 và Spring Boot 3.3.4.
- **Frontend:** Spring Boot, Thymeleaf, JavaScript và i18n.
- **Backend:** Spring REST, Spring Security, JWT, JPA, OTP và VNPay.
- **Cơ sở dữ liệu:** MySQL 8 với Spring Data JPA.
- **Cloud:** AWS Region `ap-southeast-1`, ECS Fargate, ALB, ECR, RDS, S3, Secrets Manager và CloudWatch.
- **Tự động hóa:** Maven, Docker, Terraform, GitHub Actions, Browser Smoke Test và k6.

---

### 5. Lộ trình và các mốc thực hiện

- **19/05 – 08/06/2026:** Học DevOps, Kubernetes, Amazon EC2 và IAM.
- **09/06 – 22/06/2026:** Thực hành Full-stack, CI/CD và kiến trúc thời gian thực qua KET-Vault và Tardis.
- **23/06 – 13/07/2026:** Xây dựng nền tảng EduFlow trên AWS, Terraform, Docker và Aegis Security.
- **14/07 – 03/08/2026:** Kiểm thử, VNPay, i18n, k6, dashboard và tích hợp dữ liệu.
- **04/08 – 05/08/2026:** Kiểm tra triển khai AWS, browser testing, load testing và hoàn thiện báo cáo Hugo.

---

### 6. Ước tính chi phí

Môi trường phát triển được thiết kế phù hợp với quy mô thực tập và tối ưu chi phí:

- Một frontend task và một backend task, mỗi task sử dụng **256 CPU units** và **512 MiB RAM**.
- Amazon RDS MySQL sử dụng **db.t4g.micro**, **20 GB** dung lượng ban đầu và triển khai Single-AZ.
- Hai ứng dụng sử dụng chung quy trình build và deploy image lên Amazon ECR.
- Log, secrets và object storage được quản lý bởi các dịch vụ AWS riêng biệt.
- Các biến Terraform cho phép mở rộng số lượng task, dung lượng cơ sở dữ liệu và cấu hình HA khi cần.

---

### 7. Đánh giá rủi ro

| Rủi ro                                | Ảnh hưởng                          | Biện pháp giảm thiểu                            |
| ------------------------------------- | ---------------------------------- | ----------------------------------------------- |
| Sai cấu hình JWT                      | Xác thực thất bại giữa các dịch vụ | Sử dụng chung secret và kiểm thử luồng xác thực |
| Sai callback hoặc chữ ký VNPay        | Không xác nhận được đơn hàng       | Chuẩn hóa amount, encoding và kiểm tra callback |
| Không có quyền upload trong container | Giảng viên không thể tải nội dung  | Tạo thư mục ghi được và kiểm tra trong CI       |
| Backend khởi động chậm                | Frontend timeout                   | Thiết lập timeout và health check của ALB       |
| AWS mở rộng tài nguyên                | Tăng chi phí vận hành              | Sử dụng cấu hình nhỏ cho môi trường phát triển  |

---

### 8. Kết quả mong đợi

#### Kết quả kỹ thuật

- Ba giao diện theo từng vai trò với dữ liệu thực.
- Quy trình hoàn chỉnh từ tạo khóa học, mua khóa học đến học tập và theo dõi tiến độ.
- Hai dịch vụ Spring Boot được kiểm thử và triển khai độc lập sau một ALB.
- Hạ tầng và triển khai có thể tái tạo bằng Terraform và GitHub Actions.
- Kết quả browser testing, HTTP testing, CI/CD và k6 được ghi nhận trong Workshop.

#### Giá trị dự án

EduFlow cung cấp nền tảng có thể mở rộng cho thương mại khóa học, phân tích học tập, phân phối nội dung và vận hành trên nền tảng đám mây. Dự án cũng thể hiện việc vận dụng thực tế các kiến thức về Software Engineering, Security, DevOps và AWS trong quá trình thực tập.
