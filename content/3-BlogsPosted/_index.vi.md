---
title: "Các bài blogs đã đăng"
date: 2026-10-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 - EC2 AMI TAG SHARING: CHIA SẺ TAG CỦA AMI GIỮA CÁC TÀI KHOẢN AWS](3.1-Blog1/)
Blog này giới thiệu tính năng EC2 AMI Tag Sharing, cho phép chủ sở hữu AMI chia sẻ các tag mang tiền tố `ec2:SharedTag/` cùng với AMI sang các tài khoản AWS khác một cách hoàn toàn tự động (native). Tính năng này giúp tối ưu hóa mạnh mẽ quy trình phân phối Golden AMI, giảm thiểu rủi ro lệch tag (tag drift) và loại bỏ sự phức tạp khi phải duy trì hệ thống EventBridge, SNS và Lambda đồng bộ trung gian.

###  [Blog 2 - CHUYỂN ĐỔI EC2 TỪ AMI MARKETPLACE SANG BYOL BẰNG CÔNG CỤ TỰ ĐỘNG HÓA](blog2/)
Bài viết phân tích lý thuyết và kiến trúc về việc chuyển đổi EC2 instance từ AMI Marketplace License Included sang mô hình Bring Your Own License (BYOL) bằng công cụ tự động hóa `sample-ami-to-byol`. Nội dung bóc tách kiến trúc phân rã của EC2 (Compute, Storage, Network, Identity), mô hình máy trạng thái, các ràng buộc thứ tự quan trọng và bài toán kinh tế đám mây (TCO).

###  [Blog 3 - ...](3.3-Blog3/)
Blog này giới thiệu Amazon EKS Pod Identity vừa bổ sung tính năng session policies, cho phép bạn thu hẹp quyền IAM một cách linh hoạt và chính xác cho từng pod mà không cần tạo thêm nhiều IAM roles riêng biệt. Đây là bước tiến quan trọng giúp áp dụng nguyên tắc least privilege hiệu quả hơn trong môi trường Kubernetes quy mô lớn.