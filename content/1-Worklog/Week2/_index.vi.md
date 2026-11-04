---
title: "Worklog Tuần 2"
date: 2026-10-12
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---


### Mục tiêu tuần 2:

* Hiểu vai trò của việc quản lý chi phí khi vận hành hệ thống trên AWS và nắm được dịch vụ AWS Budget.
* Phân biệt 4 loại budget: Cost Budget, Usage Budget, RI Budget và Savings Plans Budget.
* Thực hành tạo budget từ template và tạo budget tùy chỉnh (Customize) trong AWS Billing and Cost Management, cấu hình ngưỡng cảnh báo qua email.
* Tìm hiểu các gói hỗ trợ của AWS Support (Basic, Business Support+, Enterprise Support, Unified Operations) và thời gian phản hồi tương ứng.
* Nắm quy trình truy cập AWS Support Center, tạo và quản lý support request, biết cách chọn mức độ nghiêm trọng (severity) phù hợp.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| 2 | - Tìm hiểu tổng quan về AWS Budgets: <br>&emsp; + Mục đích: đặt ngân sách và gửi cảnh báo khi chi phí vượt (hoặc dự báo vượt) hạn mức <br>&emsp; + 4 loại budget: Cost, Usage, RI, Savings Plans <br>&emsp; + Lợi ích: kiểm soát chi phí, cảnh báo kịp thời, lập kế hoạch tài chính, tối ưu tài nguyên, phân bổ chi phí <br> - Tìm hiểu các bước thiết lập budget hiệu quả: xác định mục tiêu, phân tích chi phí hiện tại, đặt ngưỡng cảnh báo (thường 80% và 100%), cấu hình thông báo, theo dõi và điều chỉnh <br> - Phân biệt Reserved Instances và Savings Plans | 12/10/2026 | 12/10/2026 | <https://000007.awsfcaj.com/> |
| 3 | - **Thực hành:** <br>&emsp; + Truy cập **AWS Billing and Cost Management → Budgets → Create a budget** <br>&emsp; + Tạo budget bằng template: chọn **Use a template (simplified)** → **Monthly cost budget**, đặt tên, số tiền hàng tháng, ngưỡng cảnh báo <br>&emsp; + Xem lịch sử budget và các loại cảnh báo có sẵn trong template <br>&emsp; + Tạo **Cost Budget** tùy chỉnh (Customize): đặt tên `Monthly`, chọn Period, Recurring/Expiring, Fixed/Monthly Budget Planning, nhập Budgeted amount, scope **All AWS services**, **Unblended costs**, thêm alert threshold | 13/10/2026 | 13/10/2026 | <https://000007.awsfcaj.com/1-create-budget/> <br> <https://000007.awsfcaj.com/2-cost-budgets/> |
| 4 | - **Thực hành:** <br>&emsp; + Tạo **Usage Budget**: chọn **Usage type groups**, **EC2: ELB - Running Hours**, đặt giới hạn số giờ chạy, cấu hình alert threshold và email <br>&emsp; + Xem **Budget health** và **Budget history** <br>&emsp; + Xem lại quy trình tạo **RI Budget** (Reservation budget) và **Savings Plans Budget** (chỉ mang tính minh họa vì cần trả trước/cam kết dài hạn) <br>&emsp; + **Clean up:** chọn budget → **Actions → Delete** → xác nhận xóa, lặp lại cho tất cả budget đã tạo | 14/10/2026 | 14/10/2026 | <https://000007.awsfcaj.com/3-usage-budget/> <br> <https://000007.awsfcaj.com/4-reservation-budget/> <br> <https://000007.awsfcaj.com/5-saving-plans-budget/> <br> <https://000007.awsfcaj.com/6-clean-up/> |
| 5 | - Tìm hiểu **AWS Support** và các gói hỗ trợ: <br>&emsp; + Basic <br>&emsp; + Business Support+ <br>&emsp; + Enterprise Support <br>&emsp; + Unified Operations <br> - So sánh tính năng và thời gian phản hồi của từng gói <br> - Tìm hiểu cách truy cập AWS Support: các loại support request và quy trình đổi gói hỗ trợ | 15/10/2026 | 15/10/2026 | <https://000009.awsfcaj.com/> <br> <https://000009.awsfcaj.com/1-support-plans/> <br> <https://000009.awsfcaj.com/2-access-support/> |
| 6 | - Tìm hiểu cách quản lý support request trong AWS Support Center: <br>&emsp; + Tạo support request <br>&emsp; + Chọn mức độ nghiêm trọng (severity) phù hợp <br>&emsp; + Theo dõi và phản hồi yêu cầu <br> - Ghi nhớ các lưu ý bảo mật khi tạo support case <br> - Tổng hợp kiến thức tuần 2 và viết worklog | 16/10/2026 | 16/10/2026 | <https://000009.awsfcaj.com/3-manage-cases/> |


### Chi tiết nội dung thực hành và tìm hiểu:

#### Lab 7: Quản lý chi phí với AWS Budgets

**1. Tổng quan về AWS Budgets**

* AWS Budgets cho phép đặt ngân sách và gửi cảnh báo khi chi phí vượt hạn mức (hoặc được dự báo sẽ vượt).
* Bốn loại budget:
  * **Cost Budget:** cảnh báo khi tổng chi phí vượt ngưỡng đã đặt.
  * **Usage Budget:** cảnh báo khi mức sử dụng của dịch vụ được chọn vượt ngưỡng (ví dụ: số giờ chạy của EC2).
  * **RI Budget:** cảnh báo dựa trên việc sử dụng các Reserved Instances đã trả trước.
  * **Savings Plans Budget:** cảnh báo dựa trên việc sử dụng các dịch vụ nằm trong Savings Plans.
* Reserved Instances và Savings Plans đều là cách giảm chi phí bằng cam kết sử dụng 1 - 3 năm. Savings Plans là mô hình mới hơn, linh hoạt hơn với mức giảm giá tương đương; với EC2 nên ưu tiên dùng Savings Plans.
* Lợi ích: kiểm soát chi phí chủ động, nhận cảnh báo kịp thời, hỗ trợ lập kế hoạch tài chính và dự báo chi phí, xác định dịch vụ tốn kém để tối ưu, dễ phân bổ chi phí cho dự án/phòng ban/khách hàng.
* Các bước thiết lập budget hiệu quả: (1) xác định mục tiêu ngân sách, (2) phân tích chi phí hiện tại, (3) đặt ngưỡng cảnh báo phù hợp (thường là 80% và 100%), (4) cấu hình thông báo gửi đến đúng người có thẩm quyền, (5) theo dõi và điều chỉnh định kỳ.

**2. Tạo budget bằng template**

* Mở AWS Management Console, tìm dịch vụ **AWS Billing and Cost Management**.
* Ở menu bên trái chọn **Budgets**, nhấn **Create a budget**.
* Chọn **Use a template (simplified)**, trong mục Templates chọn **Monthly cost budget**.
* Nhập tên budget, số tiền ngân sách hàng tháng, ngưỡng cảnh báo rồi nhấn **Create budget**.
* Kiểm tra budget đã được tạo, xem Budget history và các loại cảnh báo có sẵn trong template.
* Lưu ý: budget alert chỉ cảnh báo, **không tự động dừng tài nguyên** khi vượt ngân sách; nên kết hợp với AWS Service Quotas hoặc IAM policy để kiểm soát. Cần phân quyền để chỉ người có thẩm quyền được sửa/xóa budget.

**3. Tạo Cost Budget (Customize)**

* Lưu ý: tài khoản AWS mới ban đầu có thể chỉ tạo được Cost Budget, các loại khác có thể chưa khả dụng ngay.
* Trong **Budget setup** chọn **Customize**, ở **Budget types** chọn **Cost budget**, nhấn **Next**.
* Trong **Set your budget**:
  * **Budget name:** `Monthly`
  * **Period:** Daily / Monthly / Quarterly / Annually
  * **Budget effective dates:** Recurring Budget (lặp lại định kỳ) hoặc Expiring Budget (chỉ áp dụng một lần). Múi giờ của AWS Budgets dùng chuẩn UTC.
  * **Specify your monthly budget:** Fixed (cố định mỗi kỳ) hoặc Monthly Budget Planning (đặt khác nhau cho từng tháng); nhập **Budgeted amount**.
* **Budget scope:** chọn **All AWS services**; **Aggregate costs by:** **Unblended costs**; nhấn **Next**.
* Cấu hình cảnh báo: chọn **Add an alert threshold**, thiết lập chi tiết Alert, nhấn **Next**; xem lại phần action (nếu có), nhấn **Next**.
* Xem lại toàn bộ cấu hình, nhấn **Create budget** và xác nhận budget đã tạo thành công.
* Khuyến nghị: đặt nhiều ngưỡng cảnh báo (ví dụ 50%, 80%, 100%) để chủ động kiểm soát chi phí.

**4. Tạo Usage Budget**

* Quy trình tương tự Cost Budget, khác ở chỗ Usage Budget hoạt động dựa trên **mức sử dụng tài nguyên** thay vì chi phí.
* Trong **Budget setup** chọn **Customize**, ở **Budget types** chọn **Usage budget**, nhấn **Next**.
* Nhập **Budget name**; ở mục **Budget against** chọn **Usage type groups** và chọn **EC2: ELB - Running Hours** để theo dõi số giờ chạy của EC2.
* Cấu hình **Set budget amount**: Period (Daily/Monthly/Quarterly/Annually), Budget renewal type (Recurring/Expiring), Budgeting method (Fixed/Planned) và số giờ sử dụng tối đa.
* Giữ mặc định ở **Budget scope**, nhấn **Next**.
* Ở **Configure alerts**: chọn **Add an alert threshold**, đặt ngưỡng (% ngân sách) và thêm email nhận thông báo, nhấn **Next**.
* Xem lại và nhấn **Create budget**.
* Sau khi tạo, xem **Budget health** để so sánh mức sử dụng hiện tại với ngân sách và **Budget history** để theo dõi xu hướng.
* Đặc biệt hữu ích với các dịch vụ tính phí theo giờ như EC2. Chỉ nên gửi thông báo cho người phụ trách quản lý chi phí để tránh lộ thông tin tài chính.

**5. Tạo RI Budget (minh họa)**

* Do Reserved Instances yêu cầu trả trước nên phần này chỉ mang tính minh họa.
* Trong **Budget setup** chọn **Customize** → **Reservation budget** → **Next**.
* Đặt tên budget, cấu hình **Coverage threshold**, cấu hình **Budget scope**.
* Ở **Alert setting** nhập email, chọn **Next**, sau đó **Create budget**.
* Mục đích: theo dõi mức độ sử dụng Reserved Instances để tiết kiệm chi phí; đặt ngưỡng cảnh báo phù hợp để tối ưu việc sử dụng.

**6. Tạo Savings Plans Budget (minh họa)**

* Quy trình rất giống RI Budget; khác biệt là Reservation Budget liên quan đến Reserved Instances, còn Savings Plans Budget liên quan đến cam kết Savings Plans.
* Trong **Budget setup** chọn **Customize** → **Savings Plans budget** → **Next**.
* Nhập **Budget name**, cấu hình **Utilization threshold**, giữ mặc định **Budget scope**, nhập email ở **Alert setting**, chọn **Next** rồi **Create budget**.
* Savings Plans có thể giúp tiết kiệm tới 72% so với giá On-Demand khi cam kết mức sử dụng compute ổn định (USD/giờ) trong 1 hoặc 3 năm. Nên đặt ngưỡng cảnh báo 80-90% để có thời gian điều chỉnh.

**7. Dọn dẹp tài nguyên (Clean up)**

* Vào **AWS Billing and Cost Management → Budgets**.
* Chọn budget cần xóa → **Action** → **Delete** → xác nhận **Delete** trong hộp thoại.
* Lặp lại cho tất cả budget đã tạo trong quá trình thực hành.
* Xóa budget **không ảnh hưởng** đến tài nguyên đang chạy vì budget chỉ là công cụ giám sát và cảnh báo.

#### Lab 9: Yêu cầu hỗ trợ với AWS Support

**1. Các gói hỗ trợ của AWS Support**

AWS Support chia thành 4 gói, sắp xếp từ thấp đến cao: **Basic → Business Support+ → Enterprise Support → Unified Operations**. Gói cao hơn bao gồm các dịch vụ của gói thấp hơn.

| Gói | Đặc điểm chính |
| :--- | :--- |
| **Basic** | - Miễn phí cho mọi khách hàng AWS <br> - Hỗ trợ các vấn đề về tài khoản và thanh toán, yêu cầu tăng service limit <br> - Truy cập 24/7: live chat 1-1 cho câu hỏi tài khoản/thanh toán, AWS Support Forums, AWS Health Dashboard, tài liệu và best practice guides |
| **Business Support+** | - Dành cho workload production <br> - Hỗ trợ 24/7 qua điện thoại, chat, email với Cloud Support Engineers <br> - Không giới hạn số support case và số người liên hệ <br> - AWS Trusted Advisor đầy đủ các kiểm tra (tối ưu chi phí, bảo mật, khả năng chịu lỗi, hiệu năng, giới hạn dịch vụ) <br> - AWS Support API <br> - Hỗ trợ phần mềm bên thứ ba (hệ điều hành, nền tảng, thành phần application stack phổ biến) <br> - Thời gian phản hồi: hướng dẫn chung < 24 giờ; hệ thống bị suy giảm < 12 giờ; hệ thống production bị suy giảm < 4 giờ; hệ thống production ngừng hoạt động < 1 giờ |
| **Enterprise Support** | - Bao gồm toàn bộ Business Support+ <br> - Technical Account Manager (TAM) <br> - Concierge Support Team hỗ trợ thanh toán và tài khoản <br> - Infrastructure Event Management <br> - Well-Architected Reviews <br> - Operations Reviews and Tools <br> - Training và Game Days <br> - Hệ thống nghiệp vụ trọng yếu ngừng hoạt động: phản hồi < 15 phút; các mức khác giống Business Support+ |
| **Unified Operations** | - Bao gồm toàn bộ Enterprise Support <br> - AWS Managed Services (AMS) <br> - Giám sát chủ động và quản lý sự cố 24/7 <br> - Quản lý thay đổi (change management) <br> - Quản lý bản vá (patch management) <br> - Sao lưu và khôi phục thảm họa <br> - Giám sát bảo mật và tuân thủ liên tục <br> - Đội ngũ service delivery chuyên trách |

* Cần chọn gói hỗ trợ phù hợp với yêu cầu kỹ thuật và nguồn lực của hệ thống; gói hỗ trợ cũng ảnh hưởng đến khả năng tiếp cận tư vấn bảo mật và tuân thủ từ đội ngũ AWS.
* Thời gian phản hồi dao động từ 15 phút đến 24 giờ tùy gói hỗ trợ và mức độ nghiêm trọng; AWS Support hỗ trợ đa ngôn ngữ trên phạm vi toàn cầu.

**2. Truy cập AWS Support**

* Tìm hiểu các loại support request và quy trình thay đổi gói hỗ trợ.
* Chọn đúng gói hỗ trợ giúp tối ưu chi phí vận hành nhưng vẫn đảm bảo mức hỗ trợ cần thiết cho workload quan trọng.

**3. Quản lý support request**

* AWS Support Center là nơi tập trung để tạo, quản lý và theo dõi mọi yêu cầu hỗ trợ.
* Cung cấp thông tin chi tiết và đầy đủ khi tạo request giúp đội ngũ AWS giải quyết nhanh hơn.
* **Bảo mật:** không chia sẻ access key, mật khẩu hoặc thông tin xác thực khác trong nội dung support request.
* **Chọn severity phù hợp:** chọn sai mức độ nghiêm trọng có thể dẫn đến thời gian phản hồi không như mong đợi hoặc làm chậm việc xử lý.
* Sau khi tạo request, cần thường xuyên kiểm tra AWS Support Center để cập nhật và phản hồi kịp thời các câu hỏi từ đội ngũ hỗ trợ.


### Kết quả đạt được tuần 2:

* Hiểu vai trò của AWS Budgets trong việc kiểm soát chi phí và nắm được 4 loại budget:
  * Cost Budget
  * Usage Budget
  * RI Budget
  * Savings Plans Budget

* Phân biệt được Reserved Instances và Savings Plans, biết vì sao nên ưu tiên Savings Plans cho EC2.

* Biết cách truy cập AWS Billing and Cost Management và tạo budget theo 2 cách:
  * Dùng template (Monthly cost budget)
  * Tùy chỉnh (Customize) với đầy đủ các thiết lập: period, recurring/expiring, fixed/planned, scope, aggregate costs
  
* Thực hành tạo Cost Budget và Usage Budget (theo dõi giờ chạy EC2), cấu hình ngưỡng cảnh báo và email nhận thông báo.

* Biết theo dõi Budget health và Budget history để đánh giá mức sử dụng so với ngân sách.

* Hiểu quy trình tạo RI Budget và Savings Plans Budget.

* Biết cách dọn dẹp các budget sau khi thực hành và hiểu rằng xóa budget không ảnh hưởng đến tài nguyên đang chạy.

* Hiểu rằng budget chỉ cảnh báo chứ không tự động chặn việc sử dụng tài nguyên.

* Nắm được 4 gói hỗ trợ của AWS Support (Basic, Business Support+, Enterprise Support, Unified Operations), tính năng và thời gian phản hồi của từng gói.

* Hiểu quy trình quản lý support request trong AWS Support Center, tầm quan trọng của việc chọn đúng severity và các lưu ý bảo mật khi tạo support case.
* ...


