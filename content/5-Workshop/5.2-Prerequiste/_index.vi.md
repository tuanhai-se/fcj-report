---
title: "Điều kiện tiên quyết"
date: 2026-08-05
weight: 2
chapter: false
pre: "<b>5.2.</b>"
description: "Mã nguồn, CI/CD và cấu hình hạ tầng được sử dụng trong workshop."
---

## Tài liệu và công cụ

- Mã nguồn và lịch sử commit của [EduFlowPlatform](https://github.com/L1nkinPark/EduFlowPlatform).
- [GitHub Actions run #76](https://github.com/L1nkinPark/EduFlowPlatform/actions/runs/30985947529) trên nhánh `main`.
- Kiểm tra HTTP trực tiếp tới DNS của Application Load Balancer (ALB) vào ngày **05/08/2026**.
- Cấu hình Terraform trong repository để quản lý kiến trúc AWS.

## Nguyên tắc bảo mật

- Không công khai Account ID, Access Key, Password, JWT, OTP, VNPay Secret hoặc các giá trị bí mật khác.
- Các thông tin nhạy cảm được cung cấp thông qua GitHub Actions Secrets và cấu hình runtime trên AWS.
