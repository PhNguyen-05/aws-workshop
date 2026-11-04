---
title: "Worklog Tuần 1"
date: 2026-10-05
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Mục tiêu tuần 1:

* Kết nối, làm quen với các thành viên trong First Cloud AI Journey và nắm rõ nội quy, quy định của công ty.
* Hiểu khái niệm các nhóm dịch vụ AWS cơ bản: Computer, Storage, Networking, Database.
* Tạo AWS Free Tier account, làm quen với AWS Management Console và AWS CLI.
* Hoàn thành 5 nhiệm vụ của chương trình AWS Free Tier (EC2, Amazon Bedrock, AWS Budgets, AWS Lambda, Amazon RDS) để làm quen thực tế với các dịch vụ chính.
* Hình thành thói quen quản lý chi phí và dọn dẹp tài nguyên sau khi thực hành.


### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Làm quen với các thành viên FCAJ <br> - Đọc và lưu ý các nội quy, quy định tại đơn vị thực tập                                                                                             | 05/10/2026   | 05/10/2026      |                                           |
| 3   | - Tìm hiểu AWS và các loại dịch vụ <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database <br>&emsp; + ... <br>                                            | 06/10/2026   | 06/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tạo AWS Free Tier account <br> - Tìm hiểu AWS Console & AWS CLI <br> - **Thực hành:** <br>&emsp; + Tạo AWS account <br>&emsp; + Cài AWS CLI & cấu hình <br> &emsp; + Cách sử dụng AWS CLI | 07/10/2026   | 07/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu EC2: <br>&emsp; + Instance types <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + ... <br> - Các cách remote SSH vào EC2 <br> - Tìm hiểu Elastic IP   <br>                  | 08/10/2026   | 08/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành:** <br>&emsp; + Tạo EC2 instance <br>&emsp; + Kết nối SSH <br>&emsp; + Gắn EBS volume                                                                                         | 09/10/2026   | 09/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 1:

* Hiểu AWS là gì, mô hình trả tiền theo mức sử dụng và nắm được các nhóm dịch vụ cơ bản: 
  * Compute
  * Storage
  * Networking 
  * Database
  * ...

* Hiểu cấu trúc hạ tầng toàn cầu của AWS (Region, Availability Zone, Edge Location) và mô hình trách nhiệm chia sẻ.

* Đã tạo và cấu hình AWS Free Tier account thành công.

* Làm quen với AWS Management Console và biết cách tìm, truy cập, sử dụng dịch vụ từ giao diện web.

* Cài đặt và cấu hình AWS CLI trên máy tính, bao gồm:
  * Access Key
  * Secret Key
  * Region mặc định
  * ...

* Sử dụng AWS CLI để thực hiện các thao tác cơ bản như:
  * Kiểm tra thông tin tài khoản & cấu hình
  * Lấy danh sách region
  * Xem dịch vụ EC2
  * Tạo và quản lý key pair
  * Kiểm tra thông tin dịch vụ đang chạy
  * ...

* Nắm vững kiến thức cơ bản về EC2: instance type, AMI, EBS, key pair, security group, Elastic IP và các cách kết nối SSH.

* Hoàn thành các nhiệm vụ thực hành trong chương trình AWS Free Tier:
  * EC2: tạo instance, kết nối SSH, gắn EBS volume và terminate sau khi dùng xong.
  * Amazon Bedrock: thử nghiệm foundation model (Claude Haiku 5.5) trong Playground.
  * AWS Budgets: thiết lập cost budget và cảnh báo qua email.
  * AWS Lambda: tạo web app từ blueprint HTTP, sau đó xóa function.
  * Amazon RDS: tạo database Aurora (PostgreSQL Compatible) bằng Easy create, sau đó xóa instance và cluster.

* Có khả năng kết nối giữa giao diện web và CLI để quản lý tài nguyên AWS song song.
* ...
