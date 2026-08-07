---
title: "Blog 1"
date: 2026-08-05
weight: 1
chapter: false
pre: "<b>3.1.</b>"
description: "Tổng quan về Amazon RDS, Multi-AZ, sao lưu, khôi phục và bốn cách tối ưu chi phí."
---

# Amazon RDS — Khả năng sẵn sàng cao, Sao lưu và Tối ưu chi phí cơ sở dữ liệu trên AWS

Tiếp tục chuỗi bài viết về các dịch vụ cốt lõi của AWS, bài viết này tập trung vào **Amazon RDS (Relational Database Service)** với ba nội dung thường gây nhầm lẫn cho người mới bắt đầu: **Multi-AZ**, **Backup & Restore**, và **tối ưu chi phí**.

{{< figure
src="/fcj-report/images/3-BlogsPosted/blog1-amazon-rds.png"
alt="Amazon RDS"
title="Tổng quan kiến thức về Amazon RDS">}}

## 1. Amazon RDS là gì?

Amazon RDS là dịch vụ cơ sở dữ liệu quan hệ được AWS quản lý hoàn toàn, hỗ trợ nhiều hệ quản trị như **MySQL, PostgreSQL, MariaDB, Oracle, SQL Server** và **Amazon Aurora**.

AWS chịu trách nhiệm cài đặt, cập nhật hệ điều hành và database engine, sao lưu dữ liệu cũng như xử lý failover. Nhờ đó, nhóm phát triển có thể tập trung vào thiết kế cơ sở dữ liệu, truy vấn và phát triển ứng dụng thay vì quản trị hạ tầng.

---

## 2. Multi-AZ — Đừng nhầm với Read Replica

Điểm khác biệt quan trọng nhất là **Multi-AZ được thiết kế nhằm tăng tính sẵn sàng (High Availability), không phải để mở rộng khả năng đọc dữ liệu**.

Amazon RDS hiện hỗ trợ hai mô hình triển khai Multi-AZ:

- **Multi-AZ DB Instance Deployment:** Một bản sao dự phòng (Standby) được tạo ở Availability Zone khác và đồng bộ dữ liệu theo thời gian thực. Standby không phục vụ truy vấn đọc mà chỉ được kích hoạt khi xảy ra sự cố.
- **Multi-AZ DB Cluster Deployment:** Bao gồm một Writer và hai Reader phân bố trên ba Availability Zone. Hai Reader có thể xử lý truy vấn đọc, giúp tăng khả năng chịu lỗi và mở rộng đọc. Thời gian failover thường dưới 35 giây tùy theo workload.

Khi xảy ra lỗi phần cứng, mạng hoặc Availability Zone, Amazon RDS sẽ tự động chuyển sang Standby hoặc Reader phù hợp mà không cần thao tác thủ công.

> Nếu mục tiêu chính là tăng khả năng xử lý các câu lệnh **SELECT**, hãy sử dụng **Read Replica**. Multi-AZ và Read Replica phục vụ hai mục đích hoàn toàn khác nhau: **High Availability** và **Read Scaling**.

---

## 3. Backup và Restore — Hiểu trước khi triển khai Production

Một số điểm quan trọng cần lưu ý:

- Khi tạo DB bằng AWS Management Console, **Automated Backup** được bật mặc định.
- Amazon RDS tạo snapshot toàn bộ ổ đĩa mỗi ngày trong khoảng thời gian backup window.
- Transaction Log được gửi lên Amazon S3 khoảng mỗi **5 phút**, cho phép khôi phục dữ liệu theo thời điểm (**Point-in-Time Recovery - PITR**).
- Thời gian lưu Automated Backup có thể cấu hình tối đa **35 ngày**.
- **Manual Snapshot** sẽ không tự động bị xóa theo retention policy mà chỉ mất khi người dùng chủ động xóa.
- Khi Restore từ Backup hoặc Snapshot, Amazon RDS sẽ tạo **một DB Instance mới**, không ghi đè lên cơ sở dữ liệu hiện tại.

---

## 4. Bốn cách tối ưu chi phí Amazon RDS

### 1. Sử dụng Reserved Instances

Đối với workload chạy liên tục 24/7, Reserved Instance có thể giúp tiết kiệm tới **69%** so với On-Demand tùy Region, loại Database Engine và thời hạn cam kết.

Lưu ý rằng Reserved Instance chỉ là ưu đãi về **chi phí**, không làm thay đổi cách cơ sở dữ liệu hoạt động.

### 2. Right-size trước khi mua Reserved Instance

Trước khi cam kết sử dụng RI, nên theo dõi CloudWatch trong ít nhất một chu kỳ hoạt động để đánh giá:

- CPUUtilization
- FreeableMemory
- Database Connections
- IOPS
- Latency

Nếu mua Reserved Instance cho một máy chủ đang over-provision sẽ gây lãng phí tài nguyên trong thời gian dài.

### 3. Dùng Single-AZ cho môi trường Dev/Test

Đối với môi trường phát triển hoặc kiểm thử không yêu cầu khả năng failover tự động, **Single-AZ** thường là lựa chọn tiết kiệm hơn.

Môi trường Production vẫn nên lựa chọn dựa trên yêu cầu về RTO, RPO và mức độ sẵn sàng của hệ thống.

### 4. Tự động dừng RDS ngoài giờ làm việc

Có thể sử dụng **AWS Lambda** kết hợp **Amazon EventBridge** để tự động dừng và khởi động các cơ sở dữ liệu Dev/Test.

Một DB Instance chỉ có thể dừng tối đa **7 ngày**, sau đó Amazon RDS sẽ tự khởi động lại. Trong thời gian dừng, chi phí lưu trữ và backup vẫn tiếp tục được tính.

---

## 5. Một số lưu ý khi vận hành

- Amazon RDS không cho phép truy cập trực tiếp vào hệ điều hành hoặc filesystem của máy chủ.
- Automated Backup chỉ hoạt động khi DB ở trạng thái **available**.
- Khi xóa DB Instance, có thể chọn **Retain automated backups** để giữ lại bản sao lưu tự động trong thời gian retention còn hiệu lực.
- Manual Snapshot và Final Snapshot được quản lý độc lập với Automated Backup.

---

## Tài liệu tham khảo

- Amazon RDS User Guide — Multi-AZ Deployments
- Amazon RDS User Guide — Automated Backups
- Amazon RDS User Guide — Managing Automated Backups
- Amazon RDS User Guide — Stopping a DB Instance
- Amazon RDS Reserved Instances

---

## Bài viết đã đăng

{{% button href="https://www.facebook.com/groups/awsstudygroupfcj/permalink/2235031937261766/" icon="fab fa-facebook" %}}
Xem Blog 1 trên AWS Study Group VN
{{% /button %}}
