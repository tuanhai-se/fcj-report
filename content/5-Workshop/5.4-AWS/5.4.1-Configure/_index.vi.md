---
title: "Cấu hình triển khai"
date: 2026-08-05
weight: 1
chapter: false
pre: "<b>5.4.1.</b>"
description: "Các giá trị cấu hình được sử dụng cho môi trường EduFlow trên AWS."
---

| Thuộc tính                      | Giá trị                                                      |
| ------------------------------- | ------------------------------------------------------------ |
| AWS Region                      | `ap-southeast-1`                                             |
| Dự án/môi trường Terraform      | `eduflow` / `dev`                                            |
| Giao thức công khai đã kiểm tra | HTTP                                                         |
| Endpoint công khai              | `eduflow-dev-alb-560717424.ap-southeast-1.elb.amazonaws.com` |

Thông tin xác thực SMTP, thông tin xác thực VNPay, JWT secret và database secret được quản lý thông qua các secret của môi trường triển khai và không được hiển thị trong báo cáo.

Việc xác thực cấu hình Terraform của repository đã hoàn thành thành công trong [run #76](https://github.com/L1nkinPark/EduFlowPlatform/actions/runs/30985947529).
