# Hướng dẫn quản trị Windows Server

> **Danh mục:** Hướng dẫn kỹ thuật - Windows Server
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** windows server, prtg, remote desktop, rdp, icmp, ping, reset password, network cable, wordpress backup

## Giám sát và cập nhật

**Hỏi: Cách giám sát máy chủ Windows bằng PRTG như thế nào?**
Trả lời: Để thiết lập PRTG Network Monitor theo dõi CPU, RAM, ổ đĩa và lưu lượng mạng trên Windows Server:
- Chuẩn bị: tải và cài đặt PRTG Network Monitor, cấu hình giao thức SNMP trên máy chủ Windows.
- Cài đặt SNMP trên Windows: mở Server Manager → Add Roles and Features → chọn tính năng SNMP Service và SNMP WMI Provider → cài đặt dịch vụ SNMP.
- Cấu hình SNMP Service: truy cập `services.msc`, vào SNMP Service Properties → tab Agent nhập Contact, Location → tab Security thêm Community String (chuỗi xác thực), cho phép lấy thông tin từ bất kỳ host nào.
- Cấu hình PRTG: thêm Group và Device (nhập tên thiết bị, IP máy chủ Windows), khai báo Community String đã tạo, thêm các Sensor: SNMP CPU Load, SNMP Memory, SNMP Disk Free.
- Giám sát: hệ thống sẽ hiển thị dữ liệu chi tiết về hiệu suất máy chủ trong giao diện PRTG.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-giam-sat-may-chu-windows-bang-prtg/

**Hỏi: Cách update thủ công cho Windows Server 2012, 2016, 2019 như thế nào?**
Trả lời: Cập nhật Windows Server giúp tăng cường bảo mật, cải thiện hiệu suất, thêm tính năng mới và tuân thủ các quy định:
- Bước 1: Tải gói cập nhật từ Microsoft Catalog — Windows 2016: KB5003638; Windows 2012 R2: KB5003681; Windows 2019: KB5003646.
- Bước 2: Mở tập tin cập nhật vừa tải.
- Bước 3: Chọn "Yes" để bắt đầu quá trình cài đặt.
- Bước 4: Khởi động lại máy khi được yêu cầu để hoàn tất cập nhật.
- Bước 5: Xác minh thành công bằng cách kiểm tra Windows Update để đảm bảo không còn bản cập nhật nào chưa cài đặt.
Lưu ý quan trọng: sao lưu dữ liệu trước khi cập nhật, đọc kỹ thông tin bản cập nhật, liên hệ hỗ trợ kỹ thuật nếu gặp sự cố, và tuân thủ chính sách của tổ chức.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/update-thu-cong-cho-windows-server/

## Remote Desktop và mạng

**Hỏi: Cách thay đổi port Remote Desktop trên Windows Server như thế nào?**
Trả lời: Để tăng bảo mật cho dịch vụ Remote Desktop (mặc định dùng giao thức RDP port TCP 3389):
- Bước 1: Nhấn Windows + R để mở Run, gõ `regedit.exe` và Enter để mở Registry Editor.
- Bước 2: Điều hướng đến đường dẫn `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber`.
- Bước 3: Chọn Registry tên "PortNumber", nhấp chuột phải và chọn "Modify".
- Bước 4: Tại cửa sổ "Edit DWORD", chọn "Decimal", nhập port mới (ví dụ 6969), nhấn OK.
- Bước 5: Kết nối từ xa dùng định dạng `<hostname>:<port>` hoặc `<địa chỉ IP>:<port>`.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-thay-doi-port-remote-desktop-tren-windows-server/

**Hỏi: Cách chặn truy cập từ nước ngoài vào Remote Desktop Windows 2016 như thế nào?**
Trả lời: Để bảo vệ máy chủ Windows 2016 bằng cách giới hạn truy cập Remote Desktop chỉ từ các địa chỉ IP được phép, ngăn chặn tấn công từ xa và xâm nhập trái phép. Lý do cần thiết: bảo mật hệ thống (ngăn tin tặc cài malware hoặc truy cập dữ liệu nhạy cảm), chống tấn công Brute Force, giảm lưu lượng không mong muốn, tăng khả năng giám sát. Các bước thực hiện:
- Bước 1: Mở Control Panel → Windows Firewall → Advanced Settings → Inbound Rules.
- Bước 2: Chọn "New rule" → "Custom" → Next.
- Bước 3: Chọn giao thức TCP, cổng 3389 (cổng Remote Desktop mặc định).
- Bước 4: Tại mục "Scope", chọn "These IP address" và nhập dải IP Việt Nam được phép.
- Bước 5: Chọn "Allow the connection".
- Bước 6: Áp dụng cho tất cả profile (Domain, Private, Public).
- Bước 7: Đặt tên mô tả cho quy tắc và nhấn "Finish".
Lưu ý: xác định rõ các địa chỉ IP/dải IP cần cho phép, cân nhắc dùng xác thực đa yếu tố (MFA), ghi nhật ký tất cả truy cập thành công/thất bại, và nên dùng kết nối an toàn như VPN hoặc RDP Gateway.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/chan-truy-cap-tu-nuoc-ngoai-vao-remote-desktop/

**Hỏi: Cách mở giao thức ICMP cho phép Ping trên Windows Server như thế nào?**
Trả lời: Windows mặc định chặn giao thức ICMP, cần cấu hình tường lửa để cho phép máy tính bên ngoài ping đến Windows Server. Phương pháp 1 - tạo rule tùy chỉnh:
- Bước 1: Mở Control Panel → Windows Firewall → Advanced Settings → chọn Inbound Rules.
- Bước 2: Trong Actions, chọn "New Rule" → chọn "Custom" → Next.
- Bước 3: Để Program mặc định là "All program" → Next.
- Bước 4: Ở mục Protocol, chọn "ICMPv4" (IPv4) hoặc "ICMPv6" (IPv6) → Next.
- Bước 5: Các mục IP Address để mặc định (chấp nhận mọi IP) → Next.
- Bước 6: Chọn "Allow the connection" → Next.
- Bước 7: Chọn áp dụng cho tất cả môi trường mạng → Next.
- Bước 8: Đặt tên và chú thích cho rule → Finish.
Phương pháp 2 - kích hoạt rule có sẵn: tìm và enable rule có tên "File and Printer Sharing (Echo Request – ICMPv4-In)" trong danh sách Inbound Rules.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/mo-giao-thuc-icmp-cho-phep-ping-tren-windows-server/

**Hỏi: Lỗi "A network cable is not properly plugged in or may be broken" — nguyên nhân và cách xử lý nhanh?**
Trả lời: Lỗi này xảy ra do 3 nguyên nhân chính: cáp mạng cắm sai vị trí/không đúng cách hoặc cáp bị đứt; card mạng (network adapter) bị lỗi; driver bị thiếu hoặc hỏng. 5 cách khắc phục:
- Cách 1 - Khởi động lại máy tính: tắt hoàn toàn thiết bị, chờ vài giây rồi khởi động lại; với laptop, nên tháo pin khoảng 10 phút trước khi lắp lại.
- Cách 2 - Tắt Ethernet khi không dùng: vào Network and Sharing Center → Change adapter settings → chuột phải vào cổng Ethernet → chọn Disable (thao tác hơi khác nhau giữa Windows 7 và Windows 10).
- Cách 3 - Kiểm tra kết nối cáp: xác nhận cả 2 đầu cáp Ethernet đã cắm chắc chắn (một đầu vào máy tính, đầu kia vào router), kiểm tra kết nối lỏng lẻo và thay cáp bị hỏng nếu cần.
- Cách 4 - Cập nhật driver mạng: mở Device Manager → mở rộng Network adapters → chuột phải vào driver có dấu cảnh báo → chọn Update Driver Software.
- Cách 5 - Cài lại card mạng: với máy tính cũ có card Ethernet có thể tháo rời (USB, PCMCIA hoặc PCI), tháo và lắp lại phần cứng để kiểm tra khôi phục kết nối.
Nếu vẫn còn lỗi sau khi thử các cách trên, nên liên hệ dịch vụ sửa chữa chuyên nghiệp.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/a-network-cable-is-not-properly-plugged-in-or-may-be-broken-nguyen-nhan-va-cach-xu-ly-loi-nhanh/

## Khôi phục mật khẩu và dữ liệu

**Hỏi: Cách reset password Windows Server 2008 R2 khi quên mật khẩu admin như thế nào?**
Trả lời: Đây là quy trình 2 bước giúp quản trị viên khôi phục quyền truy cập khi quên mật khẩu tài khoản admin trên Windows Server 2008 R2, mất khoảng 15 phút. Bước 1 - Thay thế Utilman.exe bằng CMD.exe: đổi tên Utilman.exe gốc thành Utilman.exe.bak, sau đó copy file CMD.EXE vào thư mục `C:\WINDOWS\SYSTEM32\` và đổi tên thành UTILMAN.EXE. Cách A - Dùng đĩa cài đặt Windows Server: khởi động từ đĩa CD/DVD Windows Server 2008, chọn chế độ Repair và mở Command Prompt, thực hiện lần lượt các lệnh để di chuyển và sao chép file. Cách B - Dùng đĩa boot: truy cập môi trường Windows XP mini qua đĩa/USB boot, điều hướng đến `C:\WINDOWS\SYSTEM32\`, đổi tên Utilman.exe thành Utilman.exe.bak, copy CMD.EXE và đổi tên thành Utilman.exe, khởi động lại máy tính. Bước 2 - Reset mật khẩu qua dòng lệnh: khởi động máy bình thường, chờ đến màn hình Welcome, nhấn biểu tượng accessibility (góc dưới trái) hoặc phím Windows + U, khi cửa sổ command prompt hiện ra, nhập lệnh `net user administrator new-password` (thay "administrator" bằng tên tài khoản và "new-password" bằng mật khẩu mong muốn). Mật khẩu có hiệu lực ngay lập tức, cho phép đăng nhập với thông tin mới.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/reset-password-windows-server/

**Hỏi: Cách sao lưu, khôi phục dữ liệu trên WordPress như thế nào?**
Trả lời: Sử dụng plugin "All-In-One WP Migration" để backup và restore dữ liệu website WordPress, phục vụ bảo vệ dữ liệu và di chuyển giữa các môi trường. Quá trình backup:
- Bước 1: Cài đặt và kích hoạt plugin "All-In-One WP Migration" trong WordPress.
- Bước 2: Truy cập menu Export tại "All-In-One WP Migration → Export".
- Bước 3: Chọn định dạng export bằng cách nhấn "Export To → File".
- Bước 4: Để plugin hoàn tất quá trình sao lưu, không đóng trình duyệt trong lúc xử lý.
- Bước 5: Sau khi hoàn tất, tải file backup về và lưu trữ cẩn thận.
Quá trình restore:
- Bước 1: Truy cập menu Import tại "All-In-One WP Migration → Import".
- Bước 2: Chọn "Import From → File" và chọn file backup đã lưu.
- Bước 3: Plugin sẽ tải file backup lên hosting.
- Bước 4: Nhấn "Continue" khi được yêu cầu xác nhận.
- Bước 5: Chờ quá trình khôi phục hoàn tất, không gián đoạn trình duyệt.
Lưu ý: không đóng trình duyệt trong lúc backup hoặc restore; đảm bảo file backup được lưu ở nơi dễ truy cập; cách này áp dụng được cho cả môi trường phát triển local lẫn hosting production.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-sao-luu-khoi-phuc-du-lieu-tren-wordpress/

**Hỏi: Cách khắc phục lỗi "CredSSP Encryption Oracle Remediation" khi Remote Desktop như thế nào?**
Trả lời: Lỗi này xuất hiện khi kết nối Remote Desktop Protocol (RDP) giữa máy client và máy chủ Windows bị chặn do cài đặt chính sách bảo mật "Encryption Oracle Remediation" của Microsoft, thường gặp sau các bản cập nhật Windows từ tháng 3/2018. Nguyên nhân: hệ thống kiểm tra phiên RDP có an toàn hay không qua giao thức CredSSP, và chặn kết nối RDP không an toàn. Cách khắc phục:
- Cách 1 (khuyến nghị) - qua Group Policy: nhấn Windows + R, gõ `gpedit.msc` rồi Enter (lưu ý Windows Home không hỗ trợ sẵn công cụ này) → đi đến `Computer Configuration > Administrative Templates > System > Credentials Delegation > Encryption Oracle Remediation` → thay đổi Policy Setting để cho phép kết nối RDP an toàn hơn.
- Cách 2 - với Windows Home: tải file hỗ trợ, giải nén và chạy với quyền Administrator để kích hoạt Group Policy.
Phương pháp qua Group Policy là cách tiếp cận chính thức và an toàn nhất để giải quyết vấn đề mà không ảnh hưởng đến bảo mật hệ thống. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-khac-phuc-loi-credssp-encryption-oracle-remediation-khi-remote-desktop/
