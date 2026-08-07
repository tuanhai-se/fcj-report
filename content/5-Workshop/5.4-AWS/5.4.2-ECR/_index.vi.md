---
title: "Build và push image lên Amazon ECR"
date: 2026-08-05
weight: 2
chapter: false
pre: "<b>5.4.2.</b>"
description: "Kết quả build và push container image từ GitHub Actions."
---

Trong [GitHub Actions run #76](https://github.com/L1nkinPark/EduFlowPlatform/actions/runs/30985947529), job **Build, push và deploy** có kết quả `success` với các bước sau:

- Cấu hình thông tin xác thực AWS.
- Đăng nhập vào Amazon ECR.
- Build và push frontend image.
- Build và push backend image.
- Triển khai các dịch vụ Amazon ECS.
- Chờ các dịch vụ Amazon ECS ổn định.
