---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

- Hiểu và thực hành các dịch vụ mạng của AWS, đặc biệt là Amazon VPC và các thành phần của nó.
- Thiết lập và cấu hình kết nối mạng trong môi trường Hybrid Cloud: VPN, Direct Connect, Hybrid DNS.
- Triển khai và cấu hình các tính năng mạng nâng cao của AWS như VPC Peering, Transit Gateway, Network ACL và Load Balancer.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                     |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------------------- |
| 2   | - Tìm hiểu về Amazon Virtual Private Cloud (VPC). <br> - Bảo mật VPC và các tính năng Multi-VPC                                                                                                   | 28/05/2026   | 28/05/2026      | <https://youtu.be/O9Ac_vGHquM?si=_eLRx1ohGnWONjq6> |
| 3   | - Thực hành Amazon VPC và kết nối AWS Site-to-Site VPN <br>&emsp; + Tạo VPC <br>&emsp; + Triển khai Amazon EC2 Instance <br>&emsp; + Thiết lập kết nối Site-to-Site VPN trên AWS <br>&emsp; + ... | 29/05/2026   | 29/05/2026      | <https://000003.awsstudygroup.com/>                |
| 4   | - Thiết lập Hybrid DNS với Route 53 Resolver <br> - **Thực hành:** <br>&emsp; + Khởi tạo CloudFormation Template <br>&emsp; + Kết nối đến RDGW <br>&emsp; + Triển khai Microsoft Active Directory | 30/05/2026   | 30/05/2026      | <https://000010.awsstudygroup.com/>                |
| 5   | - Thiết lập VPC Peering <br> - Cấu hình Cross-Peer DNS <br> - Tìm hiểu Network ACL                                                                                                                | 31/05/2026   | 31/05/2026      | <https://000019.awsstudygroup.com/vi/>             |
| 6   | - Thiết lập AWS Transit Gateway <br> - **Thực hành:** <br>&emsp; + Tạo Transit Gateway <br>&emsp; + Tạo Transit Gateway Attachments <br>&emsp; + Cấu hình Transit Gateway Route                   | 01/06/2026   | 01/06/2026      | <https://000020.awsstudygroup.com/>                |

### Kết quả đạt được tuần 2:

- Tìm hiểu Amazon VPC và các thành phần của nó như: Subnet, Route Table, ENI, Elastic IP (EIP), VPC Endpoint và Internet Gateway.

- Hiểu các khái niệm về bảo mật VPC và các tính năng của kiến trúc Multi-VPC.

- Thực hành tạo VPC và triển khai Amazon EC2 Instance bên trong VPC.

- Thiết lập thành công kết nối Site-to-Site VPN giữa hệ thống On-premises và AWS.

- Tạo Transit Gateway Attachments và cấu hình Transit Gateway Route Tables.

- Kết nối thành công đến Remote Desktop Gateway (RDGW).

- Nâng cao kỹ năng thực hành về hệ thống mạng ảo trên AWS.
