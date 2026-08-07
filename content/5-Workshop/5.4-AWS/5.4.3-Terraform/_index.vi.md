---
title: "Hạ tầng Terraform"
date: 2026-08-05
weight: 3
chapter: false
pre: "<b>5.4.3.</b>"
description: "Cấu hình hạ tầng và kết quả xác thực Terraform."
---

## Kết quả

- Job **Terraform validation** trong [run #76](https://github.com/L1nkinPark/EduFlowPlatform/actions/runs/30985947529) có kết quả `success`.
- Các bước kiểm tra định dạng, `terraform init -backend=false` và `terraform validate` đều có kết quả `success`.
- Mã nguồn Terraform định nghĩa route mặc định của ALB đến frontend và route `/api/*` đến backend.
- DNS thực tế của ALB trả về HTTP `200` cho trang chủ và API thống kê.
