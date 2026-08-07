---
title: "Kết quả kiểm thử"
date: 2026-08-05
weight: 5
chapter: false
pre: "<b>5.5.</b>"
description: "Kết quả kiểm thử HTTP, trình duyệt, tải và CI/CD của EduFlow."
---

## Kiểm tra HTTP ngày 05/08/2026

| Endpoint                                                                                           | Kết quả                                | Một lần đo thực tế |
| -------------------------------------------------------------------------------------------------- | -------------------------------------- | ------------------ |
| [Trang chủ](http://eduflow-dev-alb-560717424.ap-southeast-1.elb.amazonaws.com/)                    | HTTP `200`, `text/html; charset=UTF-8` | khoảng `498 ms`    |
| [API thống kê](http://eduflow-dev-alb-560717424.ap-southeast-1.elb.amazonaws.com/api/public/stats) | HTTP `200`, `application/json`         | khoảng `137 ms`    |

Kết quả kiểm tra HTTP được ghi nhận vào ngày **05/08/2026**.

Dữ liệu trả về từ API thống kê cho biết hệ thống có **5 khóa học, 2 giảng viên, 6 học viên và 1 lượt đăng ký khóa học**. Tệp JSON được lưu tại:
[Stored JSON](https://github.com/L1nkinPark/EduFlowPlatform/blob/main/static/evidence/public-stats-2026-08-05.json).

## Kiểm thử smoke trên trình duyệt

- Trang chủ tiếng Việt hiển thị đầy đủ thanh điều hướng, ô tìm kiếm, các khóa học nổi bật và giá khóa học.
- Chức năng chuyển đổi VI/EN cập nhật chính xác các nhãn giao diện giữa tiếng Việt và tiếng Anh.
- Trang danh mục khóa học và trang chi tiết khóa học hiển thị thành công.
- Người dùng chưa đăng nhập khi chọn **Buy Now** được chuyển hướng đến `/signin`.

Ảnh minh chứng:

- [Ảnh trang chủ](https://github.com/L1nkinPark/EduFlowPlatform/blob/main/static/evidence/eduflow-home-2026-08-05.png)
- [Ảnh trang chi tiết khóa học](https://github.com/L1nkinPark/EduFlowPlatform/blob/main/static/evidence/eduflow-course-detail-2026-08-05.png)

## Kiểm thử tải bằng k6

Kịch bản chỉ đọc dữ liệu được tăng dần lên **50 Virtual Users (VUs)**, duy trì 50 VUs trong **30 giây**, sau đó giảm dần. Kịch bản gửi yêu cầu đến các endpoint `/api/courses`, `/api/categories` và `/api/public/stats`.

| Chỉ số                | Kết quả                       |
| --------------------- | ----------------------------- |
| Số lượng request HTTP | 1.758                         |
| Kiểm tra (Checks)     | 2.930/2.930 thành công (100%) |
| Tỷ lệ lỗi HTTP        | 0,00%                         |
| Trung bình / Trung vị | 665,06 ms / 498,24 ms         |
| p90 / p95             | 1,49 s / 1,84 s               |
| Lớn nhất              | 3,13 s                        |

Tất cả các ngưỡng kiểm thử đều đạt yêu cầu. Báo cáo tổng hợp được lưu tại:

- [JSON summary](https://github.com/L1nkinPark/EduFlowPlatform/blob/main/static/evidence/k6-summary-2026-08-05.json)
- [Verification record](https://github.com/L1nkinPark/EduFlowPlatform/blob/main/static/evidence/verification-2026-08-05.md)

## Kết quả kiểm tra CI

- Backend tests: `success`.
- Frontend tests và kiểm tra quyền ghi thư mục upload khi chạy ứng dụng: `success`.
- Terraform format, `init` và `validate`: `success`.
- Build/push container image và triển khai lên Amazon ECS: `success`.

Nguồn: [GitHub Actions run #76](https://github.com/L1nkinPark/EduFlowPlatform/actions/runs/30985947529), tổng thời gian thực thi **9 phút 7 giây**.
