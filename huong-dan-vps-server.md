# Hướng dẫn quản trị VPS/Server

> **Danh mục:** Hướng dẫn kỹ thuật - Máy chủ ảo VPS/Server
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** vps, server, kvm, rdcman, fio, ioping, speedtest, windows firewall, ransomware

## Khái niệm

**Hỏi: Máy chủ ảo VPS (Virtual Private Server) là gì?**
Trả lời: VPS là phương pháp phân chia một máy chủ vật lý thành nhiều máy chủ ảo. Khác với shared hosting có thể chứa hàng trăm tài khoản, một máy chủ VPS chỉ có khoảng 1/10 số lượng đó, mang lại hiệu năng cao hơn đáng kể. Mỗi VPS hoạt động như một hệ thống độc lập với CPU, RAM, dung lượng ổ cứng riêng biệt, hệ điều hành riêng và quyền quản trị root, địa chỉ IP riêng, có thể khởi động lại bất cứ lúc nào. Ưu điểm nổi bật: bảo mật cao (hạn chế 100% khả năng bị tấn công hack local, nếu một VPS bị tấn công DDoS các VPS khác không bị ảnh hưởng), tiết kiệm chi phí so với thuê server riêng, tính linh hoạt (cài lại hệ điều hành trong 5-10 phút, nâng cấp tài nguyên không cần khởi động lại), phù hợp cho doanh nghiệp vừa, website lớn hoặc ứng dụng nặng mà shared hosting không đáp ứng được. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/may-chu-ao-vps-virtual-private-server-la-gi/

**Hỏi: Công nghệ KVM, ảo hoá KVM là gì?**
Trả lời: KVM (Kernel Virtualization Machine) là công nghệ ảo hoá phần cứng tích hợp trong Linux, cho phép hệ điều hành mô phỏng phần cứng để các hệ điều hành khác chạy trên đó. KVM đóng vai trò quản lý tài nguyên, phân bổ công bằng CPU, network và ổ đĩa cho các máy ảo (VM). Đặc điểm chính: cấp phát tài nguyên riêng, cố định cho từng VM mà không chia sẻ, đảm bảo sử dụng 100% và không bị ảnh hưởng bởi VPS khác trên cùng hệ thống, giúp ổn định và cô lập. Các tính năng nổi bật: bảo mật cao nhờ tích hợp SELinux và sVirt của Linux; hỗ trợ nhiều loại lưu trữ (NAS, local storage, network-attached storage); quản lý bộ nhớ hiệu quả với swap; hỗ trợ Live Migration (di chuyển VM đang chạy giữa các máy chủ vật lý mà không gián đoạn); hiệu năng và khả năng mở rộng cao, độ trễ thấp; có thể quản lý máy ảo thủ công từ workstation mà không cần công cụ quản lý riêng. Kiến trúc này phù hợp cho data center, hạ tầng cloud và triển khai ảo hoá doanh nghiệp. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cong-nghe-kvm-ao-hoa-kvm-la-gi/

## Công cụ kiểm tra và benchmark VPS/Server

**Hỏi: Cách kiểm tra hiệu suất ổ cứng VPS với Fio và IOPing như thế nào?**
Trả lời: Để đánh giá chính xác hiệu năng ổ cứng VPS (thay vì dùng lệnh `dd` đơn giản), đo lường IOPS và độ trễ Latency bằng Fio và IOPing:
- Cài đặt Fio (đo IOPS): `yum install -y epel-release && yum install -y fio` (CentOS) hoặc `apt-get update && apt-get install -y fio` (Ubuntu/Debian).
- Cài đặt IOPing (đo Latency): `yum install -y epel-release && yum install -y ioping` (CentOS) hoặc `apt-get update && apt-get install -y ioping` (Ubuntu/Debian).
- Kiểm tra IOPS với Fio — Random Read + Write (tỉ lệ 75:25): `fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=TGS --filename=TGS --bs=4k --iodepth=64 --size=4G --readwrite=randrw --rwmixread=75`
- Random Read: `fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=TGS --filename=TGS --bs=4k --iodepth=64 --size=4G --readwrite=randread`
- Random Write: `fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=TGS --filename=TGS --bs=4k --iodepth=64 --size=4G --readwrite=randwrite`
- Kiểm tra độ trễ với IOPing: `ioping -c 10 .` (tham số `-c 10` là thực hiện 10 request, `.` là kiểm tra thư mục hiện tại).
- Tiêu chí đánh giá: IOPS tốt — SSD đạt khoảng 40.000-50.000 (đọc), 10.000-50.000 (ghi); HDD đạt khoảng 500 (đọc), 200 (ghi). Latency tốt — dưới 1.0 ms để hệ thống hoạt động ổn định.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/kiem-tra-hieu-suat-o-cung-voi-fio-va-ioping/

**Hỏi: Cách kiểm tra tốc độ mạng VPS/Server như thế nào?**
Trả lời: Để đo lường tốc độ kết nối internet trên VPS Linux bằng công cụ speedtest-cli:
- Cài đặt speedtest-cli: `wget https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py`, sau đó `chmod a+rx speedtest.py`, `sudo mv speedtest.py /usr/local/bin/speedtest-cli`, `sudo chown root:root /usr/local/bin/speedtest-cli`.
- Kiểm tra tốc độ cơ bản: chạy lệnh `speedtest-cli`, công cụ sẽ tự động kết nối đến speedtest.net server gần nhất và hiển thị tốc độ tải xuống/tải lên.
- Tùy chọn nâng cao: thêm `--share` để lưu kết quả thành hình ảnh; `speedtest-cli --list` để xem danh sách server; `speedtest-cli --server [ID]` để test từ server cụ thể.
- Có thể chọn server Việt Nam theo mã ID của Viettel, FPT Telecom, CMC Telecom, VNPT tại các thành phố chính để đo chính xác hơn.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/kiem-tra-toc-do-mang-vps-server/

**Hỏi: Lệnh kiểm tra các thông số VPS/Server như thế nào?**
Trả lời: Ngay sau khi nhận tài khoản VPS/Server, nên kiểm tra cấu hình phần cứng và network để xác minh thông tin từ nhà cung cấp:
- Kiểm tra CPU: `cat /proc/cpuinfo` — chú ý đếm số lượng processor, model name (loại CPU), cpu MHz (tốc độ).
- Kiểm tra phiên bản nhân Linux: `uname -a`.
- Kiểm tra phiên bản hệ điều hành: CentOS dùng `cat /etc/redhat-release`, Ubuntu dùng `lsb_release -a`.
- Kiểm tra RAM: `free -h`.
- Kiểm tra HDD: `df -h`.
- Kiểm tra tốc độ I/O ổ cứng: dùng Fio và IOPing (xem hướng dẫn riêng).
- Script tự động kiểm tra CPU, RAM, Network Speed và I/O cùng lúc: `wget freevps.us/downloads/bench.sh -O - -o /dev/null|bash`.
- Kiểm tra tốc độ network Việt Nam: dùng công cụ Speedtest hoặc công cụ test tốc độ chuyên dụng.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/lenh-kiem-tra-cac-thong-so-vps-server/

**Hỏi: Cách cài đặt và sử dụng phần mềm Remote Desktop Connection Manager (RDCMan) như thế nào?**
Trả lời: RDCMan là công cụ của Microsoft dành cho quản trị viên hệ thống, giúp tổ chức và quản lý nhiều kết nối RDP tập trung, hiệu quả:
- Cài đặt: tải phần mềm từ trang Sysinternals của Microsoft, giải nén file tải về, khởi chạy ứng dụng RDCMan.
- Tạo file quản lý: chọn File → New trong giao diện chính, tạo file định dạng .rdp chứa các cấu hình kết nối; nên lưu file này cẩn thận vì có thể sao chép sang máy khác để mở tất cả các VPS đã khai báo.
- Cấu hình port (nếu cần): nếu VPS không dùng port mặc định 3389, vào tab Connection Setting để chỉnh sửa port kết nối phù hợp — chỉ áp dụng cho các VPS đã thay đổi port remote.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-va-su-dung-phan-mem-remote-desktop-connection-manager/


**Hỏi: Cách cài đặt và sử dụng phần mềm Remote Desktop Connection Manager để quản lý nhiều VPS như thế nào?**
Trả lời: Remote Desktop Connection Manager (RDCMan) giúp quản lý tập trung nhiều kết nối RDP đến các VPS/server. Cách sử dụng:
- Bước 1: Tải phần mềm, giải nén và mở ứng dụng.
- Bước 2: Chọn File → New để tạo file định dạng `.rdg` — file này chứa khai báo kết nối từ xa đến các máy chủ. Lưu ý quan trọng: có thể copy file này sang máy tính khác để mở lại tất cả các VPS đã khai báo trước đó, không cần khai báo lại.
- Bước 3: Nhập thông tin kết nối (IP, tài khoản) cho từng VPS/máy chủ qua giao diện ứng dụng.
- Bước 4: Với VPS không dùng port RDP mặc định (3389), vào tab "Connection Setting" để điều chỉnh port kết nối đúng theo cấu hình của VPS. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-va-su-dung-phan-mem-remote-desktop-connection-manager/

**Hỏi: Cách kiểm tra hiệu suất ổ cứng VPS chính xác bằng Fio và IOPing như thế nào?**
Trả lời: Lệnh `dd` truyền thống có nhiều hạn chế khi kiểm tra hiệu suất ổ cứng: chỉ ghi tuần tự (không phản ánh đọc/ghi ngẫu nhiên thực tế), dễ bị cache hệ thống làm sai lệch kết quả, thời gian test quá ngắn để đáng tin cậy, và chỉ đo được tốc độ ghi. Hai thông số quan trọng cần đo: IOPS (số thao tác đọc/ghi thực hiện được mỗi giây, càng cao càng tốt) và Latency (độ trễ bắt đầu một lần truyền dữ liệu, càng thấp càng tốt).
- Đo IOPS bằng Fio: cài đặt qua lệnh phù hợp với CentOS/Ubuntu, chạy test random read/write mix (75%-25%), random read riêng, random write riêng với block size 4KB. Ví dụ tham khảo: ổ SSD đạt khoảng 46.000 IOPS đọc, 40.000 IOPS ghi; ổ không phải SSD chỉ khoảng 2.000 IOPS.
- Đo Latency bằng IOPing: cài đặt tương tự Fio, chạy lệnh `ioping -c 10 .` để kiểm tra 10 request. Độ trễ trung bình dưới 1.0 ms (ví dụ ~0.9 ms) được coi là ổn định. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/kiem-tra-hieu-suat-o-cung-voi-fio-va-ioping/

**Hỏi: Cách kiểm tra tốc độ mạng VPS/Server như thế nào?**
Trả lời: Dùng công cụ speedtest-cli để kiểm tra tốc độ mạng. Cài đặt:
```
wget https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
chmod a+rx speedtest.py
sudo mv speedtest.py /usr/local/bin/speedtest-cli
sudo chown root:root /usr/local/bin/speedtest-cli
```
Kiểm tra cơ bản: chạy `speedtest-cli` để tự động kết nối đến máy chủ gần nhất và hiển thị tốc độ tải lên/xuống. Tùy chọn nâng cao: `speedtest-cli --share` để lưu kết quả dạng hình ảnh chia sẻ; `speedtest-cli --list` để liệt kê danh sách máy chủ; `speedtest-cli --server [ID]` để test từ máy chủ cụ thể. Một số ID máy chủ Việt Nam phổ biến: 2428 (Viettel IDC Hà Nội), 2427 (Viettel IDC TP.HCM), 2552 (FPT Telecom Hà Nội), 2515 (FPT Telecom TP.HCM), 1648 (CMC Telecom TP.HCM), 6102/6106/6085 (VNPT-NET). Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/kiem-tra-toc-do-mang-vps-server/

**Hỏi: Các lệnh kiểm tra thông số cấu hình VPS/Server sau khi nhận tài khoản như thế nào?**
Trả lời: Sau khi nhận tài khoản VPS/Server, nên xác minh ngay cấu hình phần cứng và mạng bằng các lệnh sau:
- Kiểm tra CPU: `cat /proc/cpuinfo` — đếm số dòng `processor` để biết số nhân, xem `model name` (loại CPU) và `cpu MHz` (tốc độ).
- Kiểm tra nhân Linux: `uname -a`.
- Kiểm tra hệ điều hành: CentOS dùng `cat /etc/redhat-release`, Ubuntu dùng `lsb_release -a`.
- Kiểm tra RAM: `free -h`.
- Kiểm tra ổ cứng: `df -h`.
- Script kiểm tra toàn diện (CPU, RAM, Network Speed, IO) trong 1 lệnh: `wget freevps.us/downloads/bench.sh -O - -o /dev/null|bash`.
Để kiểm tra tốc độ mạng tới Việt Nam, dùng thêm công cụ Speedtest. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/lenh-kiem-tra-cac-thong-so-vps-server/

## Bảo mật Windows Server

**Hỏi: Cách mở port trên Windows Firewall như thế nào?**
Trả lời: Để cho phép kết nối từ bên ngoài đến máy chủ qua các port cụ thể, cấu hình quy tắc trong Windows Firewall:
- Bước 1: Nhấn Windows + R để mở Run, nhập `wf.msc` và Enter để mở Windows Firewall with Advanced Security.
- Bước 2: Chuột phải vào "Inbound Rules" → "New Rule" → chọn "Custom" → Next.
- Bước 3: Chọn "All Programs" → Next.
- Bước 4: Chọn loại giao thức (TCP/UDP), để "Local port" là "any", chọn "Specific Ports" cho "Remote port" và nhập port cần mở.
- Bước 5: "Local IP" chọn "any"; "Remote IP" chọn "Any" nếu cho phép tất cả, hoặc "These IP Address" để giới hạn.
- Bước 6: Chọn "Allow the connection" → Next.
- Bước 7: Tick cả ba tùy chọn loại mạng: Domain, Private, Public.
- Bước 8: Đặt tên và mô tả quy tắc (tùy chọn), nhấn "Finish".
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-mo-port-windows-firewall/

**Hỏi: Cách fix lỗi bảo mật Windows để chặn virus ransomware mã hoá dữ liệu như thế nào?**
Trả lời: Để ngăn ngừa virus ransomware mã hóa dữ liệu trên Windows 2012 R2 và các phiên bản Windows khác:
- Bước 0 (quan trọng nhất): Đổi port Remote Desktop thành port khác, không dùng port mặc định 3389.
- Bước 1: Cập nhật toàn bộ bản vá bảo mật cho Windows.
- Bước 2: Cài đặt gói sửa lỗi Windows bị ransomware khai thác — Windows 2012 R2 cài KB4012213; các phiên bản Windows khác tham khảo hỗ trợ của nhà sản xuất máy chủ (ví dụ Dell).
- Bước 3: Tắt dịch vụ Server qua Start → Run → `services.msc`, tìm và dừng dịch vụ Server.
- Bước 4: Dùng Firewall chặn các cổng 445, 135, 138, 139 bằng cách tạo quy tắc chặn cho cả TCP và UDP.
- Bước 5: Cài đặt và sử dụng phần mềm diệt virus để bảo vệ hệ thống.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-fix-loi-bao-mat-windows-chan-virus-ransomeware-ma-hoa-du-lieu/

**Hỏi: Cách mở port trên Windows Firewall như thế nào?**
Trả lời: Các bước mở port trên Windows Firewall:
- Bước 1: Nhấn Windows + R, gõ `wf.msc` và Enter để mở "Windows Firewall with Advanced Security".
- Bước 2: Chuột phải vào "Inbound Rules" → "New Rule", chọn "Custom" → Next, chọn "All Programs" → Next.
- Bước 3: Cấu hình giao thức và port — chọn Protocol type (TCP/UDP), chọn "Specific Ports" ở Remote port và nhập port cần mở.
- Bước 4: Cấu hình IP — Local IP để "Any", Remote IP để "Any" (cho phép tất cả) hoặc chỉ định IP cụ thể.
- Bước 5: Chọn "Allow the connection" để mở port.
- Bước 6: Chọn phạm vi áp dụng: Domain, Private, Public (hoặc kết hợp).
- Bước 7: Đặt tên và mô tả cho rule, nhấn Finish. Rule mới sẽ xuất hiện trong danh sách Inbound Rules và port được mở thành công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-mo-port-windows-firewall/

**Hỏi: Cách ngăn chặn virus ransomware mã hóa dữ liệu trên Windows Server như thế nào?**
Trả lời: Các bước phòng ngừa ransomware trên Windows Server (áp dụng cho Windows 2012 R2 và các phiên bản khác):
- Bước quan trọng nhất: đổi cổng Remote Desktop từ mặc định 3389 sang cổng khác để tránh truy cập trái phép.
- Bước 1: Cập nhật Windows với các bản vá mới nhất.
- Bước 2: Cài các gói vá lỗi mà ransomware thường khai thác (ví dụ KB4012213 cho Windows 2012 R2, các bản vá tương ứng cho phiên bản Windows khác).
- Bước 3: Dừng dịch vụ Server — vào Start → Run → gõ `services.msc`, tìm dịch vụ "Server" và tắt.
- Bước 4: Dùng tường lửa chặn các cổng 445, 135, 138, 139 (tạo rule cho cả TCP và UDP).
- Bước 5: Cài đặt và sử dụng phần mềm diệt virus để bảo vệ hệ thống liên tục. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-fix-loi-bao-mat-windows-chan-virus-ransomeware-ma-hoa-du-lieu/

## Kết nối và quản lý tài khoản VPS

**Hỏi: Cách thiết lập VPN trên Windows 10 như thế nào?**
Trả lời: Các bước cấu hình kết nối VPN trên Windows 10:
- Bước 1-2: Mở Settings từ Start, chọn "Network & Internet".
- Bước 3: Trong danh sách bên trái, chọn mục VPN.
- Bước 4: Chọn "Add a VPN connection".
- Bước 5: Điền thông tin: VPN provider chọn "Windows (built-in)", đặt Connection name, nhập Server name/address, chọn VPN Type (PPTP hoặc L2TP/IPsec), chọn Type of sign-in info là "Username and password", nhập tên đăng nhập và mật khẩu.
- Bước 6: Nhấn Save — tùy chọn "Remember my sign in information" chỉ nên bật trên máy cá nhân, tắt trên máy công cộng.
- Bước 7: Quay lại giao diện VPN, chọn mạng vừa tạo và nhấn Connect.
Lưu ý: nên chọn nhà cung cấp VPN uy tín, cập nhật Windows 10 lên bản mới nhất, cấu hình DNS nếu cần truy cập trang bị chặn, có thể tạm tắt tường lửa Windows trong lúc cài đặt. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/thiet-lap-vpn-tren-windows-10/

**Hỏi: Cách sử dụng PuTTY để SSH vào VPS như thế nào?**
Trả lời: Các bước kết nối VPS bằng PuTTY:
- Bước 1: Tải PuTTY bản 64-bit từ trang chính thức.
- Bước 2: Chuẩn bị thông tin kết nối VPS (IP, tài khoản, mật khẩu) — các thông tin này được gửi qua email khi đăng ký dịch vụ; nếu quên mật khẩu, liên hệ bộ phận kỹ thuật để yêu cầu khởi tạo lại.
- Bước 3: Mở PuTTY, nhập địa chỉ IP, sau đó nhập tài khoản và mật khẩu được cấp — lưu ý mật khẩu sẽ không hiển thị khi gõ trong PuTTY, chỉ cần nhập chính xác rồi Enter.
Đổi mật khẩu sau khi đăng nhập: gõ lệnh `passwd root` rồi nhập mật khẩu mới 2 lần để xác nhận. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-putty-de-ssh-vao-vps/

**Hỏi: Cách cài đặt Remote Desktop (giao diện đồ họa) trên VPS Ubuntu như thế nào?**
Trả lời: Cài đặt giao diện đồ họa LXDE và Xrdp trên VPS Ubuntu để dùng Remote Desktop Windows kết nối vào, giúp tiết kiệm chi phí so với thuê Windows VPS. Các bước:
- Bước 1: Gỡ các gói không cần thiết: `apt-get purge apache2* bind9* samba*`.
- Bước 2: Cập nhật hệ thống: `apt-get update` rồi `apt-get upgrade`.
- Bước 3: Cài LXDE Desktop Environment: `apt-get install lxde-core` rồi `apt-get install xrdp`.
- Bước 4: Cài trình duyệt — Firefox: `apt-get install firefox` và `apt-get install flashplugin-installer`; hoặc Chromium: `apt-get install chromium-browser`.
- Kết nối: mở Remote Desktop Connection trên Windows, nhập IP của VPS, điền tài khoản/mật khẩu, nhấn OK để kết nối.
LXDE là giao diện nhẹ, tốn ít tài nguyên hệ thống, phù hợp cả cho VPS có RAM thấp. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-remote-desktop-tren-vps-ubuntu/

**Hỏi: Cách cài đặt WHM/cPanel trên CentOS VPS của Thế Giới Số như thế nào?**
Trả lời: Các bước cài đặt WHM/cPanel:
- Bước 1: Kết nối SSH vào VPS bằng tài khoản root.
- Bước 2: Cài các gói cần thiết lần lượt: `yum install selinux coreutils binutils make dialog gcc gcc-* glib*`, `yum install libexi* libjpe* libpng* gifl* freetype curl curl-* xmlrpc`, `yum upgrade kernel*`, `yum update`.
- Bước 3: Tải và chạy trình cài đặt: `cd /home`, `wget -N http://httpupdate.cpanel.net/latest`, `sh latest`.
Thời gian cài đặt thường 1-2 giờ, có thể lên tới 10 giờ với VPS cấu hình thấp. Sau khi hoàn tất, truy cập `http://<IP-VPS>:2087` để đăng nhập bằng tài khoản root. Cập nhật bản quyền bằng lệnh: `/usr/local/cpanel/cpkeyclt`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-whmcpanel-tren-centos-vps-cua-tgs/

**Hỏi: Cách reset lại mật khẩu VPS CentOS 7 khi quên như thế nào?**
Trả lời: Các bước reset mật khẩu root khi quên trên VPS CentOS 7 (khác quy trình so với CentOS 6 do thay đổi cách khởi động):
- Bước 1: Tại menu khởi động GRUB, chọn tùy chọn để chỉnh sửa.
- Bước 2: Nhấn `e`, tìm dòng chứa `ro`, thay thành `rw init=/sysroot/bin/sh`.
- Bước 3: Nhấn Ctrl+X để khởi động vào chế độ single user mode.
- Bước 4: Truy cập hệ thống bằng lệnh `chroot /sysroot`.
- Bước 5: Đặt mật khẩu mới cho root bằng `passwd root`.
- Bước 6: Cập nhật thông tin SELinux và thoát: `touch /.autorelabel` rồi `exit`.
- Khởi động lại hệ thống — đăng nhập được bằng mật khẩu mới đã tạo. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-reset-lai-mat-khau-vps-centos-7/

**Hỏi: Cách thay đổi mật khẩu VPS Windows Server như thế nào?**
Trả lời: Với Windows Server 2008: vào Start → Control Panel → chọn "User Accounts" → chọn "Make changes to your user account" → "Change your password" → nhập mật khẩu hiện tại, mật khẩu mới, xác nhận lại mật khẩu mới (có thể thêm gợi ý mật khẩu tùy chọn) → nhấn "Change password". Với Windows Server 2003: vào Start → chuột phải "Administrative Tools" → Open → mở Computer Management → vào Local Users and Groups → Users → chuột phải vào tài khoản cần đổi → chọn "Set Password" → nhập mật khẩu mới, xác nhận → OK. Cả hai cách đều cần quyền quản trị viên. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-thay-doi-mat-khau-vps-windows-server/

**Hỏi: Cần lưu ý những gì khi thuê VPS?**
Trả lời: Khác với shared hosting, mỗi VPS sở hữu tài nguyên độc lập (RAM, CPU) không chia sẻ với website khác trên cùng máy chủ vật lý. Các thông số quan trọng cần xem khi mua VPS:
- RAM: dùng để xử lý mã PHP, truy vấn database MySQL — khuyến nghị tối thiểu 1GB cho WordPress.
- SWAP: bộ nhớ ảo lưu trên ổ cứng, được dùng khi RAM đầy.
- Disk/ổ cứng: HDD tốc độ khoảng 80MB/s, SSD tốc độ trên 400MB/s (đắt hơn nhưng nhanh hơn).
- CPU Core: số lõi xử lý, thường từ 1 đến 6 core.
- Bandwidth: lưu lượng truyền dữ liệu cho phép.
- IP: số lượng địa chỉ IP được cấp.
- Control Panel: cPanel, DirectAdmin hoặc Plesk (thường tính phí thêm khoảng $8-15/tháng).
- Hệ điều hành: nên dùng Linux (CentOS, Ubuntu, Debian) cho website WordPress/PHP.
Phân loại quản lý: Managed VPS (nhà cung cấp hỗ trợ cấu hình, tối ưu, bảo mật — giá cao hơn, phù hợp người mới) và Unmanaged VPS (tự cài webserver, cấu hình, phần mềm — giá rẻ hơn nhưng cần kiến thức kỹ thuật). Công nghệ ảo hóa: KVM (ảo hóa toàn phần, tài nguyên vật lý thực sự, giá cao, hiệu năng tốt) và OpenVZ (chia sẻ tài nguyên CPU, giá rẻ hơn, dễ nâng cấp không cần khởi động lại). Khi mua VPS cần chuẩn bị: thẻ Visa hoặc PayPal, bản scan CMND 2 mặt, bản scan thẻ thanh toán 2 mặt. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/vps-la-gi-va-luu-y-gi-khi-thue-vps/

**Hỏi: Cách cài đặt thêm Memcached và Zend Opcache để tăng tốc website như thế nào?**
Trả lời: Cài Memcached: trên CentOS/RHEL dùng `yum install --enablerepo=remi php-pecl-memcache memcached libmemcached -y`; trên Ubuntu/Debian dùng `apt-get install memcached php5-memcache`. Khởi động dịch vụ bằng `service memcached start`, bật tự khởi động (CentOS) bằng `chkconfig memcached on`, sau đó khởi động lại Apache (`service httpd restart` hoặc `service apache2 restart`). Với WordPress, cài thêm plugin "Memcached is Your Friend" (không cần cấu hình thêm), theo dõi cache tại Tools → Memcached. Cài Zend Opcache: dùng lệnh `yum install --enablerepo=remi php-pecl-zendopcache -y` rồi khởi động lại Apache. Lưu ý: lần tải đầu tiên sau khi cài sẽ chậm hơn một chút; Memcached tiêu thụ thêm RAM do lưu cache trong bộ nhớ nhưng hệ thống tự giải phóng khi cần. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cai-dat-them-memcahed-va-zend-opcache/

**Hỏi: Cách xác định địa chỉ IP Public đang sử dụng như thế nào?**
Trả lời: Máy tính cá nhân thường dùng IP Private trong mạng nội bộ, và được định tuyến qua Router Gateway (dùng IP Public) để kết nối Internet. Việc xác định IP Public cần thiết khi gặp lỗi không truy cập được cPanel/WHM/webmail — thường do tường lửa khóa IP Public sau nhiều lần đăng nhập sai, và nhân viên kỹ thuật cần địa chỉ IP cùng kết quả Ping/Tracert để gỡ khóa. Cách xác định: truy cập trang https://showip.net/ — địa chỉ IP Public sẽ hiển thị ngay, dùng để cung cấp cho nhân viên hỗ trợ kỹ thuật xử lý. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/xac-dinh-dia-chi-ip-public-dang-su-dung/

**Hỏi: Cách sử dụng ZOC Terminal để đăng nhập vào server qua SSH như thế nào?**
Trả lời: ZOC Terminal là phần mềm SSH client nặng hơn PuTTY và có phí, nhưng giao diện trực quan, dễ dùng hơn, tương thích Windows XP/7/8 và Mac OS X. Các bước sử dụng:
- Bước 1: Tải xuống ZOC Terminal.
- Bước 2: Nhấp đúp vào biểu tượng để khởi động ứng dụng.
- Bước 3: Nhấn nút "Connect" ở góc phải cửa sổ.
- Bước 4: Xác nhận kết nối thành công khi giao diện terminal hiển thị.
Đây là kỹ năng cơ bản mà quản trị viên server nào cũng nên biết. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-zoc-terminal-dang-nhap-vao-server-qua-ssh/
