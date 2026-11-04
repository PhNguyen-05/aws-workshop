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

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Gặp gỡ, làm quen với các thành viên FCAJ <br> - Đọc và ghi nhớ nội quy, quy định tại đơn vị thực tập <br> - Tìm hiểu lộ trình học tập và hình thức báo cáo (worklog hằng tuần) <br> - Chuẩn bị môi trường làm việc: email, công cụ ghi chú, GitHub repo cho báo cáo | 05/10/2026 | 05/10/2026 | |
| 3 | - Tìm hiểu AWS và điện toán đám mây: <br>&emsp; + Khái niệm Cloud Computing, mô hình trả tiền theo mức sử dụng <br>&emsp; + Hạ tầng toàn cầu: Region, Availability Zone, Edge Location <br> - Tìm hiểu các nhóm dịch vụ: <br>&emsp; + Compute (EC2, Lambda) <br>&emsp; + Storage (S3, EBS) <br>&emsp; + Networking (VPC) <br>&emsp; + Database (RDS, DynamoDB) <br> - Tìm hiểu mô hình trách nhiệm chia sẻ (Shared Responsibility Model) | 06/10/2026 | 06/10/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Tạo AWS Free Tier account (đăng ký, xác minh, chọn support plan) <br> - Tìm hiểu AWS Management Console và AWS CLI <br> - **Thực hành:** <br>&emsp; + Tạo AWS account và bảo mật tài khoản <br>&emsp; + Cài đặt & cấu hình AWS CLI (`aws configure`) <br>&emsp; + Dùng AWS CLI để kiểm tra thông tin tài khoản, danh sách region, dịch vụ EC2 <br>&emsp; + **Task 3:** thiết lập cost budget với AWS Budgets, nhận cảnh báo qua email | 07/10/2026 | 07/10/2026 | <https://cloudjourney.awsstudygroup.com/> <br> <https://000001.awsfcaj.com/> |
| 5 | - Tìm hiểu EC2 cơ bản: <br>&emsp; + Instance types (General purpose, Compute/Memory optimized, ...) <br>&emsp; + AMI <br>&emsp; + EBS và các loại volume <br>&emsp; + Key pair, Security group <br> - Các cách remote SSH vào EC2 (SSH client, EC2 Instance Connect, Session Manager) <br> - Tìm hiểu Elastic IP và khác biệt giữa public IP thường với Elastic IP <br> - Đọc phần lưu ý về các dịch vụ dễ làm hao credit | 08/10/2026 | 08/10/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - **Thực hành:** <br>&emsp; + **Task 1 (EC2):** tạo instance `Test Instance`, tạo key pair `first-kp` (RSA, .pem), tạo security group, kết nối SSH, gắn EBS volume, sau đó terminate instance <br>&emsp; + **Task 2 (Bedrock):** gửi use case, chọn Claude 3 Haiku, chạy thử prompt trong Playground <br>&emsp; + **Task 4 (Lambda):** tạo function `http-function-url-tutorial` từ blueprint "Getting started with Lambda HTTP", kiểm tra function URL, sau đó xóa function <br>&emsp; + **Task 5 (RDS):** tạo database Aurora (PostgreSQL Compatible) bằng Easy create, chờ trạng thái Available, sau đó xóa instance và cluster <br> - Rà soát lại toàn bộ tài nguyên để đảm bảo đã dọn dẹp <br> - Kiểm tra trang Credits trong Billing để xác nhận credit nhận được | 09/10/2026 | 09/10/2026 | <https://000001.awsfcaj.com/> |

### Chi tiết các nhiệm vụ thực hành:

#### Task 1: Khởi tạo EC2 instance

* Mở nhiệm vụ "Launch an instance using EC2" trong widget **Explore AWS** trên Console Home, chọn **Start activity**.
* Đặt tên instance, chọn AMI, giữ cấu hình phần cứng mặc định.
* Tạo key pair mới (tên `first-kp`, loại RSA, định dạng `.pem`) và lưu file key cẩn thận để dùng khi SSH.
* Tạo security group mới với các tùy chọn mặc định, rà soát cấu hình rồi chọn **Launch instance**.
* Sau khi hoàn thành nhiệm vụ, **terminate** instance để tránh phát sinh chi phí.

#### Task 2: Amazon Bedrock Playground

* Chọn nhiệm vụ "Use a foundation model in the Amazon Bedrock", chọn model **Claude Haiku 5.5** và điền thông tin use case.
* Nếu gặp lỗi không có quyền truy cập model, tạo support case (Account and billing → Account Activation → Bedrock Allowlisting) kèm Account ID và Region, chờ AWS phản hồi.
* Khi được cấp quyền, nhập prompt thử nghiệm, chạy và xem phản hồi của model.

#### Task 3: AWS Budgets

* Chọn nhiệm vụ "Set up a cost budget using AWS Budgets".
* Nhập email nhận thông báo, tạo budget và kiểm tra budget đã được tạo thành công.
* Đây là bước quan trọng để theo dõi chi phí và tránh vượt hạn mức credit.

#### Task 4: Web app với AWS Lambda

* Chọn nhiệm vụ "Create a web app using AWS Lambda", dùng blueprint **Getting started with Lambda HTTP**.
* Đặt tên function `http-function-url-tutorial`, xác nhận các điều khoản và tạo function.
* Sau khi hoàn thành, xóa function (nhập `confirm` để xác nhận).

#### Task 5: Cơ sở dữ liệu với Amazon RDS

* Chọn nhiệm vụ "Create an Amazon RDS Database", dùng **Easy create** với engine **Aurora (PostgreSQL Compatible)**.
* Chờ database chuyển sang trạng thái **Available**.
* Dọn dẹp: xóa DB instance (`database-1-instance-1`), sau đó xóa DB cluster (`database-1`), bỏ chọn tạo final snapshot và nhập `delete me` để xác nhận.


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
