# Hướng dẫn phần cứng và quản trị Server vật lý

> **Danh mục:** Hướng dẫn kỹ thuật - Server
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** server, idrac, dell, raid, vmware esxi, vcenter, ssl vcenter

## Khái niệm

**Hỏi: Các loại server phổ biến nhất hiện nay là gì?**
Trả lời: Server (máy chủ) là hệ thống phần cứng hoặc phần mềm được thiết kế để cung cấp, xử lý và quản lý tài nguyên cho các thiết bị khác trong mạng. Phân loại theo phương pháp xây dựng: Dedicated Server (máy chủ chuyên dụng) — dành riêng cho một khách hàng, tối ưu cho website lớn và hệ thống tài chính, không chia sẻ tài nguyên với hệ thống khác; Virtual Private Server (VPS) — chia từ server vật lý thành nhiều server con độc lập, phù hợp cho website vừa và nhỏ, cho phép nâng cấp tài nguyên linh hoạt; Cloud Server (máy chủ đám mây) — chạy trên nền tảng đám mây với khả năng mở rộng linh hoạt, có chức năng cân bằng tải và phục hồi sau sự cố, lý tưởng cho ứng dụng có lưu lượng biến động. Phân loại theo chức năng: Application Server (lưu trữ và quản lý ứng dụng backend), Database Server (lưu trữ và quản lý dữ liệu, hỗ trợ sao lưu), File Server (cung cấp không gian lưu trữ và chia sẻ tệp), Mail Server (quản lý dịch vụ email với các tính năng bảo mật).
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cac-loai-server-pho-bien-nhat-hien-nay/

## Quản trị máy chủ Dell (iDRAC, RAID)

**Hỏi: Cách sử dụng iDRAC để cài đặt hệ điều hành cho máy chủ Dell như thế nào?**
Trả lời: iDRAC (Integrated Dell Remote Access Controller) giúp quản trị viên giám sát, điều khiển và khắc phục sự cố hệ thống từ xa mà không cần có mặt tại chỗ. Phần 1 - Cấu hình iDRAC:
- Bước 1: Khởi động máy chủ, nhấn F2 để vào System Setup.
- Bước 2: Vào iDRAC Settings → Network.
- Bước 3: Vô hiệu hóa DHCP và đặt static IP, DNS.
- Bước 4: Quay lại Setup → User Configuration để thiết lập password.
- Bước 5: Hoàn tất và khởi động lại server.
- Bước 6: Kiểm tra kết nối bằng ping và đăng nhập giao diện web.
Phần 2 - Mount ISO và cài đặt OS:
- Bước 1: Đăng nhập giao diện web iDRAC.
- Bước 2: Bật cửa sổ Console.
- Bước 3: Chọn Virtual Media → tìm file ISO cần mount.
- Bước 4: Click Map Device.
- Bước 5: Reset server, nhấn F11 để vào Boot Manager.
- Bước 6: Chọn "One-shot BIOS boot menu" → Virtual Optical Drive để bắt đầu cài đặt.
Phần 3 - Giám sát: sử dụng các tab Server (thông tin chung, Power/Thermal), Hardware (CPU, RAM, Fan), Storage (dung lượng lưu trữ).
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-idrac-de-cai-dat-he-dieu-hanh-cho-may-chu-dell/

**Hỏi: Cách cấu hình RAID cho Server Dell như thế nào?**
Trả lời: Để cấu hình RAID trên máy chủ Dell:
- Bước 1: Khởi động Server, sau khi server kiểm tra thông tin tài nguyên (CPU, RAM...), nhấn Ctrl + R để vào giao diện cấu hình RAID.
- Bước 2: Trong giao diện RAID, nhấn F2 để mở menu cấu hình, chọn mức RAID mong muốn (ví dụ RAID 5), tại mục "Physical Disks" nhấn Enter để chọn ổ cứng vật lý, nhập tên cho nhóm RAID tại mục "VD Name", nhấn OK để hoàn thành.
- Bước 3: Khởi động lại Server bằng Ctrl + Alt + Delete để hoàn tất quá trình cài đặt RAID.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-raid-cho-server-dell/

## VMware ESXi / vCenter

**Hỏi: Cách tạo VM (máy ảo) trên VMware ESXi như thế nào?**
Trả lời: Để tạo máy ảo trên VMware ESXi, lưu trữ file cài đặt hệ điều hành trên server thay vì dùng CD/DVD hay USB. Phần 1 - Upload file ISO:
- Bước 1: Từ giao diện quản lý ESXi, chọn Storage → datastore 1 → Datastore browser.
- Bước 2: Bấm "Create directory" và đặt tên cho thư mục lưu trữ.
- Bước 3: Chọn thư mục vừa tạo, bấm Upload và chọn file ISO hệ điều hành mong muốn.
- Bước 4: Chờ quá trình tải lên hoàn tất, file ISO sẽ được lưu trữ sẵn.
Phần 2 - Tạo máy ảo:
- Bước 1: Chọn Virtual Machine → Create / Register VM.
- Bước 2: Chọn "Create a new virtual machine" → Next.
- Bước 3: Đặt tên máy ảo, chọn hệ điều hành và phiên bản → Next.
- Bước 4: Chọn ổ đĩa lưu trữ → Next.
- Bước 5: Cấu hình phần cứng và chọn file cài đặt hệ điều hành → Next.
- Bước 6: Bấm Finish để hoàn tất.
- Bước 7: Khởi động máy ảo và cài đặt hệ điều hành.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-vm-tren-vmware-esxi/

**Hỏi: Cách fix lỗi vCenter không truy cập được do hết hạn SSL vCenter 7 như thế nào?**
Trả lời: Nguyên nhân: SSL certificate của vCenter 7 đã hết hạn, khiến dịch vụ không khởi động được, hiển thị lỗi liên quan đến vpxd-svcs services với thông báo "Service-control failed. Error: Failed to start services in profile ALL. RC=4, stderr=Failed to start vpxd-svcs services." Các bước xử lý:
- Bước 1: Kết nối SSH vào máy chủ vCenter.
- Bước 2: Tải file script Python (fixcerts_3_2.py) và upload lên vCenter.
- Bước 3: Thực thi lệnh `python fixcerts_3_2.py replace --certType all`.
- Bước 4: Xác nhận các tùy chọn bằng cách gõ "Yes" hoặc "Y" cho đến khi quá trình hoàn tất.
Script này sẽ tự động cấp mới các chứng chỉ SSL, cho phép dịch vụ vCenter khởi động lại bình thường. Tham khảo chính thức từ Broadcom Knowledge Base về thay thế chứng chỉ và khắc phục lỗi vpxd-svcs.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/fix-loi-vcenter-khong-truy-cap-duoc-do-het-han-ssl-vcenter-7/
