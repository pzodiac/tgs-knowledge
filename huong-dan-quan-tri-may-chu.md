# Hướng dẫn quản trị máy chủ (Firewall, PRTG, Database, SQL)

> **Danh mục:** Hướng dẫn kỹ thuật - Quản trị máy chủ
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** firewall, prtg, telegram, sql server, mysql, backup, restore, stocks5 proxy

## Bảo mật và giám sát máy chủ

**Hỏi: Cách cấu hình CSF (ConfigServer Firewall) chống DOS, Bruteforce cho Server như thế nào?**
Trả lời: CSF (ConfigServer Firewall) giúp bảo vệ server khỏi tấn công DOS, brute force và nhiều hình thức tấn công khác. Các lệnh CSF cơ bản: `csf -s` khởi động firewall, `csf -f` dừng firewall, `csf -r` tải lại cấu hình, `csf -a [IP]` thêm IP vào whitelist, `csf -d [IP]` chặn 1 IP. File cấu hình nằm tại `/etc/csf/csf.conf`. Các cấu hình bảo mật quan trọng:
- Giới hạn kết nối (chống DOS): ví dụ `CONNLIMIT = "80;20"` giới hạn port 80 tối đa 20 kết nối mỗi IP.
- Chống Port Flood: `PORTFLOOD = "80;tcp;100;5"` chặn IP tạo 100 kết nối TCP đến port 80 trong 5 giây.
- Chống SYN Flood: bật bằng `SYNFLOOD = "1"`, giới hạn tốc độ bằng `SYNFLOOD_RATE = "75/s"`.
- Quản lý cổng: mở cổng qua cấu hình `TCP_IN` và `TCP_OUT` — các cổng phổ biến: SSH (22), HTTP (80), HTTPS (443), FTP (21), DNS (53).
- Chặn theo quốc gia: `CC_DENY = "RU,CN"` chặn IP từ các quốc gia chỉ định.
- Chống đăng nhập sai nhiều lần: `LF_SSHD = "3"` chặn IP sau 3 lần đăng nhập SSH thất bại, có cấu hình tương tự cho FTP và các dịch vụ khác.
- Tích hợp danh sách spam: CSF tích hợp nhiều blocklist (Spamhaus, DShield, TOR exit nodes...) qua file `/etc/csf/csf.blocklists`.
Lưu ý: cần reload lại cấu hình bằng lệnh CSF phù hợp sau mỗi lần thay đổi. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cau-hinh-csf-cho-server-chong-doss-brutefore/

**Hỏi: Cách thiết lập Rule trên Firewall để chặn IP Remote Access vào Windows Server như thế nào?**
Trả lời: Nguyên tắc cốt lõi là "Default Deny" — chỉ tạo Firewall Rule Allow cho dịch vụ và nguồn truy cập thực sự cần thiết:
- Bước 1: Thiết lập chính sách mặc định chặn (Block) tất cả kết nối inbound bằng PowerShell.
- Bước 2: Tạo các Allow Rule riêng cho các subnet đáng tin cậy trên những port cần thiết.
- Bước 3: Kiểm tra lại các rule đã bật, port đã đúng cấu hình, và dịch vụ đang lắng nghe đúng.
- Bước 4: Kiểm thử — xác nhận IP được phép có thể kết nối, trong khi nguồn không được phép bị từ chối.
Khuyến nghị bổ sung: luôn duy trì quyền truy cập qua KVM hoặc console làm phương án dự phòng; dùng placeholder IP thay vì địa chỉ thật khi chia sẻ tài liệu; bật MFA cho tài khoản quản trị; theo dõi Windows Event Logs để phát hiện các lần đăng nhập thất bại. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cach-thiet-lap-rule-tren-firewall-de-chan-ip-remote-access-vao-windows-server/

**Hỏi: Cách giám sát máy chủ Linux từ xa bằng PRTG như thế nào?**
Trả lời: Để giám sát máy chủ Linux từ xa qua PRTG Network Monitor (CPU, RAM, disk, network):
- Bước 1: Tạo tài khoản riêng trên máy chủ Linux (không dùng root) bằng lệnh `useradd prtg` rồi `passwd prtg` để thiết lập mật khẩu.
- Bước 2: Trong giao diện PRTG, vào Devices, thêm thiết bị mới với hostname/IP của máy chủ, nhập username và password vừa tạo vào phần SSH credentials.
- Bước 3: Sau khi tạo thiết bị, chọn "Run Auto-Discovery" để PRTG tự động kết nối các sensor giám sát và bắt đầu thu thập chỉ số hệ thống.
Cách này tăng bảo mật vì tránh dùng quyền root, đồng thời hỗ trợ giám sát toàn diện từ xa qua SSH. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-giam-sat-may-chu-linux-bang-prtg/

**Hỏi: Cách cấu hình PRTG gửi thông báo qua Telegram như thế nào?**
Trả lời: Để tích hợp PRTG với Telegram nhận thông báo cảnh báo, thực hiện 2 phần:
Phần 1 - Tạo Bot Telegram:
- Tìm BotFather trên Telegram, nhập lệnh `/start`.
- Tạo bot mới với tên và username (username phải kết thúc bằng "bot").
- Tắt "Privacy mode" để bot hoạt động trong nhóm.
- Thêm bot vào nhóm Telegram của bạn và cấp quyền quản trị viên cho bot.
Phần 2 - Cấu hình PRTG:
- Truy cập API Telegram để lấy Chat ID (Chat ID nhóm thường có dấu "-" ở đầu).
- Tạo Notification Template mới trong PRTG, bật tính năng "Execute HTTP Action".
- Nhập URL API và phương thức POST với payload chứa Chat ID.
- Kiểm tra bằng nút chuông để xác nhận hoạt động, sau đó gán template vào notification triggers của thiết bị. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-prtg-voi-telegram/

## Database và SQL Server

**Hỏi: Cách enable Remote SQL Server để truy cập từ xa như thế nào?**
Trả lời: Nếu gặp lỗi khi kết nối từ xa vào MSSQL Server, cần cấu hình cho phép truy cập từ xa. Trước tiên tạm tắt firewall hoặc mở port mặc định 1433 của SQL, sau đó:
- Bước 1: Mở SQL Server Management Studio, kết nối đến SQL Server, vào Properties.
- Bước 2: Vào tab Connections, bật "Allow remote connection to this server".
- Bước 3: Vào SQL Server Configuration Manager, tìm Protocols cho MSSQL, xác nhận TCP/IP đang bật (nếu đang tắt thì bật lên).
- Bước 4: Cấu hình địa chỉ IP và port — cập nhật IP Address khớp với IP hiện tại của máy chủ, đặt TCP Port là 1433. Nếu TCP Dynamic Ports có giá trị mặc định 0, xóa trống trường này. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-enable-remote-sql-server-tu-xa-vao/

**Hỏi: Cách backup và restore database MySQL bằng dòng lệnh như thế nào?**
Trả lời: Để sao lưu và phục hồi dữ liệu MySQL qua command line:
- Sao lưu database: dùng lệnh `mysqldump –opt -u [uname] -p [dbname] > [backupfile.sql]`. Ví dụ: `mysqldump -u root -p kinhdoanh > backup_kd.sql`.
- Sao lưu bảng cụ thể: liệt kê tên các bảng cách nhau bằng dấu cách trong lệnh để sao lưu riêng biệt.
- Nén tệp sao lưu (với database lớn): `mysqldump -u [uname] -p [dbname] | gzip -9 > [backupfile.sql.gz]`.
- Phục hồi database: dùng lệnh `mysql -u [uname] -p [dbname] < [backupfile.sql]`. Ví dụ: `mysql -u root -p kinhdoanh < backup_kd.sql`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-backup-va-restore-database/

**Hỏi: Cách xử lý lỗi Row size too large (8126) khi import database MySQL như thế nào?**
Trả lời: Lỗi này xuất hiện khi InnoDB không thể lưu trữ các hàng dữ liệu do quá lớn — InnoDB có kích thước hàng tối đa gần bằng một nửa giá trị biến hệ thống `innodb_page_size`. Cách xử lý:
- Bước 1: Truy cập SSH vào server.
- Bước 2: Mở file cấu hình `/etc/my.cnf`, thêm dòng `innodb_strict_mode = 0` để vô hiệu hóa strict mode.
- Bước 3: Restart dịch vụ MySQL và thử import lại database.
- Bước 4 (nếu vẫn lỗi): thêm `innodb_log_file_size = 512M`, sau đó restart MySQL và thử lại. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-xu-ly-loi-row-size-too-large-8126-khi-import-database/

**Hỏi: Cách cài đặt Prometheus và Grafana trên Linux để giám sát máy chủ như thế nào?**
Trả lời: Áp dụng cho Ubuntu 20.04+ hoặc RockyLinux 8/9, cần quyền root/sudo và kết nối Internet. Cài đặt Prometheus: tạo user riêng cho Prometheus, tải phiên bản v2.54.1, cấu hình thư mục và tạo file systemd service; sau khi khởi động, Prometheus chạy trên port 9090. Cài đặt Grafana: thêm repository (dùng apt với Ubuntu hoặc yum với RockyLinux), cài đặt gói grafana, bật systemd service; Grafana chạy trên port 3000 với tài khoản đăng nhập mặc định admin/admin. Kết nối Grafana với Prometheus: vào Configuration → Data Sources, thêm Prometheus làm nguồn dữ liệu với URL `http://localhost:9090`, kiểm tra kết nối. Nên import sẵn các dashboard có sẵn từ grafana.com (như Node Exporter Full hoặc Docker Monitoring) bằng Dashboard ID hoặc file JSON. Có thể cài thêm Node Exporter để giám sát hệ thống Linux hoặc Windows Exporter cho môi trường Windows. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-prometheus-va-grafana-tren-linux/

**Hỏi: Cách cấu hình SSL VPN Host to LAN như thế nào?**
Trả lời: Để thiết lập kết nối VPN từ máy tính vào mạng nội bộ qua thiết bị Vigor2912 (DrayTek):
- Bước 1: Tải và cài đặt phần mềm DrayTek Smart VPN Client từ trang chủ DrayTek (có bản cho các hệ điều hành khác).
- Bước 2: Mở phần mềm, chọn "Add" để tạo Profile mới với thông số: Tên Profile (tùy chọn), Loại kết nối chọn "SSL VPN Tunnel", IP/Host điền IP WAN hoặc tên miền, Port nhập port SSL VPN (ví dụ 443), thông tin đăng nhập dùng username/password được cấp, và bật "Fast SSL" trong Advanced Options.
- Bước 3: Chọn profile "SSL VPN" vừa tạo và nhấn "Connect" — trạng thái hiển thị "Connected" khi kết nối thành công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-ssl-vpn-host-to-lan/

**Hỏi: Cách xử lý các lỗi phổ biến trong MySQL như thế nào?**
Trả lời: Tổng hợp cách xử lý 7 lỗi MySQL thường gặp:
- "mysqld dead but subsys locked" (MySQL không khởi động lại được, tập tin bị khóa): sao lưu tập tin khóa, xóa tiến trình bị khóa, tắt các dịch vụ liên quan rồi khởi động lại MySQL.
- "ERROR 2006 - MySQL server has gone away" (database quá lớn vượt giới hạn cấu hình): sửa file `my.cnf`, thêm `max_allowed_packet=500M`, khởi động lại dịch vụ.
- "InnoDB log file is of different size" (xảy ra sau khi chuyển đổi database InnoDB): phục hồi cấu hình, thực thi lệnh tắt nhanh, xóa tập tin log cũ, điều chỉnh kích thước log trong `my.cnf`.
- "MySQL running but PID file not found": tạo thư mục `/var/run/mysql`, khởi tạo file `mysqld.pid`, cấp quyền cho user MySQL.
- "Too many connections" (số kết nối vượt giới hạn máy chủ): tăng `max_connections` và `max_user_connections` trong `my.cnf`, khởi động lại MySQL.
- "Can't connect through socket" (không tìm thấy file `mysql.sock`): tìm vị trí file, cập nhật đường dẫn trong `my.cnf`, tạo liên kết tượng trưng (symlink) nếu cần.
- "Got error 28 from table handler" (ổ cứng đã đầy): dừng MySQL, giải phóng dung lượng bằng cách xóa file tạm/log cũ, khởi động lại dịch vụ. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/xu-ly-cac-loi-pho-bien-trong-mysql/

## Khác

**Hỏi: Cách thiết lập proxy Stocks5 (SOCKS5) trên Telegram như thế nào?**
Trả lời: Sau khi có thông tin từ nhà cung cấp proxy (địa chỉ IP, port, thông tin xác thực):
- Trên di động: mở menu (icon 3 gạch) → Settings → Data and Storage → cuộn xuống chọn "Proxy Settings" → bật công tắc proxy → nhập thông tin proxy và xác nhận bằng dấu tick.
- Trên desktop: mở Settings → chọn "Advanced" → "Connection type" → chọn "Use Custom Proxy" → nhập hostname/IP máy chủ và port → thêm thông tin đăng nhập nếu được yêu cầu → nhấn Save.
Với proxy SOCKS5 hoặc HTTP, cần cung cấp: hostname/IP máy chủ, port tương ứng, và thông tin đăng nhập (nếu quản trị viên yêu cầu). Để tắt proxy: chuyển "Use Proxy" về off trên di động hoặc chọn "Disable Proxy" trên desktop. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-thiet-lap-stocks5-tren-telegram/
