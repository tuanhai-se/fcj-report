---
title: "Dọn dẹp"
date: 2026-08-05
weight: 6
chapter: false
pre: "<b>5.6.</b>"
description: "Trạng thái ứng dụng và quy trình triển khai lại EduFlow."
---

# Workshop triển khai EduFlow trên AWS

Phần này trình bày quy trình triển khai EduFlow trên AWS, kết quả CI/CD, kiểm thử trình duyệt và kiểm thử tải được thực hiện vào ngày **05/08/2026**.

## Kết quả triển khai ngày 05/08/2026

| Hạng mục                        | Kết quả                                                                                                                                                                                                               |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Website ứng dụng                | [EduFlow ALB](http://eduflow-dev-alb-560717424.ap-southeast-1.elb.amazonaws.com/) phản hồi HTTP `200`                                                                                                                 |
| API công khai                   | [`/api/public/stats`](http://eduflow-dev-alb-560717424.ap-southeast-1.elb.amazonaws.com/api/public/stats) phản hồi HTTP `200`; dữ liệu trả về gồm **5 khóa học, 2 giảng viên, 6 học viên và 1 lượt đăng ký khóa học** |
| Tên miền triển khai             | `eduflow-dev-alb-560717424.ap-southeast-1.elb.amazonaws.com` — DNS của **AWS Application Load Balancer**                                                                                                              |
| CI/CD                           | [GitHub Actions run #76](https://github.com/L1nkinPark/EduFlowPlatform/actions/runs/30985947529) thực thi thành công                                                                                                  |
| Thời gian pipeline              | `9 phút 7 giây` theo ghi nhận của GitHub Actions                                                                                                                                                                      |
| Kiểm thử smoke trên trình duyệt | Đã kiểm tra trang chủ, trang danh mục và chi tiết khóa học, chuyển đổi giao diện Việt–Anh và chuyển hướng người dùng chưa đăng nhập đến `/signin` khi bắt đầu mua khóa học                                            |
| Kết quả kiểm thử k6             | 50 VUs, 1.758 request, 0 lỗi, p95 `1,84 giây`; [JSON summary](https://github.com/L1nkinPark/EduFlowPlatform/blob/main/static/evidence/k6-summary-2026-08-05.json)                                                     |
| Báo cáo trực tuyến              | [GitHub Pages](https://l1nkinpark.github.io/EduFlowPlatform/) đang hoạt động và được triển khai tự động từ mã nguồn Hugo                                                                                              |
