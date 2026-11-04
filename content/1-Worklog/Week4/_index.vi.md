---
title: "Worklog Tuần 4"
date: 2026-10-26
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


### Mục tiêu tuần 4:

* Hiểu Amazon EC2 (Elastic Compute Cloud) và các tính năng cốt lõi: instance type, AMI, EBS, key pair, security group, Elastic IP.
* Chuẩn bị hạ tầng mạng (VPC, security group) và khởi tạo, kết nối tới EC2 chạy Windows Server 2025 và Amazon Linux 2023.
* Thực hành các thao tác EC2 cơ bản: đổi instance type, snapshot EBS, tạo và chia sẻ custom AMI, khôi phục quyền truy cập khi mất key pair.
* Triển khai ứng dụng Node.js "AWS User Management" (CRUD) trên Linux (LAMP + Node.js) và Windows (XAMPP + Node.js).
* Áp dụng IAM để quản lý chi phí và giới hạn sử dụng EC2 (theo region, instance family, instance type, loại EBS volume, địa chỉ IP, khung giờ).
* Hiểu cách cấp quyền cho ứng dụng truy cập dịch vụ AWS: dùng access key (nên tránh) và dùng IAM Role gắn vào EC2 (nên dùng).
* Dọn dẹp tài nguyên đúng thứ tự để tránh phát sinh chi phí.


### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Tìm hiểu tổng quan Amazon EC2: <br>&emsp; + Khái niệm, các trường hợp sử dụng <br>&emsp; + Instance type, AMI, tùy chọn lưu trữ (EBS, instance store) <br>&emsp; + Bảo mật và truy cập: key pair, security group, NACL, IAM role, Session Manager <br>&emsp; + Tính năng: Auto Scaling, Load Balancing, Elastic IP, Placement Group, nhiều AZ <br> - **Thực hành (Preparation):** <br>&emsp; + Tạo VPC cho Linux instance và VPC cho Windows instance <br>&emsp; + Tạo Security Group cho Linux và cho Windows | 26/10/2026 | 26/10/2026 | <https://000004.awsfcaj.com/> <br> <https://000004.awsfcaj.com/1-introduce/> <br> <https://000004.awsfcaj.com/2-prerequiste/> |
| 3 | - **Thực hành:** <br>&emsp; + Khởi tạo Microsoft Windows Server 2025 instance và kết nối từ máy tính (Remote Desktop) <br>&emsp; + Khởi tạo Amazon Linux instance và kết nối (SSH) <br> - Ghi nhớ các lưu ý: instance đang chạy vẫn tính phí, cần terminate khi không dùng; truy cập từ xa phụ thuộc cấu hình security group | 27/10/2026 | 27/10/2026 | <https://000004.awsfcaj.com/3-launchwindowsinstance/> <br> <https://000004.awsfcaj.com/4-launchlinuxinstance/> |
| 4 | - **Thực hành Amazon EC2 Basic:** <br>&emsp; + Thay đổi instance type <br>&emsp; + Tạo và quản lý EBS snapshot <br>&emsp; + Tạo Custom AMI và khởi chạy instance từ Custom AMI <br>&emsp; + Khôi phục quyền truy cập Windows instance (dùng Systems Manager) và Linux instance (dùng user data) khi mất key pair <br>&emsp; + Remote Desktop vào EC2 Ubuntu <br>&emsp; + (Tùy chọn) EBS Snapshots Archive và Share AMI | 28/10/2026 | 28/10/2026 | <https://000004.awsfcaj.com/5-amazonec2basic/> |
| 5 | - **Triển khai ứng dụng AWS User Management (Node.js, Express, Express-Handlebars, MySQL):** <br>&emsp; + Linux (Amazon Linux 2023): cài LAMP (chuẩn bị, kiểm tra, cấu hình/bảo mật database, cài phpMyAdmin), cài Node.js, triển khai ứng dụng <br>&emsp; + Windows (Windows Server 2025): cài XAMPP, cài Node.js, triển khai ứng dụng <br>&emsp; + Mở inbound port trong security group (3000 cho ứng dụng, 80 cho XAMPP) và kiểm tra các chức năng CRUD | 29/10/2026 | 29/10/2026 | <https://000004.awsfcaj.com/6-awsfcjmanagement-linux/> <br> <https://000004.awsfcaj.com/7-awsfcjmanagement-windows/> |
| 6 | - **Quản lý chi phí và quyền dùng với IAM:** giới hạn dịch vụ theo region; giới hạn EC2 theo instance family và instance type; quản lý loại EBS volume; giới hạn quyền xóa tài nguyên theo IP công ty và theo khung giờ <br> - **Workshop IAM Role cho EC2:** tạo EC2 và S3 bucket; dùng access key của IAM user (không khuyến nghị); dùng IAM role gắn vào EC2 (khuyến nghị) <br> - **Clean up** cả hai workshop <br> - Tổng hợp kiến thức tuần 4 và viết worklog | 30/10/2026 | 30/10/2026 | <https://000004.awsfcaj.com/8-costusagegovernance/> <br> <https://000004.awsfcaj.com/9-cleanup/> <br> <https://000048.awsfcaj.com/> <br> <https://000048.awsfcaj.com/4-cleanup/> |


### Chi tiết nội dung thực hành và tìm hiểu:

#### Lab 4: Giới thiệu Amazon EC2

**1. Tổng quan Amazon EC2**

* Amazon EC2 cung cấp năng lực tính toán theo yêu cầu, co giãn trên AWS Cloud, cho phép chạy máy chủ ảo mà không cần đầu tư phần cứng trước. Dùng cho web hosting, ứng dụng, cơ sở dữ liệu, dịch vụ xác thực và các tác vụ doanh nghiệp.
* Có thể chạy hoặc dừng số lượng máy chủ ảo tùy ý, cấu hình bảo mật và mạng, quản lý lưu trữ, tăng giảm dung lượng.
* **Instance type:** xác định cấu hình phần cứng của máy chủ ảo, bắt buộc chọn khi tạo instance.
  * CPU: Intel, AMD, AWS Graviton (ARM) và GPU; RAM thay đổi theo từng instance family; mạng từ mức trung bình đến 100+ Gbps.
  * Các family: C (tối ưu compute, ví dụ xử lý batch), R (tối ưu bộ nhớ, ví dụ database), T và M (đa dụng).
* **AMI (Amazon Machine Image):** gồm hệ điều hành và cấu hình, application server và ứng dụng, quyền khởi chạy, block device mapping. Có AMI do AWS cung cấp, AMI trên AWS Marketplace và custom AMI tạo từ instance có sẵn.
* **Lưu trữ:**
  * **EBS volume:** lưu trữ bền vững gắn qua mạng, tồn tại độc lập với vòng đời instance.
  * **Instance store:** lưu trữ tạm gắn trực tiếp với máy chủ vật lý, dữ liệu mất khi instance bị stop hoặc terminate.
* **Bảo mật và truy cập:** key pair (SSH), security group (tường lửa ảo), NACL (lọc gói tin stateless ở cấp subnet), IAM role (cấp quyền cho ứng dụng chạy trên EC2), Systems Manager Session Manager (truy cập không cần mở cổng SSH/RDP hay phân phối private key).
* **Tính năng chính:**
  * Hạ tầng co giãn: EC2 Auto Scaling, Elastic Load Balancing, gắn/gỡ tài nguyên động.
  * Mạng: VPC, Elastic IP (IPv4 tĩnh), Placement Group.
  * Sẵn sàng cao: nhiều Availability Zone, Local Zones, AWS Outposts, Wavelength Zones.
  * Quản lý tài nguyên: Tags, Resource Groups, CloudFormation.
  * Giám sát: CloudWatch, CloudTrail, Systems Manager.
* **Lưu ý:** chọn instance family phù hợp với workload để cân bằng hiệu năng và chi phí; dữ liệu cần giữ lâu dài phải lưu trên EBS, không để trên instance store; gắn Elastic IP vào instance đang chạy hoặc release khi không dùng để tránh phát sinh phí.
* Workshop được kiểm thử trên Microsoft Windows Server 2025 và Amazon Linux 2023 (có thêm Ubuntu), dùng ứng dụng Node.js mẫu **AWS User Management** với chức năng CRUD.

**2. Chuẩn bị (Preparation)**

* Trước khi tạo EC2 cần chuẩn bị mạng và kiểm soát bảo mật:
  * **VPC:** mạng cô lập cho instance; tạo riêng cho môi trường Linux (2.1) và Windows (2.2).
  * **Security Group:** tường lửa ảo (stateful) kiểm soát lưu lượng vào/ra; tạo riêng cho Linux (2.3) và Windows (2.4).
* Lưu ý: cấu hình VPC và security group sai có thể chặn lưu lượng hợp lệ hoặc làm lộ instance. Security group chỉ nên cho phép đúng lưu lượng ứng dụng cần và chặn các truy cập khác.

**3. Khởi tạo và kết nối Windows Server 2025 / Amazon Linux**

* **Windows Server 2025:** bản server mới nhất của Microsoft với bảo mật và hiệu năng tốt hơn; AWS cung cấp qua AMI đã gồm license, tích hợp với Systems Manager và Active Directory. Gồm hai phần: khởi tạo instance (3.1) và kết nối từ máy tính (3.2).
* **Amazon Linux:** bản Linux do AWS cung cấp, hướng tới môi trường an toàn, ổn định, hiệu năng cao cho ứng dụng EC2. Gồm khởi tạo instance (4.1) và kết nối (4.2).
* Cảnh báo chi phí: instance đang chạy vẫn tính phí, cần terminate khi không còn dùng. Lưu ý bảo mật: truy cập từ xa (RDP/SSH) phụ thuộc vào cấu hình security group.

**4. Amazon EC2 Basic**

Các thao tác cơ bản với EC2 và các thành phần, gồm thay đổi cấu hình và khôi phục quyền truy cập khi mất key pair:

* **5.1 Modify EC2 Instance Type:** thay đổi instance type của instance.
* **5.2 Create and Manage EBS Snapshots:** tạo và quản lý snapshot của EBS volume.
* **5.3 Create Custom AMI:** tạo AMI tùy chỉnh từ instance.
* **5.4 Launch an Instance from a Custom AMI:** khởi chạy instance mới từ AMI đã tạo.
* **5.5 Recovering Access to Windows Instances:** khôi phục truy cập Windows instance bằng AWS Systems Manager khi mất key pair.
* **5.6 Recovering Access to Linux Instances:** khôi phục truy cập Linux instance bằng user data khi mất key pair.
* **5.7 Remote Desktop to EC2-Ubuntu:** kết nối remote desktop vào EC2 Ubuntu.
* **5.8 Amazon EBS Snapshots Archive (tùy chọn):** lưu trữ lâu dài snapshot (archive).
* **5.9 Share AMI (tùy chọn):** chia sẻ AMI.
* Lưu ý bảo mật: quản lý key pair an toàn và biết các cách truy cập thay thế. Các thao tác này giúp xây dựng kiến trúc có khả năng phục hồi và quản lý tài nguyên compute hiệu quả.

**5. Triển khai ứng dụng AWS User Management trên Amazon Linux 2023**

* Ứng dụng dùng Node.js, Express, Express-Handlebars và MySQL, cung cấp giao diện CRUD để quản lý người dùng.
* Điều kiện: EC2 chạy Amazon Linux 2023, kiến thức cơ bản về AWS và Linux, tài khoản AWS có đủ quyền.
* Các bước:
  1. **Cài LAMP (Linux, Apache, MySQL, PHP):** chuẩn bị LAMP server (6.1.1), kiểm tra LAMP server (6.1.2), cấu hình và bảo mật database server (6.1.3), cài phpMyAdmin (6.1.4).
  2. **Cài Node.js** (6.2).
  3. **Triển khai ứng dụng** (6.3): clone repository, cài dependency bằng npm, cấu hình kết nối database, chạy server và kiểm tra các chức năng CRUD.
* Mẹo: khi cấu hình MariaDB/MySQL, chạy `mysql_secure_installation` để đặt mật khẩu root mạnh và xóa test database. Trong production nên dùng bản LTS mới nhất của Node.js để có các bản vá bảo mật mới nhất.
* Cảnh báo: security group của EC2 phải cho phép inbound ở port của ứng dụng (thường là 3000) để truy cập từ trình duyệt.

**6. Triển khai ứng dụng Node.js trên Amazon EC2 Windows**

* Triển khai cùng ứng dụng AWS User Management trên Windows Server 2025 Base.
* Các bước:
  1. Cài XAMPP (MySQL/MariaDB, Apache, PHP) làm môi trường database (7.1).
  2. Cấu hình schema database qua phpMyAdmin.
  3. Cài Node.js (7.2).
  4. Triển khai và cấu hình ứng dụng (7.3).
  5. Kiểm tra các thao tác CRUD.
* Cảnh báo: security group phải cho phép inbound port 80 (XAMPP) và port 3000 (ứng dụng Node.js).
* Mẹo: XAMPP đơn giản hóa việc cài MySQL, Apache, PHP trên Windows, phù hợp phát triển và kiểm thử; với production nên dùng Amazon RDS for MySQL thay cho database cài local để tăng khả năng mở rộng, độ tin cậy và bảo mật.

**7. Quản lý chi phí và quyền sử dụng với IAM**

* IAM cung cấp kiểm soát truy cập chi tiết, hỗ trợ cả bảo mật lẫn quản lý chi phí. Tính linh hoạt của EC2 giúp dựng nhanh môi trường như XAMPP và LAMP nhưng cũng cần được quản trị.
* Policy IAM theo đặc quyền tối thiểu vừa tăng bảo mật vừa hỗ trợ tối ưu chi phí do ngăn việc tạo tài nguyên trái phép; policy được thiết kế tốt tránh cả over-provisioning (lãng phí) lẫn under-provisioning (giảm hiệu năng).
* Sáu nội dung thực hành:
  * **8.1** Giới hạn sử dụng dịch vụ theo AWS Region.
  * **8.2** Giới hạn EC2 theo instance family.
  * **8.3** Kiểm soát việc tạo EC2 theo instance type.
  * **8.4** Quản lý loại EBS volume được phép dùng.
  * **8.5** Giới hạn quyền xóa tài nguyên theo địa chỉ IP của công ty.
  * **8.6** Giới hạn quyền xóa tài nguyên theo khung thời gian.

**8. Dọn dẹp tài nguyên (workshop EC2)**

Thực hiện theo thứ tự:
1. **Terminate EC2 instance:** EC2 → Instances → chọn instance của lab → Instance state → Terminate instance.
2. **Deregister AMI:** EC2 → AMIs → chọn AMI đã tạo → Actions → Deregister AMI.
3. **Xóa EBS snapshot:** EC2 → Snapshots → chọn snapshot của lab → Actions → Delete snapshot.
4. **Xóa Security Group:** EC2 → Security Groups → Actions → Delete security group (cần chắc chắn group không còn gắn với instance đang chạy).
5. **Xóa Key Pair:** EC2 → Key Pairs → chọn key pair của lab → Actions → Delete.
6. **Xóa VPC:** VPC → Your VPCs → chọn VPC của lab → Actions → Delete VPC (xóa VPC cũng xóa subnet, route table, internet gateway và NAT gateway liên quan).
7. **Xóa tài nguyên IAM:** User Groups, Users, Policies, Roles.

#### Lab 48: Cấp quyền cho ứng dụng truy cập dịch vụ AWS bằng IAM Role

**1. Mục tiêu và kiến trúc**

* Workshop trình bày hai cách cấp quyền cho ứng dụng truy cập dịch vụ AWS:
  1. **Access key (access key ID và secret access key)** của IAM user: cách làm và lý do nên tránh.
  2. **IAM role gắn vào EC2:** cách được khuyến nghị.
* Dịch vụ sử dụng: AWS IAM (user, access key, role), Amazon EC2 (chạy ứng dụng, gắn role) và Amazon S3 (bucket mà ứng dụng truy cập).
* Kịch bản: ứng dụng Python chạy trên EC2 upload đối tượng lên S3 bucket, lần lượt dùng access key rồi dùng IAM role.

**2. Preparation**

* **1.1 Create EC2 Instance:** tạo EC2 instance chạy ứng dụng.
* **1.2 Create S3 bucket:** tạo S3 bucket mà ứng dụng sẽ truy cập.

**3. Dùng access key (không khuyến nghị)**

* **2.1 Generate IAM user and access key:** tạo IAM user và sinh cặp access key, secret access key.
* **2.2 Use access key:** dùng access key trong ứng dụng Python để upload đối tượng lên S3 bucket.
* Access key là credential dài hạn, cần lưu trong ứng dụng/máy chủ nên dễ bị lộ.

**4. Dùng IAM role trên EC2 (khuyến nghị)**

* **3.1 Create IAM role:** tạo IAM role có quyền với dịch vụ cần dùng (role trong bài là `EC2RoleS3Upload`).
* **3.2 Using IAM role:** gắn role vào EC2 instance và xác nhận ứng dụng upload được file lên S3 mà không cần lưu key.

**5. Dọn dẹp (workshop IAM Role)**

* **Xóa S3 bucket** (region us-east-1): chọn bucket `s3-instancerole-xxxx` → Empty → nhập `permanently delete` → Empty; sau đó chọn lại bucket → Delete → nhập tên bucket → Delete bucket.
* **Terminate EC2 instance:** EC2 → Instances → chọn instance của lab → Instance state → Terminate instance → Terminate.
* **Xóa IAM user `IAMAccessKey`:** IAM → Users → chọn user → Delete → chọn tắt (deactivate) access key → nhập `confirm` → Delete user.
* **Xóa IAM role `EC2RoleS3Upload`:** IAM → Roles → tìm `ec2` → chọn role → Delete → nhập tên role → Delete.


### Kết quả đạt được tuần 4:

* Hiểu Amazon EC2 và các thành phần chính:
  * Instance type và các family (C, R, T, M)
  * AMI
  * EBS volume và instance store
  * Key pair, security group, NACL
  * Elastic IP và các tính năng sẵn sàng cao (nhiều AZ, Auto Scaling, Load Balancing)

* Nắm quy trình chuẩn bị VPC và security group riêng cho môi trường Linux và Windows trước khi tạo instance.

* Biết cách khởi tạo và kết nối tới Windows Server 2025 và Amazon Linux 2023.

* Nắm các thao tác EC2 cơ bản: đổi instance type, snapshot EBS, Custom AMI, khởi chạy từ AMI, chia sẻ AMI, EBS Snapshots Archive và các cách khôi phục truy cập khi mất key pair.

* Hiểu cấu trúc triển khai ứng dụng Node.js (Express, Express-Handlebars, MySQL) trên Linux (LAMP + Node.js) và Windows (XAMPP + Node.js), cùng các port cần mở trong security group (3000 và 80).

* Hiểu vì sao production nên dùng Amazon RDS for MySQL thay cho database cài local, và nên dùng bản LTS của Node.js.

* Hiểu cách dùng IAM để quản lý chi phí: giới hạn theo region, instance family, instance type, loại EBS volume, địa chỉ IP và khung giờ.

* Phân biệt hai cách cấp quyền cho ứng dụng truy cập dịch vụ AWS, hiểu vì sao nên dùng IAM Role gắn vào EC2 thay cho access key dài hạn.

* Biết thứ tự dọn dẹp tài nguyên EC2 (instance → AMI → snapshot → security group → key pair → VPC → IAM) và dọn dẹp tài nguyên của workshop IAM Role (S3 bucket, EC2, IAM user, IAM role).
* ...
