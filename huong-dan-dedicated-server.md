# Hướng dẫn quản trị Dedicated Server (Firewall, Virtualization, Giám sát)

> **Danh mục:** Hướng dẫn kỹ thuật - Dedicated Server
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** vmware esxi, pfsense, cve-2022-26809, iperf, iptraf, apache, semaphore

## Firewall và virtualization

**Hỏi: Cách cấu hình và quản lý Firewall trên VMware ESXi như thế nào?**
Trả lời: Có 3 cách quản lý firewall trên host VMware ESXi:
- Cách 1 - vSphere Client (GUI): truy cập Security Profile qua tab Configuration, có thể xem dịch vụ đang bật/tắt, đặt chính sách khởi động, giới hạn truy cập từ xa theo dải IP (ví dụ chỉ cho phép SSH từ dải 192.168.10.0/24 của các máy quản trị).
- Cách 2 - Dòng lệnh ESXCLI: SSH vào host và dùng lệnh `esxcli network firewall` để liệt kê ruleset đang bật/tắt, xem rule theo từng dịch vụ, bật dịch vụ và giới hạn IP truy cập, xem trạng thái firewall, bật/tắt toàn bộ firewall.
- Cách 3 - PowerCLI (PowerShell): dùng cmdlet PowerShell hoặc `Get-EsxCli` để thực hiện các thao tác tương tự ESXCLI theo hướng tự động hóa.
Lưu ý quan trọng: không có cách đơn giản để thêm ruleset tùy chỉnh ngoài việc tạo và import qua đóng gói VIB, đòi hỏi kỹ năng kỹ thuật và thời gian.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-va-quan-ly-firewall-tren-vmware-esxi/

**Hỏi: Cách cài đặt và cấu hình cơ bản pfSense như thế nào?**
Trả lời: pfSense là nền tảng firewall mã nguồn mở xây dựng trên FreeBSD, dùng để tạo firewall và router chuyên dụng, được nhiều quản trị viên hệ thống tin dùng nhờ độ tin cậy và tính năng tương đương các giải pháp thương mại như Cisco hay Fortigate. Ưu điểm chính: tiết kiệm chi phí (miễn phí, thay thế cho firewall doanh nghiệp đắt tiền), nhẹ (chỉ cần CPU 1 GHz, RAM 1GB, ổ cứng 1GB, 2 card mạng), linh hoạt (hỗ trợ mở rộng từ bên thứ ba, cài được trên máy tính thông thường). Các bước cài đặt: tải file ISO, chọn tùy chọn cài đặt, phân vùng ổ cứng, cấu hình interface WAN/LAN, thiết lập DHCP server, tạo firewall rule. Sau khi cài đặt, quản trị viên có thể: chặn port và IP cụ thể, tạo rule tùy chỉnh với hành động Pass/Block/Reject, đặt rule theo giao thức (TCP, UDP, ICMP...), định nghĩa nguồn/đích, bật ghi log để giám sát. Thông tin đăng nhập mặc định: Username "Admin" / Password "pfsense".
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/3383-2/

**Hỏi: Cách tạo VM (máy ảo) trên VMware ESXi như thế nào?**
Trả lời: Các bước tạo máy ảo (VM) trên VMware ESXi:
- Bước 1 - Upload file ISO hệ điều hành: vào Storage → Datastore 1 → Datastore browser, tạo thư mục mới, upload file ISO của hệ điều hành cần cài vào thư mục đó (nên lưu sẵn trên ESXi để tiết kiệm thời gian cài đặt).
- Bước 2 - Tạo máy ảo: chọn Virtual Machine → Create/Register VM → chọn Create a new virtual machine → đặt tên cho máy ảo và chọn hệ điều hành phù hợp → chọn ổ đĩa lưu trữ → cấu hình phần cứng và chọn file ISO cài đặt → nhấn Finish để hoàn tất.
- Bước 3 - Cài đặt hệ điều hành: khởi động máy ảo vừa tạo và tiến hành cài đặt hệ điều hành như bình thường.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-vm-tren-vmware-esxi/

## Bảo mật và cảnh báo lỗ hổng

**Hỏi: Lỗ hổng CVE-2022-26809 ảnh hưởng đến Windows như thế nào và cách khắc phục?**
Trả lời: CVE-2022-26809 là lỗ hổng thực thi mã từ xa (RCE) công bố ngày 12/04/2022, ảnh hưởng đến toàn bộ hệ điều hành Windows, cho phép hacker tấn công trực tiếp vào VPS/Server chạy Windows mà không cần tài khoản. Lỗ hổng ảnh hưởng đến mọi phần mềm/ứng dụng cài trên máy Windows bị ảnh hưởng, bao gồm cả phần mềm kế toán MISA được nhiều doanh nghiệp Việt Nam sử dụng. Mức độ: được xếp loại Critical (nghiêm trọng), nằm trong bản cập nhật Patch Tuesday tháng 4 của Microsoft vá hơn 100 lỗ hổng, thuộc nhóm 10 lỗ hổng mức "critical" trong đợt đó. Các biện pháp khắc phục:
- Bảo vệ dữ liệu: sao lưu hàng ngày, lưu trên macOS, Linux hoặc dịch vụ đám mây (Google Drive, OneDrive, DropBox).
- Thận trọng khi cập nhật: tránh cập nhật phần mềm MISA trong giai đoạn còn lỗ hổng.
- Bảo mật mạng: hạn chế mở port 445 ra ngoài, giới hạn kết nối từ xa khi cần thiết.
- Diệt virus: cài đặt và duy trì phần mềm bảo mật cập nhật (Kaspersky, Norton, Windows Defender, BKAV).
- Vá hệ điều hành: cài Windows bản quyền với bản vá bảo mật mới nhất ngay lập tức.
- SQL Server: đổi port mặc định 1433 nếu bật truy cập từ xa, tăng cường thông tin đăng nhập.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/lo-hong-cve-2022-26809-anh-huong-den-toan-bo-he-dieu-hanh-windows/

## Quản trị từ xa qua KVM IP

**Hỏi: KVM IP là gì và cách sử dụng như thế nào?**
Trả lời: IPKVM (hay KVM over IP) là thiết bị hỗ trợ kết nối từ xa vào server thông qua đường truyền mạng riêng, cho phép quản trị viên truy cập máy chủ từ xa mà không cần tới trực tiếp địa điểm đặt server. Lợi ích chính: xử lý sự cố từ xa khi không thể dùng remote desktop hay SSH, quản lý nhiều server tại các trung tâm dữ liệu khác nhau đồng thời, tiết kiệm chi phí và loại bỏ hạn chế về khoảng cách địa lý, cấu hình BIOS/cài đặt hệ điều hành/truy cập server ngay cả khi server bị treo. Các bước sử dụng:
- Bước 1: Cài đặt phiên bản Java mới nhất trên máy tính.
- Bước 2: Cấu hình Security Java, thêm địa chỉ IP của IPKVM vào danh sách được phép.
- Bước 3: Truy cập bằng trình duyệt (Firefox, Chrome, Safari) qua địa chỉ IP được cung cấp.
- Bước 4: Đăng nhập bằng username và password.
- Bước 5: Bấm nút kết nối hoặc tải file spider.jnlp (tùy loại KVM — phổ biến là Lantronix và Raritan, mỗi loại có giao diện quản lý khác nhau).
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-kvm-ip/

**Hỏi: Cách dùng KVM để kiểm tra màn hình server như thế nào?**
Trả lời: Các bước dùng KVM kiểm tra màn hình server:
- Bước 1: Cài đặt Java phiên bản mới nhất từ java.com/en/download/.
- Bước 2: Truy cập địa chỉ KVM được cung cấp (ví dụ https://103.74.123.254/) bằng username và password đã nhận.
- Bước 3: Nhấn nút connect để tải tệp Java, sau đó chạy tệp này trên máy tính.
- Bước 4: Sau khi chạy tệp Java, màn hình server xuất hiện để thao tác như bình thường.
Mục đích sử dụng: cài đặt hệ điều hành, cấu hình BIOS, truy cập server khi mất kết nối internet hoặc bị treo do quá tải tài nguyên.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-kvm-kiem-tra-man-hinh-server/

## Giám sát và xử lý sự cố

**Hỏi: Cách giám sát băng thông mạng trên CentOS bằng iperf như thế nào?**
Trả lời: Iperf là công cụ miễn phí dùng để đo lường lượng dữ liệu mạng (throughput) tối đa mà một server có thể xử lý, giúp phát hiện vấn đề hệ thống mạng bằng cách xác định máy chủ không đáp ứng lưu lượng dữ liệu mong đợi. Cài đặt trên Debian/Ubuntu: `apt-get install iperf`. Cài đặt trên CentOS/Fedora: `yum install epel-release -y` sau đó `yum install iperf -y`. Các tùy chọn chính: `-c` kết nối đến máy chủ theo địa chỉ IP; `-p` định cổng (mặc định 5001); `-t` thời lượng kết nối (giây); `-u` sử dụng giao thức UDP; `-P` số kết nối song song. Công cụ này hữu ích để đo lường throughput giữa hai máy chủ có sự khác biệt về vị trí địa lý.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-iptraf-tren-centos-de-giam-sat-bang-thong/

**Hỏi: Cách xử lý lỗi Apache "No Space Left On Device" khi start như thế nào?**
Trả lời: Lỗi này xảy ra khi máy chủ hết semaphores, thể hiện qua thông báo "AH02478: failed to create proxy mutex" — hệ thống không thể tạo semaphore mới cho Apache. Các bước xử lý:
- Bước 1: Kiểm tra lỗi hiện tại bằng lệnh `tail /var/log/httpd/error_log`.
- Bước 2: Xác định số lượng semaphores đang dùng bằng lệnh `ipcs -s`.
- Bước 3: Xóa các semaphores — Phương pháp 1: `for whatever in \`ipcs -s | awk '{print $2}'\`; do ipcrm -s $whatever; done`; Phương pháp 2: dừng Apache bằng `/etc/init.d/httpd stop`, chạy `ipcs -s | grep nobody | gawk '{ print $2 }' | xargs -n 1 ipcrm sem`, rồi khởi động lại bằng `/etc/init.d/httpd start`.
- Bước 4: Tăng giới hạn semaphore vĩnh viễn bằng cách thêm vào file `/etc/sysctl.conf` hai dòng `kernel.msgmni = 512` và `kernel.sem = 250 128000 32 512`, sau đó áp dụng cấu hình bằng lệnh `sysctl -p`.
Các bước này giải quyết vấn đề hết tài nguyên semaphore, cho phép Apache khởi động lại thành công.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-xu-ly-loi-khi-start-apache-no-space-left-on-device/
