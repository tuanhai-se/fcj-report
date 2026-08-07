---
title: "Triển khai EduFlow trên AWS"
date: 2026-08-05
weight: 4
chapter: false
pre: "<b>5.4.</b>"
description: "Địa chỉ ứng dụng và kết quả triển khai trên AWS."
---

Ứng dụng được kiểm tra thông qua DNS mặc định của **AWS Application Load Balancer** tại Region `ap-southeast-1`:

- [Trang chủ EduFlow](http://eduflow-dev-alb-560717424.ap-southeast-1.elb.amazonaws.com/)
- [API thống kê công khai](http://eduflow-dev-alb-560717424.ap-southeast-1.elb.amazonaws.com/api/public/stats)

Cả hai endpoint đều phản hồi HTTP `200` vào ngày **05/08/2026**. Ứng dụng được cung cấp thông qua DNS mặc định do **AWS Application Load Balancer** cấp.

{{% children description="true" /%}}
