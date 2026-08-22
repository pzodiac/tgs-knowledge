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

**Hỏi: Cách cài đặt VMware Workstation và tạo máy ảo như thế nào?**
Trả lời: VMware Workstation là phần mềm ảo hóa desktop, cho phép chạy thêm hệ điều hành khác hoặc cài phần mềm không tương thích với hệ điều hành chính. Các bước:
- Cài đặt: tải và chạy trình cài đặt VMware, đồng ý điều khoản, bấm tiếp tục qua các bước cho đến khi thấy nút "Install", bấm "Finish" để hoàn tất.
- Tạo máy ảo: mở VMware, chọn "Create a New Virtual Machine" → chọn chế độ "Typical" → duyệt và chọn file hệ điều hành (ví dụ Windows 7, 8, 10) → đặt tên máy ảo → chọn vị trí lưu trữ (khác với vị trí cài VMware) → tùy chỉnh cấu hình phần cứng nếu cần → bật máy ảo để bắt đầu cài đặt hệ điều hành. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-may-ao-bang-vmware-va-tao-may-ao/

**Hỏi: Cách setup NFS Server và Client trên CentOS 7.x để chia sẻ dữ liệu như thế nào?**
Trả lời: Ví dụ với Server (server.tgs.com.vn, IP 192.168.1.100) và Client (client.tgs.com.vn, IP 192.168.1.101), các bước thực hiện:
- Bước 1: Cấu hình Firewall — cài đặt firewalld, khởi động dịch vụ, cho phép SSH và NFS đi qua tường lửa bằng lệnh firewall-cmd.
- Bước 2: Cài đặt NFS — trên server cài nfs-utils và khởi động nfs-server.service; trên client chỉ cần cài gói nfs-utils.
- Bước 3: Tạo thư mục chia sẻ — tạo "/var/nfs", điều chỉnh quyền sở hữu cho nfsnobody, chỉnh sửa file /etc/exports để khai báo các phân vùng cho phép truy cập từ client.
- Bước 4: Mount trên Client — tạo thư mục đích và mount các chia sẻ NFS bằng lệnh mount với địa chỉ server.
- Bước 5: Kiểm tra — tạo file kiểm tra trên client, xác minh file xuất hiện trên server với quyền sở hữu phù hợp.
- Bước 6: Mount tự động — thêm mục vào /etc/fstab để NFS tự động mount sau khi khởi động lại hệ thống. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/setup-nfs-server-va-client-tren-centos-7-x-de-chia-se-du-lieu/

**Hỏi: Cách cập nhật bản vá lỗ hổng CVE-2019-0708 trên Windows Server như thế nào?**
Trả lời: Lỗ hổng CVE-2019-0708 cho phép thực thi lệnh từ xa trên giao thức Remote Desktop Services của Windows — kẻ tấn công có thể khai thác mà không cần thông tin đăng nhập hoặc sự tương tác của người dùng. Cách vá theo từng phiên bản:
- Windows Server 2012 R2 và 2016: tải bản vá từ portal.msrc.microsoft.com, cài đặt file .msu tương ứng.
- Windows 7 và Windows Server 2008/2008 R2: truy cập Microsoft Update Catalog để tải bản fix KB4499175.
- Windows XP và Server 2003: tham khảo hướng dẫn tại support.microsoft.com.
Biện pháp khắc phục tạm thời trong khi chờ cập nhật: kích hoạt Network Level Authentication (NLA), hoặc chặn TCP port 3389/hạn chế quyền truy cập theo địa chỉ IP. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cap-nhat-ban-va-lo-hong-ma-cve-2019-0708-tren-may-chu-windows-recommended-to-update-the-code-vulnerability-patch-cve-2019-0708-on-windows-server/

**Hỏi: RAM ECC, RAM ECC REG và RAM FB-DIMM là gì?**
Trả lời: Phân biệt 3 loại RAM dùng cho máy chủ:
- RAM ECC (Error Checking and Correction): có khả năng điều khiển dòng dữ liệu ra/vào; khi xảy ra lỗi truyền tín hiệu tốc độ cao, chỉ cần gửi lại gói tin bị lỗi thay vì toàn bộ dòng dữ liệu. Tất cả RAM dành cho máy chủ đều được yêu cầu có ECC để đảm bảo độ ổn định cao.
- RAM ECC REG (Registered Memory): là loại SDRAM có các thanh ghi (register) gắn trực tiếp trên module nhớ, tái định hướng tín hiệu qua các chip nhớ và cho phép module chứa nhiều chip hơn. Lưu ý: registered memory và unbuffered memory không tương thích với nhau trong cùng một hệ thống.
- RAM FB-DIMM (Fully Buffered DIMM): công nghệ RAM máy chủ mới hơn, dùng giao tiếp SERIAL thay vì giao tiếp song song như DIMM thông thường, cho phép tạo nhiều kênh truyền tín hiệu và tăng đáng kể tốc độ truyền tải dữ liệu, được thiết kế để tăng tốc độ và độ ổn định. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/ram-ecc-la-gi-ram-ecc-reg-la-gi-ram-fbdimm-la-gi/
