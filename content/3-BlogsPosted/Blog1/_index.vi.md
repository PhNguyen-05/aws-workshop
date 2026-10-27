---
title: "Blog 1"
date: 2026-10-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# EC2 AMI TAG SHARING: CHIA SẺ TAG CỦA AMI GIỮA CÁC TÀI KHOẢN AWS

> Bài viết tóm tắt, phân tích và thực hành dựa trên bài gốc **"Introducing EC2 AMI Tag Sharing: Share EC2 tags across AWS accounts"** (AWS Compute Blog, 05/10/2026, tác giả Gena Gizzi và Smriti Ohri).
> Nguồn: <https://aws.amazon.com/blogs/compute/introducing-ec2-ami-tag-sharing-share-ec2-tags-across-aws-accounts/>

## 1. Bối cảnh: tag và AMI

**Tag** là các cặp key–value gắn vào tài nguyên AWS. Chúng được dùng để theo dõi chi phí (FinOps), kiểm soát tuân thủ (security) và phân quyền theo thuộc tính (ABAC – Attribute-Based Access Control).

**AMI (Amazon Machine Image)** là "bản mẫu" dùng để khởi tạo EC2 instance. Nhiều tổ chức gắn tag cho AMI để ghi trạng thái phê duyệt, phiên bản hệ điều hành, nhóm sở hữu...

## 2. Vấn đề trước đây

Khi chia sẻ một AMI sang tài khoản AWS khác, **tag không đi theo AMI**. Tài khoản nhận thấy image nhưng không thấy các thông tin mô tả nó. Muốn giải quyết, tổ chức phải tự dựng quy trình sao chép tag, thường dùng Amazon SNS và AWS Lambda. Cách này có ba nhược điểm:

- Phải xây dựng và bảo trì thêm hạ tầng.
- Số lệnh `CreateTags` tăng nhanh khi có nhiều AMI, nhiều Region, nhiều tài khoản, dễ bị throttling hoặc Lambda lỗi.
- Tag có thể bị lệch (tag drift) khi một lần gọi thất bại mà không ai biết.

## 3. EC2 Tag Sharing là gì?

Đây là tính năng cho phép chủ sở hữu AMI **chia sẻ tag cùng với AMI** cho mọi tài khoản có quyền truy cập image (chia sẻ riêng cho tài khoản, cho cả tổ chức, hoặc công khai).

**Cách dùng rất đơn giản:** đặt tiền tố `ec2:SharedTag/` vào đầu key của tag.

| Key của tag | Ai nhìn thấy |
|---|---|
| `ec2:SharedTag/status` | Chủ sở hữu và mọi tài khoản được chia sẻ AMI |
| `team` (không có tiền tố) | Chỉ chủ sở hữu (tag riêng) |

**Các quy tắc cần nhớ:**

- Chỉ **chủ sở hữu AMI** mới được tạo, sửa, xóa tag có tiền tố `ec2:SharedTag/`.
- Tài khoản nhận chỉ **xem** được shared tag, nhưng vẫn thêm được tag riêng của mình lên AMI đó.
- Khi chủ sở hữu cập nhật shared tag, thay đổi tự động xuất hiện ở các tài khoản nhận, không cần quy trình sao chép.
- Shared tag tính vào hạn mức **50 tag/tài nguyên** của chủ sở hữu, không tính vào hạn mức của tài khoản nhận.
- Khi copy AMI có shared tag, cần thêm tham số `--copy-image-tags` để tag được sao chép theo.
- Ở thời điểm ra mắt, tính năng **chỉ hỗ trợ tài nguyên AMI**.

## 4. Use case tiêu biểu: Golden AMI

Nhiều tổ chức có một tài khoản "build factory" chuyên tạo các AMI đã được gia cố bảo mật (golden AMI) rồi chia sẻ cho hàng trăm tài khoản workload. Các tài khoản này dùng tag như `status=approved` hoặc `patch-date=...` để áp chính sách chỉ cho phép khởi chạy từ AMI đã được duyệt.

| | Trước đây | Với Tag Sharing |
|---|---|---|
| Cách đồng bộ tag | SNS gửi thông báo, Lambda ở từng tài khoản gọi `CreateTags` | Chủ sở hữu gắn tag có tiền tố `ec2:SharedTag/` một lần |
| Khi cập nhật (ví dụ đánh dấu deprecated) | Phải chạy lại quy trình | Tự động hiển thị ở mọi tài khoản |
| Rủi ro | Throttling, Lambda lỗi, tag drift | Giảm đáng kể |

## 5. Những điều cần lưu ý

1. **Không đưa thông tin nhạy cảm vào shared tag**, vì mọi tài khoản có quyền truy cập AMI đều đọc được.
2. **Chính sách ABAC ở tài khoản nhận vẫn áp dụng cho shared tag.** Ví dụ, nếu chính sách chỉ cho `ec2:RunInstances` khi AMI có tag `status=approved`, thì tag `ec2:SharedTag/status=approved` sẽ được tính theo chính sách đó.
3. **Quyền kiểm soát nằm ở chủ sở hữu AMI.** Tài khoản nhận không sửa được giá trị mà chính sách của mình phụ thuộc, nên chỉ nên dựa vào shared tag khi tin tưởng chủ sở hữu.
4. Nên kết hợp với tính năng **Allowed AMIs** để chỉ cho phép dùng AMI từ các nhà cung cấp đáng tin cậy (có thể bắt đầu ở chế độ audit để xem tác động trước khi áp dụng).

## 6. Thực hành của mình

> **TODO:** Điền sau khi bạn làm xong theo file hướng dẫn thực hành (`HUONG-DAN-THUC-HANH.md`). Chỉ ghi những gì bạn thực sự đã làm.

- Môi trường: Region ..., instance type ..., AMI nền ...
- Các bước đã làm: tạo AMI → gắn tag riêng và shared tag → kiểm tra bằng `describe-tags` và `describe-images` → xóa shared tag → dọn dẹp.
- Kết quả quan sát: ...
- Khó khăn gặp phải và cách xử lý: ...

![Danh sách tag trên AMI](/images/blog1/ami-tags-console.png)
![Kết quả describe-tags](/images/blog1/describe-tags-cli.png)

## 7. Nhận xét của mình

> **TODO:** Viết 3–5 câu bằng cách hiểu của riêng bạn. Gợi ý các ý để triển khai:
> - Tag Sharing giải quyết được "khoảng trống" nào so với cách dùng SNS + Lambda?
> - Rủi ro khi để tài khoản khác quyết định giá trị tag mà chính sách ABAC của mình phụ thuộc.
> - Bạn sẽ áp dụng tag nào cho AMI nếu làm trong một dự án thực tế?

## 8. Tài liệu tham khảo

- [Introducing EC2 AMI Tag Sharing: Share EC2 tags across AWS accounts (AWS Compute Blog)](https://aws.amazon.com/blogs/compute/introducing-ec2-ami-tag-sharing-share-ec2-tags-across-aws-accounts/)
- [Tag your Amazon EC2 resources (EC2 User Guide)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)
- [Allowed AMIs (EC2 User Guide)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-allowed-amis.html)
- Bài đăng trên nhóm AWS Study Group: 