# Câu hỏi thường gặp về Dịch Vụ S3 Storage

> **Danh mục:** Dịch vụ S3 Storage
> **Cập nhật lần cuối:** 2026-08-21
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** s3, object storage, lưu trữ đối tượng, backup, s3 api

## Tổng quan

**Hỏi: Dịch vụ S3 là gì?**
Trả lời: S3 (Simple Storage Service) là dịch vụ lưu trữ đối tượng (Object Storage) cho phép lưu trữ và quản lý dữ liệu như hình ảnh, video, tài liệu, file sao lưu và nhiều loại dữ liệu khác. Dịch vụ được thiết kế với khả năng mở rộng linh hoạt, độ bền dữ liệu cao và truy cập nhanh qua Internet hoặc mạng nội bộ.

**Hỏi: Tôi có thể lưu trữ những loại dữ liệu nào trên S3?**
Trả lời: Bạn có thể lưu trữ hầu hết mọi loại tệp tin: hình ảnh, video, tài liệu, file backup database, log hệ thống, file cài đặt, tài nguyên tĩnh của website/ứng dụng và nhiều loại dữ liệu phi cấu trúc khác.

**Hỏi: Dữ liệu trên S3 có an toàn không?**
Trả lời: Có. Dữ liệu được bảo vệ bằng cơ chế bảo mật đa lớp, hạ tầng đặt tại Việt Nam, cùng khả năng sao lưu và phục hồi hiệu quả giúp giảm thiểu rủi ro mất mát dữ liệu.

**Hỏi: Tôi có thể truy cập dữ liệu S3 từ đâu?**
Trả lời: Bạn có thể truy cập dữ liệu từ bất kỳ đâu có kết nối Internet thông qua chuẩn S3 Protocol/API, hoặc qua mạng nội bộ nếu triển khai theo mô hình riêng.

**Hỏi: Dịch vụ S3 có hỗ trợ sao lưu và khôi phục dữ liệu không?**
Trả lời: Có. S3 Storage được sử dụng phổ biến làm điểm lưu trữ backup an toàn cho máy chủ, database, giúp phòng chống ransomware và khôi phục dữ liệu nhanh chóng khi cần.

**Hỏi: Chi phí sử dụng dịch vụ S3 được tính như thế nào?**
Trả lời: Chi phí được tính theo dung lượng lưu trữ thực tế sử dụng mỗi tháng, chia theo các mức Lite, Basic, Pro, Enterprise hoặc tùy chỉnh dung lượng theo nhu cầu qua công cụ ước tính giá.

**Hỏi: Tôi có thể tích hợp S3 với ứng dụng hiện có không?**
Trả lời: Có. S3 tương thích 100% chuẩn S3 API, dễ dàng tích hợp với các SDK quen thuộc (Boto3, AWS SDK), công cụ phổ biến (rclone, Cyberduck, MinIO) cũng như quy trình CI/CD và kiến trúc Microservices mà không cần sửa code, chỉ cần đổi endpoint.

## Bảng giá tham khảo dịch vụ S3 (cam kết 1 năm)

Giá đã bao gồm ưu đãi cam kết theo năm, chưa bao gồm 10% phí giá trị gia tăng (VAT) khi xuất hóa đơn.

**Hỏi: Giá gói S3 Lite là bao nhiêu?**
Trả lời: Gói **Lite** giá 45.000đ/tháng (giá gốc 50.000đ). Dung lượng: 50GB - 99GB. Đơn giá vượt mức: 100.000đ/100GB.

**Hỏi: Giá gói S3 Basic là bao nhiêu?**
Trả lời: Gói **Basic** giá 90.000đ/tháng (giá gốc 100.000đ). Dung lượng: 100GB - 499GB. Đơn giá vượt mức: 100.000đ/100GB.

**Hỏi: Giá gói S3 Pro là bao nhiêu?**
Trả lời: Gói **Pro** giá 427.500đ/tháng (giá gốc 475.000đ), là gói được sử dụng phổ biến nhất. Dung lượng: 500GB - 1023GB. Đơn giá vượt mức: 95.000đ/100GB.

**Hỏi: Giá gói S3 Enterprise là bao nhiêu?**
Trả lời: Gói **Enterprise** giá 829.000đ/tháng (giá gốc 921.600đ). Dung lượng: 1TB trở lên. Đơn giá vượt mức: 90.000đ/100GB.

**Hỏi: Các gói S3 khác nhau như thế nào?**
Trả lời: 4 gói S3 (Lite, Basic, Pro, Enterprise) khác nhau theo dung lượng lưu trữ và đơn giá vượt mức: Lite (50GB-99GB, 45.000đ/tháng, vượt mức 100.000đ/100GB), Basic (100GB-499GB, 90.000đ/tháng, vượt mức 100.000đ/100GB), Pro (500GB-1023GB, 427.500đ/tháng, vượt mức 95.000đ/100GB, phổ biến nhất), Enterprise (1TB trở lên, 829.000đ/tháng, vượt mức 90.000đ/100GB). Đơn giá vượt mức giảm dần khi dùng gói dung lượng lớn hơn.

## Hướng dẫn kỹ thuật kết nối S3

**Hỏi: Cách kết nối và quản lý S3 bằng Cyberduck và S3 Browser như thế nào?**
Trả lời: Để kết nối S3 Thế Giới Số (s3.tgs.com.vn, cổng 9000) qua 2 công cụ phổ biến:
Cách 1 - Cyberduck: tải ứng dụng tại https://cyberduck.io/download/. Khi cấu hình kết nối, chọn kiểu S3 (HTTP, Deprecated path style requests), nhập Access Key là username và Secret Access Key là password. Nếu không chỉnh được cổng, vào Edit → Preferences → Profile → Deprecated. Cyberduck hỗ trợ upload file, download file, xóa file, đổi tên file, tạo thư mục mới, di chuyển file.
Cách 2 - S3 Browser: tải tại https://s3browser.com/download.aspx. Khi thiết lập tài khoản, chọn Account type là S3 Compatible Storage, nhập API Endpoint là địa chỉ server, Access ID là username, Secret Access Key là password, rồi chọn "Add new account" để hoàn tất thêm tài khoản. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cach-su-dung-cyberduck-ket-noi-voi-minio/

**Hỏi: Cách cài đặt MinIO trên Ubuntu 22.04 như thế nào?**
Trả lời: Các bước cài đặt và cấu hình MinIO object storage trên Ubuntu 22.04:
- Chuẩn bị hệ thống: cập nhật các gói hệ thống, tạo tài khoản riêng minio-user (không dùng root), tạo thư mục lưu trữ dữ liệu tại /data/minio.
- Cài đặt MinIO Server: tải và cài binary MinIO vào /usr/local/bin, cấu hình thông tin đăng nhập và đường dẫn lưu trữ trong /etc/minio/minio.conf, tạo systemd service để tự khởi động cùng hệ thống, bật và chạy service.
- Cấu hình mạng: mở firewall cho port 9000 (API) và 9001 (Web UI).
- Truy cập giao diện web: truy cập http://IP_SERVER:9001 với thông tin đăng nhập đã cấu hình.
- Cài đặt MinIO Client: cài công cụ `mc` và thiết lập alias kết nối đến server.
- Quản lý Bucket & File: tạo bucket và upload file thử nghiệm.
- Quản lý người dùng & quyền: tạo user, thiết lập quota cho bucket, định nghĩa policy dạng JSON, và gán quyền cho từng tài khoản. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-minio-tren-ubuntu-22-04/
