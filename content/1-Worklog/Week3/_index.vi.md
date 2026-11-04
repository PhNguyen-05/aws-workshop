---
title: "Worklog Tuần 3"
date: 2026-10-19
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu tuần 3:

* Hiểu vai trò của AWS IAM (Identity and Access Management) trong việc kiểm soát truy cập và nắm các thành phần chính: User, Group, Policy, Role.
* Thực hành tạo IAM Group, IAM User, IAM Role và chuyển đổi quyền (Switch Role) theo nguyên tắc đặc quyền tối thiểu (least privilege).
* Hiểu Amazon VPC: CIDR, subnet, route table, Internet Gateway, NAT Gateway.
* Phân biệt Security Group và Network ACL, hiểu VPC Flow Logs và Reachability Analyzer.
* Nắm kiến trúc triển khai VPC nhiều AZ cùng EC2, NAT Gateway, Session Manager và giám sát bằng CloudWatch.
* Hiểu mô hình AWS Site-to-Site VPN (Virtual Private Gateway, Customer Gateway, tunnel) và biết cách dọn dẹp tài nguyên đúng thứ tự để tránh phát sinh chi phí.


### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Tìm hiểu tổng quan AWS IAM: <br>&emsp; + Root user và lý do không dùng cho công việc hằng ngày <br>&emsp; + Tính năng: chia sẻ quyền truy cập, kiểm soát chi tiết, truy cập an toàn cho ứng dụng <br>&emsp; + Tính năng bảo mật: MFA, identity federation, tích hợp CloudTrail <br>&emsp; + Các cách truy cập: Console, CLI, SDK/API <br> - Tìm hiểu 4 thành phần: IAM User, IAM Group, IAM Policy, IAM Role <br> - Lưu ý IAM và AWS STS miễn phí | 19/10/2026 | 19/10/2026 | <https://000002.awsfcaj.com/> <br> <https://000002.awsfcaj.com/1-introduction/> |
| 3 | - **Thực hành:** <br>&emsp; + Tạo IAM Group có quyền quản trị <br>&emsp; + Tạo IAM User và thêm vào group quản trị <br>&emsp; + Thiết lập thông tin đăng nhập an toàn, đăng nhập bằng Admin User để kiểm tra quyền <br> - Ghi nhớ nguyên tắc: quản lý quyền qua group, không gắn policy trực tiếp cho từng user | 20/10/2026 | 20/10/2026 | <https://000002.awsfcaj.com/2-create-admin-user-and-group/> |
| 4 | - **Thực hành:** <br>&emsp; + Tạo IAM Role `Admin` với managed policy `AdministratorAccess` <br>&emsp; + Tạo IAM User `OperatorUser` <br>&emsp; + Cấp quyền cho `OperatorUser` được assume role <br>&emsp; + Đăng nhập Console bằng `OperatorUser` và thực hiện Switch Role <br>&emsp; + Chuyển về role ban đầu sau khi làm xong <br>&emsp; + **Clean up:** xóa user, xóa group, kiểm tra IAM Dashboard | 21/10/2026 | 21/10/2026 | <https://000002.awsfcaj.com/3-aws-role/> <br> <https://000002.awsfcaj.com/4-switch-roles/> <br> <https://000002.awsfcaj.com/5-cleanup/> |
| 5 | - Tìm hiểu Amazon VPC: <br>&emsp; + Khái niệm VPC, CIDR `/16` đến `/28`, secondary CIDR, IPv6, IPAM <br>&emsp; + Subnet (public, private, isolated), Route Table, Internet Gateway, NAT Gateway <br> - Tìm hiểu tường lửa trong VPC: <br>&emsp; + Security Group (stateful, cấp instance/ENI) <br>&emsp; + Network ACL (stateless, cấp subnet, đánh giá rule theo số thứ tự) <br> - Tìm hiểu VPC Flow Logs và VPC Reachability Analyzer | 22/10/2026 | 22/10/2026 | <https://000003.awsfcaj.com/> <br> <https://000003.awsfcaj.com/1-introduce/> <br> <https://000003.awsfcaj.com/2-firewallinvpc/> |
| 6 | - Tìm hiểu quy trình xây dựng VPC nhiều AZ: tạo VPC, subnet, Internet Gateway, route table, security group, bật VPC Flow Logs <br> - Tìm hiểu triển khai EC2 và các thành phần liên quan: NAT Gateway, Reachability Analyzer, EC2 Instance Connect Endpoint, Systems Manager Session Manager, giám sát CloudWatch <br> - Tìm hiểu AWS Site-to-Site VPN: Virtual Private Gateway, Customer Gateway, BGP/static routing, 2 tunnel IPsec <br> - Tìm hiểu thứ tự dọn dẹp tài nguyên và các mẫu Infrastructure as Code (CloudFormation, CDK, Terraform) <br> - Tổng hợp kiến thức tuần 3 và viết worklog | 23/10/2026 | 23/10/2026 | <https://000003.awsfcaj.com/3-prerequisite/> <br> <https://000003.awsfcaj.com/4-createec2server/> <br> <https://000003.awsfcaj.com/5-vpnsitetosite/> <br> <https://000003.awsfcaj.com/6-cleanup/> <br> <https://000003.awsfcaj.com/7-infrastructureascode/> |


### Chi tiết nội dung thực hành và tìm hiểu:

#### Lab 2: Quản lý quyền truy cập với AWS IAM

**1. Tổng quan về IAM**

* IAM là dịch vụ bảo mật giúp quản lý tập trung quyền truy cập vào các dịch vụ và tài nguyên AWS: tạo danh tính người dùng, phân quyền chi tiết và xác thực an toàn.
* **Root user** có toàn quyền với mọi thứ trong tài khoản: cần bảo vệ bằng mật khẩu mạnh và MFA, không dùng cho công việc hằng ngày mà tạo IAM user với quyền phù hợp; chỉ dùng root cho một số tác vụ đặc thù.
* Ba tính năng chính:
  * Chia sẻ quyền truy cập tài khoản mà không chia sẻ thông tin đăng nhập.
  * Kiểm soát truy cập chi tiết theo từng người dùng, từng tài nguyên, theo nguyên tắc đặc quyền tối thiểu và custom policy.
  * Truy cập an toàn cho ứng dụng: quản lý credential cho ứng dụng chạy trên EC2 và dùng IAM role cho giao tiếp giữa các dịch vụ.
* Tính năng bảo mật: MFA, identity federation với nhà cung cấp bên ngoài, tích hợp CloudTrail để kiểm toán, hỗ trợ tuân thủ PCI DSS, mô hình eventual consistency.
* Ba cách truy cập: AWS Management Console, công cụ dòng lệnh (AWS CLI, PowerShell), truy cập lập trình (AWS SDK, IAM Query API qua HTTPS). AWS CloudShell cho phép dùng dòng lệnh ngay trên console mà không cần cài đặt.
* Chi phí: IAM và AWS STS miễn phí, chỉ trả phí cho các dịch vụ AWS khác.
* Bốn thành phần cốt lõi:
  * **IAM User:** danh tính riêng lẻ với credential dài hạn.
  * **IAM Group:** tập hợp các user.
  * **IAM Policy:** định nghĩa quyền dưới dạng JSON.
  * **IAM Role:** credential tạm thời để truy cập giữa các dịch vụ hoặc giữa các thực thể được tin cậy.

**2. Tạo IAM Group và IAM User**

* Tạo IAM Group và cấu hình quyền quản trị.
* Tạo IAM User mới và thêm vào group quản trị.
* Thiết lập thông tin đăng nhập an toàn, đăng nhập vào tài khoản AWS bằng Admin User để xác minh quyền quản trị.
* Lưu ý: ghi chép và lưu trữ an toàn thông tin đăng nhập vì các bài sau còn dùng đến; nên quản lý quyền qua group thay vì gắn policy trực tiếp cho từng user.

**3. Tạo IAM Role và IAM User**

* Role cấp credential tạm thời thay cho access key dài hạn và ủy quyền cho các thực thể được tin cậy (IAM user trong tài khoản hoặc dịch vụ AWS). Dùng role thay vì gắn policy trực tiếp giúp kiểm soát tốt hơn và quản lý đơn giản hơn.
* Mục tiêu:
  * Tạo IAM Role tên **Admin** với managed policy **AdministratorAccess**.
  * Tạo IAM User tên **OperatorUser**.
  * Cấu hình quyền cho OperatorUser được assume role.
* Cảnh báo: `AdministratorAccess` cấp toàn quyền, cần tuân thủ chính sách bảo mật của tổ chức khi gán quyền quản trị.

**4. Switch Role**

* Switch role là việc tạm thời đảm nhận (assume) một role đã tạo, kể cả giữa các tài khoản AWS, dùng credential tạm thời thay cho khóa dài hạn, phù hợp với nguyên tắc đặc quyền tối thiểu.
* Các bước: (1) cấu hình quyền switch role cho OperatorUser, (2) truy cập AWS Console bằng OperatorUser, (3) thực hiện switch role.
* Điều kiện tiên quyết: đã hoàn thành phần tạo admin group/user và phần tạo Admin role, OperatorUser.
* Mẹo: sau khi làm xong trong role đã assume, nên chuyển về role ban đầu để audit trail rõ ràng và tránh truy cập tài nguyên ngoài ý muốn.

**5. Dọn dẹp tài nguyên**

* Mở IAM Console, chọn **Users**, chọn user của lab, chọn **Delete** và xác nhận trong hộp thoại.
* Chọn **User groups**, chọn group của lab, chọn **Delete** và xác nhận.
* Kiểm tra IAM Dashboard để chắc chắn đã xóa hết tài nguyên của lab.

**Mẹo bảo mật chung:** dùng IAM Role cho quyền tạm thời thay vì gán trực tiếp, đặt thời gian hết hạn session, theo dõi hoạt động truy cập qua AWS CloudTrail, và thường xuyên rà soát quyền theo nguyên tắc đặc quyền tối thiểu.

#### Lab 3: Amazon VPC và AWS Site-to-Site VPN

**1. Giới thiệu Amazon VPC**

* VPC là mạng ảo được cô lập về mặt logic trong AWS, nơi chạy EC2, ECS/EKS, RDS... và tự kiểm soát cấu hình mạng; được xây dựng trên AWS Nitro System.
* Năng lực chính:
  * Quản lý IP: IPv4 CIDR chính và phụ, IPv6 gốc, Amazon VPC IP Address Manager (IPAM).
  * Phân đoạn mạng: subnet public, private, isolated, IPv6-only trên nhiều AZ.
  * Định tuyến và gateway: route table, Internet Gateway, Egress-Only Internet Gateway (IPv6), NAT Gateway.
  * Kết nối hybrid/nhiều VPC: Site-to-Site VPN, Direct Connect, VPC Peering, Transit Gateway, Cloud WAN.
  * Bảo mật: Security Group (stateful), Network ACL (stateless), AWS Network Firewall.
* Địa chỉ IP:
  * IPv4 CIDR từ `/16` (65.536 địa chỉ) đến `/28` (16 địa chỉ); không thể sửa CIDR chính sau khi tạo; có thể thêm tối đa 4 secondary CIDR.
  * CIDR của VPC không được trùng với mạng on-premises, VPC peering hoặc mạng transit.
  * VPC mặc định dùng `172.31.0.0/16`, có một public subnet ở mỗi AZ.
  * IPv6: `/56` cho mỗi VPC và `/64` cho mỗi subnet.
  * Lưu ý chi phí: từ 01/02/2024, AWS tính 0,005 USD/giờ cho mỗi địa chỉ public IPv4.
* Nên dùng custom VPC thay cho default VPC trong production để áp dụng đặc quyền tối thiểu, chia nhiều tầng subnet (Public, Application, Database) và tránh xung đột CIDR.
* Thành phần giám sát: VPC Flow Logs (ghi metadata lưu lượng IP, gửi tới CloudWatch Logs, S3 hoặc Kinesis Data Firehose) và VPC Reachability Analyzer (kiểm tra đường đi giữa các tài nguyên mà không cần gửi gói tin).

**2. Tường lửa trong VPC**

* **Security Group:**
  * Tường lửa ảo cấp instance (ENI), có tính stateful: lưu lượng phản hồi tự động được cho phép.
  * Mỗi instance có thể gắn tối đa 5 Security Group.
  * Mặc định cho phép toàn bộ outbound và chặn toàn bộ inbound.
  * Rule có thể tham chiếu Security Group khác, dịch vụ AWS hoặc dải IP.
* **Network ACL (NACL):**
  * Tường lửa tùy chọn cấp subnet, stateless: phải định nghĩa rule tường minh cho cả inbound và outbound.
  * Mỗi subnet gắn với đúng một NACL (mặc định hoặc tùy chỉnh); rule được đánh giá theo số thứ tự, số nhỏ trước.
  * NACL mặc định cho phép toàn bộ inbound và outbound.
* Hai lớp bổ sung cho nhau: Security Group kiểm soát chi tiết ở cấp instance, NACL bảo vệ rộng ở cấp subnet.

**3. Các bước chuẩn bị (Preparation Steps)**

* Mục tiêu: xây dựng VPC nhiều AZ có public và private subnet theo best practice của AWS.
* Thứ tự thực hiện: tạo VPC → tạo Subnet → tạo Internet Gateway → tạo Route Table → tạo Security Group → bật VPC Flow Logs (có thể xem VPC Resource Map để quan sát cấu trúc).
* Lưu ý chi phí: hầu hết thành phần VPC miễn phí nhưng NAT Gateway có tính phí. Workshop tạo từng thành phần riêng lẻ (không dùng VPC Wizard) để hiểu cách các thành phần kết nối với nhau.

**4. Triển khai Amazon EC2**

* Kiến trúc nhiều AZ, NAT Gateway sẵn sàng cao, truy cập an toàn (SSH và Session Manager), giám sát và cảnh báo bằng CloudWatch, kiểm tra đường đi bằng VPC Reachability Analyzer.
* Thứ tự: tạo EC2 server → kiểm tra kết nối → tạo NAT Gateway → dùng Reachability Analyzer → tạo EC2 Instance Connect Endpoint (tùy chọn) → AWS Systems Manager Session Manager → CloudWatch Monitoring & Alerting. Điều kiện tiên quyết là đã bật VPC Flow Logs.
* Các tài nguyên trong lab: EC2 Public, EC2 Private, NAT-Gateway (NAT-Gateway-AZ1a, NAT-Gateway-AZ1b), các VPC endpoint cho Session Manager: SSM-Endpoint, SSM Messages Endpoint, EC2 Messages Endpoint.

**5. Thiết lập AWS Site-to-Site VPN**

* Site-to-Site VPN kết nối trung tâm dữ liệu on-premises với Amazon VPC.
* Khái niệm:
  * **Virtual Private Gateway (VPG):** điểm kết thúc tunnel phía AWS.
  * **Customer Gateway (CGW):** đại diện cho thiết bị VPN phía khách hàng (phần cứng hoặc phần mềm).
  * **BGP:** định tuyến động nếu CGW hỗ trợ; nếu không thì dùng **static routing** và tự khai báo route.
  * **IPsec:** giao thức tạo tunnel bảo mật qua Internet.
  * Mỗi VPN connection có **2 tunnel** để đảm bảo sẵn sàng cao; kết nối được khởi tạo từ CGW tới VPG.
* Các bước: thiết lập VPG → thiết lập CGW → chọn BGP hoặc static routing → tunnel lên khi có lưu lượng.
* Kịch bản lab: văn phòng chính là VPC **ASG** (`10.10.0.0/16`), chi nhánh là VPC **ASG VPN** (`10.11.0.0/16`), đặt ở các AZ khác nhau. Ban đầu cả hai bên cho phép SSH từ bên ngoài nhưng các instance chưa ping được nhau bằng private IP; mục tiêu là kết nối qua private IP thông qua VPN.
* Khách hàng chịu trách nhiệm chuẩn bị kết nối Internet.

**6. Dọn dẹp tài nguyên (Clean up)**

* Xóa theo đúng thứ tự phụ thuộc để tránh lỗi và dừng phát sinh chi phí:
  1. **EC2 instances:** EC2 Public, EC2 Private, EC2 Customer Gateway (nếu có) → Instance state → Terminate instance.
  2. **NAT Gateway:** NAT-Gateway, NAT-Gateway-AZ1a, NAT-Gateway-AZ1b → Actions → Delete NAT gateway, nhập `delete`; chờ trạng thái Deleted (khoảng 5-10 phút).
  3. **Elastic IP:** Actions → Release Elastic IP addresses.
  4. **VPC endpoints:** SSM-Endpoint, SSM Messages Endpoint, EC2 Messages Endpoint → Actions → Delete VPC endpoints.
  5. **Tài nguyên VPN (nếu có):** xóa Site-to-Site VPN connection (chờ 2-5 phút); detach rồi xóa Virtual Private Gateway; xóa Customer Gateway.
  6. **VPC:** xóa ASG VPN trước, sau đó xóa ASG → Actions → Delete VPC, nhập `delete`.
* Chi phí tham khảo: NAT Gateway 0,045 USD/giờ cộng 0,045 USD/GB xử lý; Elastic IP không dùng 0,005 USD/giờ; VPN connection 0,05 USD/giờ.
* Lưu ý: dữ liệu instance store sẽ mất khi terminate; EBS volume có thể còn lại nếu tắt "Delete on Termination"; CloudWatch Logs, EBS snapshot và S3 bucket có thể vẫn phát sinh phí và cần xóa riêng.
* Kiểm tra lại bằng Billing Dashboard, Cost Explorer, Bills và thiết lập Budget Alerts.

**7. Infrastructure as Code**

* IaC mang lại khả năng lặp lại, quản lý phiên bản, tự động hóa, chuẩn hóa, chính sách bảo mật đồng nhất và dấu vết kiểm toán.
* Workshop cung cấp 3 mẫu template tạo VPC `10.10.0.0/16` nhiều AZ gồm public/private subnet, 2 NAT Gateway, route table, security group, VPC endpoint cho SSM và VPC Flow Logs:
  * **CloudFormation (YAML):** tham số `VpcCidr` và `Environment` (dev, staging, prod); triển khai bằng `aws cloudformation create-stack`.
  * **AWS CDK (TypeScript):** stack `ProductionVpcStack`; triển khai bằng `cdk bootstrap` và `cdk deploy`.
  * **Terraform:** file `terraform/main.tf`; triển khai bằng `terraform init`, `plan`, `apply`.
* Best practice: không mở SSH từ `0.0.0.0/0` trong production, dùng Secrets Manager hoặc Parameter Store cho bí mật, IAM đặc quyền tối thiểu, mã hóa dữ liệu, triển khai nhiều AZ, dùng CI/CD và tách môi trường dev/staging/prod.


### Kết quả đạt được tuần 3:

* Hiểu vai trò của IAM và phân biệt 4 thành phần:
  * IAM User
  * IAM Group
  * IAM Policy
  * IAM Role

* Hiểu vì sao không nên dùng root user cho công việc hằng ngày và cần bật MFA.

* Nắm quy trình quản lý quyền theo mô hình: tạo group quản trị, tạo user, gán quyền qua group; tạo role `Admin` với `AdministratorAccess`, tạo `OperatorUser` và dùng Switch Role để đảm nhận quyền tạm thời.

* Hiểu nguyên tắc đặc quyền tối thiểu, ưu tiên dùng role với credential tạm thời, và biết cách dọn dẹp user/group sau khi thực hành.

* Hiểu cấu trúc Amazon VPC: CIDR, subnet public/private/isolated, route table, Internet Gateway, NAT Gateway, IPv6 và IPAM.

* Phân biệt được Security Group (stateful, cấp instance) và Network ACL (stateless, cấp subnet, đánh giá rule theo thứ tự).

* Hiểu vai trò của VPC Flow Logs và VPC Reachability Analyzer trong giám sát và khắc phục sự cố mạng.

* Nắm quy trình xây dựng VPC nhiều AZ và triển khai EC2 với NAT Gateway, Session Manager, CloudWatch.

* Hiểu mô hình AWS Site-to-Site VPN: VPG, CGW, BGP/static routing, 2 tunnel IPsec.

* Biết thứ tự dọn dẹp tài nguyên VPC (EC2 → NAT Gateway → Elastic IP → VPC endpoint → VPN → VPC) và các khoản chi phí dễ phát sinh như NAT Gateway, Elastic IP, public IPv4, VPN connection.

* Biết đến các mẫu Infrastructure as Code (CloudFormation, CDK, Terraform) để triển khai VPC tự động.
* ...


















### Kết quả đạt được tuần 3: