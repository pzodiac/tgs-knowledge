# Hướng dẫn quản trị VPS/Linux

> **Danh mục:** Hướng dẫn kỹ thuật - VPS/Linux
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** vps, linux, ssh, putty, remote desktop, vpn, port ssh

## Kết nối và đăng nhập VPS

**Hỏi: Cách kết nối vào VPS Linux từ Windows bằng Command Prompt như thế nào?**
Trả lời: Để kết nối vào VPS Linux từ Windows qua Command Prompt:
- Bước 1: Mở Command Prompt bằng cách tìm kiếm "cmd" trong thanh tìm kiếm hệ thống Windows.
- Bước 2: Nhập lệnh theo format `ssh@<ip-address>` với `<ip-address>` được thay thế bằng địa chỉ IP của VPS đã đăng ký trên portal của Thế Giới Số. Sau khi thực hiện lệnh, điền mật khẩu để hoàn tất kết nối vào máy chủ. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-ket-noi-vao-vps-linux/

**Hỏi: Cách đăng nhập vào VPS Linux với SSH như thế nào?**
Trả lời: SSH (Secure Shell) là giao thức mạng bảo mật để truy cập và quản lý VPS từ xa. Thông tin cần chuẩn bị: IP máy chủ, tên người dùng đăng nhập (thường là root), mật khẩu người dùng, và cổng giao tiếp mặc định 22.
- Đăng nhập từ Windows: sử dụng phần mềm PuTTY, nhập địa chỉ IP máy chủ vào trường được chỉ định (cổng mặc định là 22).
- Đăng nhập từ Linux/macOS: mở Terminal và chạy lệnh `ssh root@103.0.24.123` (thay IP thực tế). Nếu dùng cổng khác: `ssh root@103.0.24.123 -p 8993`.
- Cài đặt SSH trên RHEL/CentOS: chạy lệnh cài đặt `openssh-server`, `openssh-clients` và khởi động dịch vụ.
- Cài đặt SSH trên Ubuntu/Debian: sử dụng `apt install openssh-client` và `openssh-server`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-dang-nhap-vao-vps-linux-voi-ssh/

**Hỏi: Cách đổi port SSH để tăng cường bảo mật như thế nào?**
Trả lời: Thay đổi cổng SSH từ mặc định (22) sang cổng khác là biện pháp bảo vệ hiệu quả cho máy chủ:
- Bước 1: Mở tệp cấu hình SSH bằng lệnh `nano /etc/ssh/sshd_config`. Nếu chưa có Nano, cài bằng `apt-get install nano -y` (Ubuntu) hoặc `yum install nano -y` (CentOS).
- Bước 2: Tìm dòng "Port 22" trong tệp (xóa ký tự # nếu có), thay giá trị bằng cổng mong muốn, ví dụ "Port 6969". Lưu bằng Ctrl+O, Enter, thoát bằng Ctrl+X.
- Bước 3: Khởi động lại dịch vụ SSH — Ubuntu: `services ssh restart`; CentOS: `service sshd restart`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-doi-port-ssh-de-tang-cuong-bao-mat/

**Hỏi: Cách sử dụng Remote Desktop trên điện thoại Android để kết nối VPS/PC như thế nào?**
Trả lời: Để kết nối máy tính hoặc VPS từ điện thoại Android qua ứng dụng Remote Desktop:
- Bước 1: Tìm kiếm "remote desktop" trên Google Play Store, tải phiên bản mới nhất do Microsoft Corp phát hành (tránh bản Remote Desktop 8 vì là phiên bản cũ).
- Bước 2: Chấp nhận điều khoản sử dụng khi mở lần đầu, nhấn dấu "+" và chọn "Add PC", chọn "ADD MANUALLY" để nhập thủ công, điền địa chỉ IP, tên người dùng và mật khẩu VPS/PC, rồi lưu tài khoản để đăng nhập lần sau.
- Bước 3: Chọn PC cần remote từ trang chính ứng dụng để bắt đầu phiên kết nối. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-remote-desktop-tren-dien-thoai-android/

## Quản trị hệ thống Linux

**Hỏi: Cách xóa những tập tin có kích thước khổng lồ trên Linux mà không ảnh hưởng hiệu năng như thế nào?**
Trả lời: Với các file rất lớn (100GB-200GB), nên dùng lệnh `ionice` để quản lý mức ưu tiên I/O khi xóa, tránh ảnh hưởng hiệu năng hệ thống:
- Cách 1 - Idle Class: chạy `ionice -c 3 rm <tên file>` để gán tác vụ xóa vào lớp I/O idle, chỉ dùng tài nguyên khi các tiến trình khác không cần.
- Cách 2 - Best-Effort mức ưu tiên thấp: dùng `ionice -c 2 -n 6 rm <tên file>` khi thời gian rảnh của hệ thống hạn chế.
- Lưu ý: với file nhạy cảm, nên cân nhắc dùng công cụ xóa an toàn như `shred` hoặc `wipe` thay vì lệnh `rm` thông thường. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/lam-the-nao-de-xoa-nhung-tap-tin-co-kich-thuoc-khong-lo-tren-linux/

**Hỏi: Cách chuyển đổi default gateway của máy tính khi kết nối VPN Host-to-LAN như thế nào?**
Trả lời: Nếu bị mất kết nối Internet khi dùng VPN trên Windows, khắc phục bằng cách tắt tính năng "Use default gateway on remote network":
- Bước 1: Mở cửa sổ Run (Start → nhập `ncpa.cpl`).
- Bước 2: Thiết lập kết nối VPN (nếu chưa có), sau đó chuột phải vào kết nối VPN và chọn Properties.
- Bước 3: Vào tab Networking, chọn giao thức TCP/IP rồi nhấn Properties.
- Bước 4: Bấm nút Advanced để mở cài đặt nâng cao.
- Bước 5: Bỏ dấu check tại ô "Use default gateway on remote network" để ngăn VPN thay đổi cổng mạng mặc định. Kết nối lại VPN để duy trì kết nối Internet thường xuyên trong khi dùng VPN. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/chuyen-doi-default-gateway-cua-may-tinh-khi-ket-noi-vpn-host-to-lan/

## Cài đặt hệ điều hành

**Hỏi: Cách cài đặt CentOS 7 như thế nào?**
Trả lời: CentOS (Community Enterprise Operating System) là bản phân phối hệ điều hành tự do mã nguồn mở dựa trên Linux Kernel, phát triển từ Red Hat Enterprise Linux (RHEL). Cần chuẩn bị máy chủ/máy ảo để cài đặt và đĩa cài CentOS (tải từ centos.org) — hướng dẫn tập trung vào bản Minimal. Các bước cài đặt:
- Bước 1: Gắn đĩa cài đặt và khởi động máy chủ, ở menu khởi đầu chọn "Install CentOS 7" và chọn ngôn ngữ (khuyến nghị English).
- Bước 2: Cấu hình phân vùng — có thể dùng mặc định hoặc tự tùy chỉnh thủ công. Sơ đồ phân vùng khuyến nghị: /boot từ 200MB trở lên (chứa file kernel để khởi động hệ thống), swap tùy theo nhu cầu sử dụng, / (root) chứa toàn bộ dữ liệu máy chủ dùng phần dung lượng còn lại.
- Bước 3: Nếu phân vùng thủ công, đổi kiểu phân vùng từ LVM sang Standard Partition, dùng nút + để thêm từng phân vùng, sau đó chọn "Accept Changes" để xác nhận — lưu ý thao tác này sẽ format toàn bộ ổ cứng.
- Bước 4: Trước khi cài đặt hoàn tất, cấu hình thêm mạng, hostname, ngày giờ, tạo tài khoản người dùng hoặc đặt mật khẩu root.
- Bước 5: Sau khi cài đặt xong, chọn "Reboot" để khởi động lại máy chủ và vào hệ điều hành CentOS vừa cài.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-centos-7/

## Quản trị lưu trữ và phân vùng ổ đĩa

**Hỏi: Cách sử dụng lệnh fdisk trên Linux để quản lý phân vùng ổ đĩa như thế nào?**
Trả lời: fdisk là tiện ích quản lý phân vùng đĩa cứng trên Linux, cho phép xem, tạo, thay đổi kích thước và xóa phân vùng, hỗ trợ tối đa 4 phân vùng chính với kích thước tối thiểu 40MB. Các bước sử dụng:
- Liệt kê phân vùng: xem phân vùng của thiết bị cụ thể bằng `fdisk -l /dev/sda`, hoặc xem tất cả bằng `fdisk -l`.
- Tạo bảng phân vùng: chạy `fdisk /dev/sdb`, nhập `m` để xem danh sách lệnh, nhập `g` để chọn lược đồ GPT (khuyến nghị cho đĩa trên 2TB).
- Tạo phân vùng mới: nhập `n`, chọn số phân vùng (mặc định 1), xác nhận sector đầu tiên (mặc định 2048), đặt kích thước (ví dụ `+1G` cho 1GiB). Lặp lại để tạo thêm phân vùng.
- Xác nhận và lưu: nhập `p` để hiển thị bảng phân vùng, nhập `w` để ghi và thoát.
- Định dạng phân vùng: `sudo mkfs.ext4 -F /dev/sdb1`.
- Gắn kết (mount) phân vùng: tạo thư mục bằng `sudo mkdir -p /mnt/audio`, sau đó `sudo mount /dev/sdb1 /mnt/audio`. Để tự động gắn kết khi khởi động, thêm cấu hình vào file `/etc/fstab`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/su-dung-lenh-fdisk-tren-linux/

**Hỏi: Cách mount và umount ổ cứng hay thiết bị trên Linux như thế nào?**
Trả lời: Khác với Windows, các thiết bị lưu trữ trên Linux (USB, CD/DVD, file ISO, phân vùng ổ cứng, network share) cần được "mount" vào một thư mục trống (mount point) trước khi truy cập được. Cách mount theo device file:
- Bước 1: Xác định ổ đĩa bằng lệnh `lsblk` để xem các thiết bị lưu trữ đang kết nối.
- Bước 2: Tạo mount point là một thư mục trống, ví dụ `/home/tmp/`.
- Bước 3: Chạy lệnh mount: `mount -t ext4 -o defaults /dev/sdb1 /home/tmp/`.
- Bước 4: Xác nhận thành công bằng lệnh `df -h`.
Cách mount theo UUID (khuyến nghị khi có nhiều ổ đĩa, tránh lỗi khi tên thiết bị đổi sau khi khởi động lại): tìm UUID bằng `lsblk -o NAME,UUID,SIZE`, sau đó thêm vào file `/etc/fstab` dòng dạng `UUID=<uuid-value> /home/tmp/ ext4 defaults 0 0`.
Để ngắt kết nối an toàn, dùng lệnh `umount /dev/sdb1` hoặc `umount /home/tmp` (lưu ý là "umount" không phải "unmount"). Quan trọng: xóa dòng tương ứng khỏi `/etc/fstab` để tránh bị tự động mount lại khi khởi động. Quy trình tương tự áp dụng cho các thiết bị khác như USB, CD/DVD. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/mount-umount-o-cung-hay-thiet-bi-tren-linux/

**Hỏi: Cách mở rộng dung lượng ổ cứng của máy ảo CentOS trên VMware như thế nào?**
Trả lời: Hướng dẫn mở rộng dung lượng đĩa máy ảo CentOS bằng LVM (Logical Volume Manager):
- Bước 1: Kiểm tra dung lượng hiện tại bằng `df -h` và `fdisk /dev/sda` trước khi mở rộng.
- Bước 2: Tắt máy ảo, tăng dung lượng đĩa trong cài đặt VMware (ví dụ từ 20GB lên 40GB), khởi động lại và xác nhận bằng fdisk.
- Bước 3: Tạo phân vùng mới bằng `fdisk /dev/sda` để tạo phân vùng sda3 với dung lượng mới trống, sau đó khởi động lại hệ thống.
- Bước 4: Mở rộng Volume Group: chạy `pvcreate /dev/sda3` rồi `vgextend centos /dev/sda3` để đưa phân vùng mới vào nhóm ổ đĩa.
- Bước 5: Mở rộng Logical Volume: chạy `lvextend -l +100%FREE /dev/mapper/centos-root` để cấp toàn bộ dung lượng trống cho volume root.
- Bước 6: Resize hệ thống file: với CentOS 7 dùng `xfs_growfs /dev/mapper/centos-root` (CentOS 6 dùng `resize2fs`).
- Bước 7: Xác nhận bằng `df -h` để thấy dung lượng đã tăng.
Lưu ý: nên sao lưu dữ liệu trước khi thực hiện các thao tác này do có rủi ro. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/mo-rong-dung-luong-o-cung-cua-may-ao-centos-tren-vmware/

## Cài đặt phần mềm và dịch vụ trên Linux

**Hỏi: Cách setup NFS Server và Client trên CentOS 7.x để chia sẻ dữ liệu như thế nào?**
Trả lời: Các bước thiết lập NFS (Network File System) để chia sẻ dữ liệu giữa các máy chủ CentOS 7.x:
- Bước 1 - Cấu hình Firewall: cài firewalld bằng `yum -y install firewalld`, khởi động và bật tự khởi động bằng `systemctl start firewalld.service` và `systemctl enable firewalld.service`, sau đó mở các cổng cần thiết (SSH, NFS) bằng `firewall-cmd --permanent --zone=public --add-service=<tên dịch vụ>` rồi reload.
- Bước 2 - Cài đặt NFS: trên server chạy `yum -y install nfs-utils` rồi kích hoạt dịch vụ; trên client chạy `yum install nfs-utils`.
- Bước 3 - Tạo thư mục chia sẻ: tạo thư mục `/var/nfs`, thiết lập quyền sở hữu cho user `nfsnobody`, chỉnh sửa file `/etc/exports` để khai báo các phân vùng chia sẻ kèm địa chỉ IP client được phép truy cập.
- Bước 4 - Mount trên Client: tạo điểm mount và gắn kết thư mục chia sẻ từ server.
- Bước 5 - Kiểm tra: tạo file thử trên client, xác nhận file xuất hiện trên server.
- Bước 6 - Mount tự động: thêm cấu hình vào `/etc/fstab` để NFS tự động mount khi hệ thống khởi động. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/setup-nfs-server-va-client-tren-centos-7-x-de-chia-se-du-lieu/

**Hỏi: Cách cài đặt Zabbix 3.0 (Monitoring Server) trên VPS CentOS 7.x/RHEL 7.x như thế nào?**
Trả lời: Các bước cài đặt Zabbix để giám sát hệ thống:
- Chuẩn bị: cập nhật hệ thống và khởi động lại trước khi cài đặt.
- Bước 1 - Thiết lập Repository: cài `epel-release` và thêm repo Zabbix qua RPM.
- Bước 2 - Cài đặt gói: cài Zabbix server, database MariaDB, web server Apache và PHP bằng yum.
- Bước 3 - Cấu hình Database: khởi động MariaDB, bảo mật cài đặt, tạo database `zabbix_db`, tạo user, import schema database từ `/usr/share/doc/zabbix-server-mysql-3.0.1/create.sql`.
- Bước 4 - Cấu hình Server: chỉnh sửa `/etc/zabbix/zabbix_server.conf` với thông tin database, cấu hình PHP trong `/etc/php.ini` (timezone, memory limit, upload size).
- Bước 5 - Firewall & SELinux: mở các cổng 10050, 10051, 80; áp dụng rule SELinux để Apache kết nối được.
- Bước 6 - Kích hoạt dịch vụ: khởi động và bật tự khởi động cho zabbix-server và httpd.
- Bước 7 - Giao diện Web: truy cập `http://<IP-server>/zabbix/` để hoàn tất cấu hình qua trình thiết lập web.
- Bước 8 - Cài Agent: cài zabbix-agent trên các node cần giám sát và cấu hình kết nối về server. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/install-zabbix-3-0-monitoring-server-tren-vps-su-dung-centos-7-x-rhel-7-x/

**Hỏi: Cách phát hiện tấn công xmlrpc.php/wp-login.php bằng công cụ tcpflow như thế nào?**
Trả lời: Tcpflow là công cụ phân tích traffic mạng đơn giản, dễ dùng, phù hợp cho quản trị viên VPS tự theo dõi lưu lượng HTTP. Cài đặt trên CentOS: `yum install --nogpgcheck http://pkgs.repoforge.org/tcpflow/tcpflow-0.21-1.2.el6.rf.x86_64.rpm`. Giám sát traffic HTTP để phát hiện tấn công: chạy lệnh `tcpflow -p -c -i eth0 port 80 | grep -oE '(GET|POST|HEAD) .* HTTP/1.[01]|Host: .*'` để theo dõi các request đến xmlrpc.php hoặc wp-login.php — đây là các endpoint thường bị tấn công brute-force hoặc khai thác lỗ hổng trên WordPress. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/su-dung-mytop-de-monitor-mysql-performance/

**Hỏi: Cách cài đặt Nextcloud trên CentOS 7 như thế nào?**
Trả lời: Yêu cầu: CentOS 7, tối thiểu 1GB RAM, 10GB dung lượng đĩa trống, PHP 7. Các bước cài đặt:
- Bước 1 - Cài Web Server: cài LAMP (Apache-MariaDB-PHP) hoặc LEMP (Nginx-MariaDB-PHP).
- Bước 2 - Tạo Database: tạo database MySQL cùng user/mật khẩu bằng câu lệnh SQL.
- Bước 3 - Cấu hình Virtual Host: cấu hình Nginx (`/etc/nginx/conf.d/nextcloud.conf`) hoặc Apache (`/etc/httpd/conf.d/nextcloud.conf`), tạo thư mục document root.
- Bước 4 - Firewall: mở dịch vụ HTTP/HTTPS bằng `firewall-cmd --permanent --add-service=http` (và tương tự cho https).
- Bước 5 - Cài đặt Nextcloud: tải bản 18.0.3 bằng wget, giải nén và chuyển vào thư mục public_html, đặt đúng quyền sở hữu cho user apache hoặc nginx tùy cấu hình, truy cập domain trên trình duyệt để hoàn tất qua wizard.
- Bước 6 - Cấu hình: tạo tài khoản admin (tránh dùng username "Admin"), chọn database MySQL/MariaDB, nhập thông tin database đã tạo ở bước 2, hoàn tất setup wizard. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-nextcloud-tren-centos-7/

**Hỏi: Cách cài đặt Composer trên CentOS 7 như thế nào?**
Trả lời: Composer là chương trình dòng lệnh cài đặt package và thư viện từ kho packagist.org, là công cụ quản lý dependency phổ biến nhất cho dự án PHP. Yêu cầu trước khi cài: quyền truy cập SSH với tư cách root, cURL, PHP (bao gồm php-cli). Các bước cài đặt:
- Bước 1: Tải và cài đặt bằng lệnh `curl -sS https://getcomposer.org/installer | php`.
- Bước 2: Di chuyển file thực thi bằng `mv composer.phar /usr/local/bin/composer`.
- Bước 3: Xác minh cài đặt bằng `composer --version`.
Các lệnh Composer cơ bản: cài package `composer require package-name`; gỡ package `composer remove package-name`; cập nhật tất cả `composer update`; cập nhật không kèm gói dev `composer update --no-dev`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cach-cai-dat-composer-tren-centos-7/

**Hỏi: Cách cài đặt Let's Encrypt trên Ubuntu 18 như thế nào?**
Trả lời: Let's Encrypt là cơ quan cấp chứng chỉ SSL/TLS miễn phí. Các bước cài đặt trên Ubuntu 18:
- Bước 1 - Cài Certbot: chạy lần lượt `sudo apt-get install software-properties-common`, `sudo add-apt-repository universe`, `sudo add-apt-repository ppa:certbot/certbot`, `sudo apt-get install certbot`, rồi `sudo apt update && sudo apt upgrade -y`.
- Bước 2 - Nhận chứng chỉ SSL: chạy `sudo certbot-auto certonly --standalone -d example.com -d www.example.com`, cung cấp email và trả lời các câu hỏi xác thực để nhận chứng chỉ.
- Bước 3 - Kiểm tra chứng chỉ: vào thư mục `/etc/letsencrypt/live/example.com` và chạy `ls` để xem file chứng chỉ.
- Bước 4 - Cấu hình Virtual Host: với Nginx, thêm cấu hình SSL trỏ đến `fullchain.pem` và `privkey.pem` trong thư mục chứng chỉ; với Apache, khai báo `SSLEngine on`, `SSLCertificateFile`, `SSLCertificateKeyFile`, `SSLCertificateChainFile`.
- Bước 5 - Tự động gia hạn: chạy `EDITOR=nano crontab -e`, thêm dòng `0 2 * * * sudo /usr/bin/certbot -q renew` để tự động gia hạn chứng chỉ. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cai-dat-lets-encrypt-tren-ubuntu-18/

**Hỏi: Cách thay đổi Timezone trên CentOS 8 như thế nào?**
Trả lời: Các bước thay đổi múi giờ hệ thống trên CentOS 8:
- Bước 1: Kiểm tra múi giờ hiện tại bằng lệnh `timedatectl`, hoặc kiểm tra liên kết tượng trưng bằng `ls -l /etc/localtime`.
- Bước 2: Xác định múi giờ mới cần đổi sang — tham khảo danh sách đầy đủ tại https://www.php.net/manual/en/timezones.php.
- Bước 3: Áp dụng múi giờ mới bằng lệnh `timedatectl set-timezone <tên-múi-giờ>` (ví dụ: `Asia/Ho_Chi_Minh`).
- Bước 4: Chạy lại `timedatectl` để xác nhận cấu hình múi giờ đã được áp dụng thành công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-thay-doi-timezone-tren-centos-8/

## Quản trị hệ thống nâng cao

**Hỏi: Cách vô hiệu hóa (tắt) SELinux trên CentOS 8 như thế nào?**
Trả lời: SELinux (Security Enhanced Linux) là cơ chế bảo mật tích hợp trong nhân Linux, được các bản phân phối dựa trên RHEL sử dụng, thêm lớp kiểm soát truy cập qua các quy tắc chính sách. SELinux có 3 chế độ hoạt động: Enforcing (áp dụng đầy đủ quy tắc), Permissive (chỉ ghi log các hành động bị từ chối, không chặn), Disabled (tắt hoàn toàn). Các bước thực hiện:
- Bước 1: Kiểm tra chế độ hiện tại bằng lệnh `sestatus`.
- Bước 2: Chuyển tạm thời sang Permissive bằng `setenforce 0`.
- Bước 3: Đổi vĩnh viễn thành Permissive: `sed -i 's/SELINUX=enforcing/SELINUX=permissive/g' /etc/selinux/config` rồi khởi động lại bằng `shutdown -r now`.
- Bước 4: Nếu muốn tắt hoàn toàn: `sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config` và `sed -i 's/SELINUX=permissive/SELINUX=disabled/g' /etc/selinux/config`, sau đó khởi động lại.
Lưu ý: nên chuyển sang chế độ Permissive thay vì tắt hoàn toàn để vẫn đảm bảo an ninh hệ thống. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-vo-hieu-hoa-selinux-tren-centos-8/

**Hỏi: Cách tạo Swap trên CentOS 8 như thế nào?**
Trả lời: Các bước tạo và quản lý bộ nhớ Swap trên CentOS 8:
- Tạo swapfile (ví dụ 1GB): `fallocate -l 1G /swapfile` (nếu lệnh không hoạt động, dùng `dd if=/dev/zero of=/swapfile bs=1024 count=1048576`).
- Thiết lập quyền và kích hoạt: `chmod 600 /swapfile`, sau đó `mkswap /swapfile` và `swapon /swapfile`.
- Xác minh: `swapon --show`.
- Làm vĩnh viễn: thêm dòng `/swapfile swap swap defaults 0 0` vào file `/etc/fstab`.
- Điều chỉnh swappiness: kiểm tra giá trị hiện tại bằng `cat /proc/sys/vm/swappiness`, đặt giá trị mới (ví dụ 10) bằng `sysctl vm.swappiness=10`, để lưu vĩnh viễn thêm `vm.swappiness=10` vào `/etc/sysctl.conf`.
- Xóa swap file khi cần: vô hiệu hóa bằng `swapoff -v /swapfile`, xóa dòng cấu hình khỏi `/etc/fstab`, rồi xóa file bằng `rm /swapfile`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-swap-tren-centos-8/

**Hỏi: Cách cài đặt Git trên CentOS 7 như thế nào?**
Trả lời: Git là hệ thống kiểm soát phiên bản (VCS) phổ biến, do Linus Torvalds sáng lập. Các bước cài đặt và sử dụng cơ bản:
- Cài đặt: `sudo yum makecache`, `yum -y install epel-release`, `sudo yum -y install git`, kiểm tra bằng `git --version`.
- Cấu hình global: `git config --global user.name 'TÊN ĐẦY ĐỦ'` và `git config --global user.email 'EMAIL'`.
- Khởi tạo repository: `cd THU-MUC-DU-AN` rồi `git init`.
- Theo dõi file: `git status` để kiểm tra trạng thái, `git add -A` để thêm tất cả file.
- Commit thay đổi: `git commit -m 'MÔ TẢ THAY ĐỔI'`.
- Xem lịch sử: `git log` hoặc `git log --oneline`.
- Push lên GitHub: `git remote add origin URL-REPOSITORY` rồi `git push -u origin master`.
- Clone repository: `git clone URL-REPOSITORY`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-git-tren-centos-7/

**Hỏi: Cách cài đặt Redis trên CentOS 7 như thế nào?**
Trả lời: Redis là hệ thống lưu trữ dữ liệu dạng key-value mạnh mẽ, dùng làm database, bộ nhớ cache hoặc message broker. Các bước cài đặt:
- Bước 1: Cài EPEL bằng `yum install epel-release`.
- Bước 2: Cài Redis: `yum install -y redis`, sau đó `systemctl enable redis` và `systemctl start redis`.
- Bước 3: Cài Igbinary — tải bản phù hợp phiên bản PHP từ pecl.php.net (igbinary-2.0.8.tgz cho PHP 5.6, igbinary-3.1.2.tgz cho PHP 7), chạy `phpize`, `configure`, `make`, `make install`.
- Bước 4: Cài Redis PHP Extension — tương tự tải bản phù hợp (redis-4.3.0.tgz cho PHP 5.6, redis-5.2.1.tgz cho PHP 7), cấu hình với flag `--enable-redis-igbinary`.
- Bước 5: Load module — tạo file `/etc/php.d/00-custom.ini` với nội dung `extension=igbinary.so` và `extension=redis.so`, sau đó restart php-fpm.
- Bước 6: Cấu hình Redis Cache — sửa file `/etc/redis/redis.conf` thêm `maxmemory 256mb`, `maxmemory-policy allkeys-lru`, `save ""`.
- Bước 7: Kiểm tra kết nối bằng `redis-cli ping` — kết quả trả về `PONG` là thành công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-redis-tren-centos-7/

**Hỏi: Cách kiểm tra các cổng (port) đang mở trong Linux như thế nào?**
Trả lời: Có 4 cách chính để kiểm tra port đang mở trên Linux:
- Lệnh `ss` (khuyến nghị, thay thế cho netstat đã lỗi thời): `ss -tl` xem kết nối TCP đang lắng nghe, `ss -lu` xem UDP đang lắng nghe, `ss -lntup` xem cả TCP/UDP kèm tên tiến trình.
- Lệnh `netstat`: dùng `netstat -pnlu` trong đó p = tên tiến trình/PID, n = số hiệu port, l = socket đang lắng nghe, t = kết nối TCP, u = kết nối UDP.
- Lệnh `lsof`: `lsof -i` xem socket đang mở, `lsof -n -P | grep LISTEN` lọc kết nối đang lắng nghe, `lsof -i tcp` hoặc `lsof -i udp` xem riêng TCP/UDP.
- Công cụ `nmap` (cần cài đặt qua package manager): `nmap -sT -O localhost` quét port TCP đang mở, `nmap -sU localhost` quét port UDP. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/kiem-tra-cac-cong-dang-mo-trong-linux/

**Hỏi: Cách hiển thị lượng RAM sử dụng trên Linux như thế nào?**
Trả lời: ps_mem là công cụ viết bằng Python dùng để hiển thị lượng RAM mà từng ứng dụng đang sử dụng trên hệ thống Linux. Cài đặt: trên CentOS chạy `yum install ps_mem -y`; trên Ubuntu chạy `sudo apt-get install python-pip -y` rồi `sudo pip install ps_mem`. Cách sử dụng:
- Xem mức tiêu thụ RAM của mỗi chương trình: `ps_mem`.
- Hiển thị đường dẫn đầy đủ: `ps_mem -s`.
- Kiểm tra RAM của một chương trình cụ thể theo PID: `ps_mem -p <PID>`.
- Giám sát liên tục, cập nhật mỗi 2 giây: `ps_mem w 2`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/hien-thi-luong-ram-su-dung-tren-linux/

**Hỏi: Cách kiểm tra phiên bản CentOS đang sử dụng như thế nào?**
Trả lời: Có 4 cách kiểm tra phiên bản CentOS:
- Lệnh RPM: `rpm --query centos-release`.
- File `/etc/centos-release`: chạy `cat /etc/centos-release`.
- File `/etc/os-release`: chạy `cat /etc/os-release` (hoạt động trên CentOS 7 trở lên có systemd).
- Lệnh `hostnamectl`: hiển thị thông tin hệ điều hành (CentOS 7 trở lên).
Phiên bản CentOS theo định dạng "X.Y" (ví dụ CentOS 7.9), trong đó X là phiên bản chính, Y là phiên bản phụ — mỗi phiên bản có chu kỳ hỗ trợ khác nhau, ảnh hưởng đến khả năng nhận cập nhật bảo mật. Việc kiểm tra phiên bản giúp cập nhật bảo mật phù hợp, kiểm tra tương thích phần mềm và lập kế hoạch nâng cấp kịp thời. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/kiem-tra-phien-ban-centos/

## Công cụ dòng lệnh và dịch vụ mạng

**Hỏi: Cách tạo tệp tin bằng dòng lệnh trên Linux như thế nào?**
Trả lời: Có 7 cách tạo tệp tin trên Linux qua dòng lệnh:
- Lệnh `touch`: tạo tệp trống, ví dụ `sudo touch touch.txt`.
- Lệnh `cat`: tạo tệp kèm nội dung bằng `cat > cat.txt`, gõ nội dung rồi lưu bằng Ctrl+D.
- Lệnh `echo`: tạo tệp với nội dung trực tiếp, ví dụ `echo "nội dung" > echo.txt`.
- Lệnh `printf`: tương tự echo, ví dụ `printf "nội dung" > printf.txt`.
- Trình soạn thảo `nano`: `nano nano.txt`, lưu bằng Ctrl+X rồi nhập y.
- Trình soạn thảo `vi`: `vi vi.txt`, nhấn `i` để vào chế độ chèn, `Esc` rồi `:wq` để lưu.
- Trình soạn thảo `vim`: `vim vim.txt`, thao tác tương tự vi.
Xem tệp đã tạo bằng lệnh `ls -l`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-tep-tin-bang-dong-lenh-tren-linux/

**Hỏi: Cách cài đặt TFTP Server trên CentOS 7 như thế nào?**
Trả lời: Các bước cài đặt TFTP Server:
- Bước 1: Cài đặt gói và dependency: `yum install tftp tftp-server xinetd`.
- Bước 2: Tắt SELinux — sửa `/etc/selinux/config` thành `SELINUX=disabled`, áp dụng ngay bằng `setenforce 0`.
- Bước 3: Tạo user và thư mục riêng cho TFTP: `useradd --no-create-home -s /sbin/nologin tftp`, `mkdir -p /tftpdata`, `chmod 777 /tftpdata`, `touch /tftpdata/demo.txt`, `chown tftp:tftp -R /tftpdata`.
- Bước 4: Cấu hình dịch vụ — sửa `/etc/xinetd.d/tftp` với các tham số: `-c` cho phép tạo file, `-s /tftpdata` đặt thư mục gốc, `-u tftp` user chạy dịch vụ, `-p` bỏ qua kiểm tra quyền, `-U 117` đặt umask, `-v` bật log chi tiết.
- Bước 5: Sửa file `/usr/lib/systemd/system/tftp.service` (dòng ExecStart) khớp với cấu hình xinetd, sau đó reload bằng `systemctl daemon-reload`.
- Bước 6: Khởi động dịch vụ: `systemctl start xinetd`, `systemctl start tftp`, `systemctl enable xinetd`, `systemctl enable tftp`.
- Bước 7: Xác nhận cổng UDP 69 đang lắng nghe: `netstat -antpu | grep 69`.
- Bước 8: Mở firewall: `iptables -I INPUT -p udp --dport 69 -j ACCEPT`.
- Bước 9: Kiểm tra kết nối bằng TFTP client: `tftp 192.168.1.42` rồi `get demo.txt`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-tftp-server-tren-centos-7/

**Hỏi: Cách tìm kiếm toàn bộ file .PHP trên Linux (kể cả nghi ngờ chứa mã độc) như thế nào?**
Trả lời: Các lệnh tìm kiếm file PHP trên hệ thống, hữu ích khi kiểm tra malware hoặc shell code lạ:
- Cách 1: `find . -print | grep -i '.*\[.\]php'`.
- Cách 2 (hiển thị kèm thời gian tạo/sửa, dễ xác định file nghi vấn): `find . -type f -name '*.php' -printf '%TY-%Tm-%Td %TT %p\n' | sort`.
- Tìm mã độc nâng cao (phát hiện các đoạn code sử dụng biến toàn cục đáng ngờ): `pcregrep -rlM '\<\?php.*\n.*\$GLOBALS' /home/domain.com/html/` trong đó `-r` tìm đệ quy trong thư mục con, `-l` chỉ hiển thị tên file, `-M` cho phép tìm kiếm nhiều dòng liên tiếp.
Nên kiểm tra định kỳ bằng các lệnh này để đảm bảo tính toàn vẹn của server. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tim-kiem-toan-bo-file-php-tren-linux/

**Hỏi: Cách tạo MySQL database và user bằng lệnh terminal như thế nào?**
Trả lời: Các bước tạo database và user MySQL qua dòng lệnh:
- Bước 1: Đăng nhập MySQL: `mysql -u root -p`.
- Bước 2: Tạo database: `create database dbname;`.
- Bước 3: Tạo user mới: `create user 'username'@'localhost' identified by 'password';`.
- Bước 4: Đổi mật khẩu user (nếu cần): `set password for 'username'@'localhost' = password('password');`.
- Bước 5: Cấp quyền cho user — toàn bộ quyền: `grant all on dbname.* to username@localhost;`; hoặc quyền cụ thể, ví dụ chỉ SELECT: `grant SELECT on dbname.* to username@localhost;`. Các quyền có thể cấp: ALL, ALTER, CREATE VIEW, CREATE, DELETE, DROP, GRANT OPTION, INDEX, INSERT, SELECT, SHOW VIEW, TRIGGER, UPDATE.
- Bước 6: Áp dụng thay đổi: `FLUSH PRIVILEGES;`.
- Bước 7: Thoát: `exit`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tao-mysql-database-va-user-bang-lenh-terminal/

**Hỏi: Cách cài đặt LEMP (Linux, Nginx, MariaDB, PHP) trên CentOS như thế nào?**
Trả lời: Các bước cài đặt LEMP Stack trên CentOS:
- Bước 1: Thêm các repository cần thiết — EPEL, Remi, và Nginx repo trước khi cài đặt.
- Bước 2: Cài Nginx và PHP — có thể chọn nhiều phiên bản PHP (5.3 đến 7.1) bằng flag enablerepo, lệnh khác nhau tùy phiên bản CentOS (5, 6, 7).
- Bước 3: Cài các module PHP cần thiết (ví dụ module gd cho xử lý đồ họa) qua yum với repo flag phù hợp.
- Bước 4: Cấu hình dịch vụ — dừng Apache (httpd), khởi động và bật Nginx cùng PHP-FPM, cấu hình worker processes và virtual host trong Nginx, đổi user/group của PHP-FPM từ apache sang nginx.
- Bước 5: Cài đặt MariaDB — chạy `mysql_secure_installation` để đặt mật khẩu root, xóa user ẩn danh, tắt remote root access.
- Bước 6: Kiểm tra — tạo file PHP info tại `/usr/share/nginx/html/info.php` để xác nhận cấu hình đúng.
Hướng dẫn áp dụng cho cả systemctl (CentOS 7) và lệnh service (CentOS 6/5). Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-lemp-linux-nginx-mariadb-php-tren-centos/

**Hỏi: Cách cài đặt LAMP trên CentOS 7 như thế nào?**
Trả lời: LAMP là hệ thống phần mềm tạo môi trường máy chủ web, gồm Linux (hệ điều hành), Apache (web server), MySQL/MariaDB (database), PHP (ngôn ngữ script). Các bước cài đặt:
- Cài Apache: `sudo yum -y install httpd`, khởi động `systemctl start httpd`, bật tự khởi động `systemctl enable httpd`, kiểm tra bằng cách truy cập IP server trên trình duyệt.
- Cài MariaDB: `yum -y install mariadb mariadb-server`, khởi động `systemctl start mariadb`, bảo mật cài đặt bằng `mysql_secure_installation`, bật tự khởi động `systemctl enable mariadb`.
- Cấu hình PHP: thêm repo Remi bằng `rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-7.rpm`, cài `yum-utils`, bật phiên bản PHP mong muốn (7.0-7.3), ví dụ PHP 7.2 dùng `yum-config-manager --enable remi-php72`, cài đặt bằng `yum -y install php php-opcache php-mysql`, khởi động lại Apache bằng `systemctl restart httpd`.
- Kiểm tra: tạo file test `echo "<?php phpinfo();?>" > /var/www/html/info.php` rồi truy cập `<ip-server>/info.php` trên trình duyệt để xác nhận. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-lamp-tren-centos-7/

**Hỏi: Cách sử dụng Netstat để quản lý và tra cứu thông tin kết nối mạng trên Server như thế nào?**
Trả lời: Cài đặt netstat: trên CentOS/RHEL tìm gói bằng `yum whatprovides */netstat` rồi cài `yum install net-tools -y`; trên Ubuntu cài bằng `sudo apt-get install net-tools`. Các tùy chọn quan trọng: `-l` hiển thị socket đang lắng nghe, `-t` kết nối TCP, `-u` kết nối UDP, `-n` hiển thị địa chỉ dạng số (không resolve DNS), `-p` hiển thị chương trình/PID liên quan. Các lệnh thực tế thường dùng:
- Kiểm tra tiến trình đang lắng nghe trên port: `netstat -ltnp` hoặc lọc theo port cụ thể `netstat -ltnp | grep -w ':22'`.
- Xem toàn bộ port đang mở: `netstat -tulpn`.
- Hiển thị tất cả loại socket: `netstat -a`.
- Xem thống kê mạng: `netstat -s`.
- Xem thông tin định tuyến: `netstat -r`.
- Phân tích số kết nối đến 1 port cụ thể (ví dụ 443) theo từng IP: kết hợp `netstat -tn` với grep, awk, sort để lọc và đếm kết nối theo địa chỉ IP. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/su-dung-netstat-quan-ly-tra-cuu-thong-tin-ket-noi-mang-tren-server/

**Hỏi: Cách sử dụng Cron/Crontab để tự động chạy script trên Server Linux như thế nào?**
Trả lời: Crontab tương tự Task Scheduler trên Windows, cho phép chạy tác vụ theo lịch biểu (phút/giờ/ngày/tuần/tháng) dựa vào đồng hồ hệ thống, dùng cho việc như xóa log cũ hoặc cập nhật phần mềm tự động. Kiểm tra trạng thái dịch vụ: CentOS dùng `systemctl status crond`, Ubuntu dùng `systemctl status cron`. Quản lý crontab: liệt kê tác vụ hiện tại bằng `crontab -l`, biên tập bằng `crontab -e`. Định dạng crontab gồm 6 cột: `[phút] [giờ] [ngày] [tháng] [thứ] [lệnh/script]` — phút (0-59), giờ (0-23), ngày (1-31), tháng (1-12), thứ (0-7, 0/7=Chủ nhật), dùng `*` để chỉ "tất cả". Ví dụ thường dùng: `0 3 * * * /script/abc.sh` chạy 3h sáng hàng ngày; `*/30 * * * * /script/abc.sh` chạy mỗi 30 phút; `0 */8 * * * /scripts/abc.sh` chạy mỗi 8 giờ; `@daily`, `@weekly`, `@monthly` cho các lịch chạy định kỳ tương ứng. Ứng dụng thực tế: kiểm tra/sửa database MySQL lúc 3h sáng bằng `0 3 * * * /repairdb.sh`, hoặc chạy script PHP mỗi 30 phút bằng `*/30 * * * * php /home/user/run30.php`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/su-dung-cron-crontab-tu-dong-chay-script-tren-server-linux/

**Hỏi: Cách thay đổi Hostname trong Linux như thế nào?**
Trả lời: Trên CentOS:
- Bước 1: Kiểm tra hostname hiện tại bằng `hostname`.
- Bước 2: Đặt hostname mới: `hostname "tgs.com"`.
- Bước 3: Sửa file `/etc/sysconfig/network`, đặt `NETWORKING=yes` và `HOSTNAME=tgs.com`.
- Bước 4: Sửa file `/etc/hosts` — đổi block cuối thành `127.0.0.1 tgs.com` và `::1 tgs.com`, thêm dòng cuối với IP VPS và hostname, ví dụ `xxx.xxx.xxx.xxx tgs.com thegioiso`.
- Bước 5: Khởi động lại server bằng `reboot`, kiểm tra lại bằng `hostname`.
Trên Ubuntu: kiểm tra bằng `hostname`, đổi bằng `hostname tgs.com`, sửa file `/etc/hostname` và `/etc/hosts` tương tự CentOS. Lưu ý: từ Ubuntu 14.04 trở lên có thể dùng lệnh đơn giản `hostnamectl set-hostname thegioiso`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-thay-doi-hostname-trong-linux/

## Tiện ích và thông tin hệ thống khác

**Hỏi: Cách cài đặt 7zip trên CentOS 7 như thế nào?**
Trả lời: 7zip là ứng dụng mã nguồn mở giúp nén và giải nén file, hỗ trợ nhiều định dạng như 7z, ZIP, RAR, TAR, GZIP. 7zip không có sẵn trong repository mặc định của CentOS 7 nên cần thêm repo ngoài. Các bước cài đặt:
- Bước 1: Cài `unzip` bằng `yum install -y unzip`.
- Bước 2: Tải và cài repo rpmforge: `wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.i686.rpm` rồi `rpm -ivh rpmforge-release-0.5.2-2.el6.rf.i686.rpm`.
- Bước 3: Giải nén file: `7za x [tên-file-7z]`.
- Bước 4: Nén file: `7za a -mx=9 tenfile.7z [tên-file/thư-mục]`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cai-dat-7zip-tren-centos-7/

**Hỏi: Cách xem file log trên VPS như thế nào?**
Trả lời: Tất cả file log trên server Linux được lưu tại thư mục `/var/log` (truy cập bằng `cd /var/log/`, liệt kê bằng `ls -l`). Các file log quan trọng:
- `auth.log`: log về xác thực.
- `boot.log`: log hoạt động trong quá trình khởi động hệ thống.
- `cron`: log các lịch hoạt động tự động.
- `dmesg`: log bộ đệm hệ thống.
- `message`: log thông tin chung của hệ thống.
- `httpd/`: thư mục chứa log Apache.
- `maillog`: log hoạt động mail trên máy chủ.
- `secure`: log bảo mật.
- `wtmp`: log đăng nhập.
- `yum.log`: log của Yum.
Xem file log bằng lệnh `more -f /var/log/secure` hoặc `tail -n 30 /var/log/secure` (xem 30 dòng gần nhất). Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cach-xem-file-log-tren-vps/

**Hỏi: Có những bản hệ điều hành (OS) Windows Server và Linux nào phổ biến?**
Trả lời: Về Windows Server, các phiên bản chính qua các thời kỳ gồm: dòng Windows NT (3.1 đến 4.0), Windows 2000 (nhiều bản), Windows Server 2003, 2008, 2012, 2016 và 2019; ngoài ra còn có các biến thể chuyên biệt như Windows Home Server, Windows Storage Server, Windows HPC Server. Về Linux, có rất nhiều bản phân phối (distro) khác nhau do tính chất mã nguồn mở, các bản phổ biến gồm: Ubuntu (và các biến thể như Kubuntu, Lubuntu), Debian GNU/Linux, CentOS, Fedora, Linux Mint, Kali Linux. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/thong-tin-cac-ban-he-dieu-hanh-os-windows-va-linux/

**Hỏi: Có những phương pháp nào để cài đặt Linux an toàn, đơn giản trên máy tính Windows?**
Trả lời: 5 phương pháp cài đặt/trải nghiệm Linux trên máy tính đang chạy Windows:
- Khởi chạy trực tiếp bằng CD/DVD: tải file ISO của Linux, ghi vào đĩa (trên Windows Vista/7/8 trở lên chỉ cần chuột phải vào file ISO chọn "Burn Disc Image"). Ưu điểm: an toàn tuyệt đối với Windows hiện tại; nhược điểm: hiệu suất chưa tối ưu.
- Phương pháp USB: dùng công cụ Linux Live USB Creator (LiLi) để tạo USB khởi động Linux, không cần ổ đĩa quang, USB có thể tái sử dụng nhiều lần; cần chỉnh BIOS để khởi động từ USB.
- Cài đặt từ Windows bằng Wubi: tải Wubi, chọn ổ cứng cài đặt, username/password và phiên bản Ubuntu — sau khi cài, Ubuntu xuất hiện trong menu khởi động cùng Windows; nhược điểm là một số tính năng bị hạn chế.
- Cài đặt máy ảo: dùng phần mềm VirtualBox để chạy Ubuntu trong cửa sổ ngay trên Windows, dễ dàng chuyển đổi qua lại giữa 2 hệ điều hành, không ảnh hưởng ổ đĩa gốc; nhược điểm là tốc độ chậm hơn cách khác.
- Dùng thử online: trải nghiệm Ubuntu ngay qua trình duyệt web mà không cần cài đặt gì, tương tự máy ảo trực tuyến. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/5-phuong-phap-cai-dat-linux-an-toan-va-don-gian-tren-may-tinh-windows/

**Hỏi: Cách xem cấu hình máy (hardware) trên Linux như thế nào?**
Trả lời: Dùng công cụ `lshw` (tương tự CPU-Z trên Windows) để xem đầy đủ thông tin cấu hình phần cứng. Các bước:
- Bước 1: Tải lshw, ví dụ trên CentOS: `wget http://pkgs.repoforge.org/lshw/lshw-2.14-1.el5.rf.x86_64.rpm`.
- Bước 2: Cài đặt: `rpm -ivh lshw-2.14-1.el5.rf.x86_64.rpm`.
- Bước 3: Chạy công cụ: `lshw`.
Công cụ này hiển thị thông tin chính xác, đầy đủ về hardware và firmware BIOS, loại RAM, số khe RAM trống, HDD, CPU và nhiều thông số khác — bao gồm mainboard, BIOS, CPU, cache, bộ nhớ RAM, card mạng, ổ cứng và các thiết bị khác. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/xem-cau-hinh-may-tren-linux/

**Hỏi: Cách đặt IP tĩnh trong CentOS 6 như thế nào?**
Trả lời: IP tĩnh (IP Static) là địa chỉ IP cố định gán cho thiết bị như máy chủ hoặc router. Các bước cấu hình trên CentOS 6:
- Bước 1: Sửa file cấu hình mạng bằng lệnh `vi /etc/sysconfig/network-scripts/ifcfg-eth0`, nhập các thông số: `BOOTPROTO=none`, `IPADDR=192.168.80.100` (IP mong muốn), `NETMASK=255.255.255.0`, `GATEWAY=192.168.80.254`, `DNS1=192.168.80.100`, `DNS2=8.8.8.8`.
- Bước 2: Khởi động lại dịch vụ mạng: `/etc/rc.d/init.d/network restart`.
- Bước 3: Kiểm tra kết quả bằng `ifconfig eth0`.
Các lỗi thường gặp: xung đột IP (địa chỉ trùng lặp trong mạng), cấu hình sai (lỗi ở IP/subnet mask/gateway/DNS), dịch vụ không khởi động lại (thay đổi chưa có hiệu lực), phân giải tên miền lỗi (cấu hình DNS không chính xác). Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/dat-ip-tinh-trong-centos-6/

**Hỏi: Cách cho phép đăng nhập (login) bằng root qua SSH trên Debian 9 như thế nào?**
Trả lời: Mặc định Debian 9 không cho phép login bằng root. Cách bật lại:
- Bước 1: Mở file cấu hình SSH bằng nano: `nano /etc/ssh/sshd_config`.
- Bước 2: Tìm dòng "PermitRootLogin no" và đổi thành "PermitRootLogin yes".
- Bước 3: Lưu file (Ctrl+X rồi nhấn Y), khởi động lại dịch vụ SSH: `systemctl restart sshd`.
Sau đó có thể đăng nhập bằng tài khoản root qua SSH. Lưu ý: cho phép đăng nhập trực tiếp bằng root có thể ảnh hưởng đến bảo mật hệ thống, nên cân nhắc kỹ trước khi áp dụng. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cho-phep-login-root-tren-debian-9/

**Hỏi: Cách xem cấu hình phần cứng máy chủ trên Linux bằng lshw như thế nào?**
Trả lời: Các bước kiểm tra thông tin phần cứng bằng công cụ lshw trên CentOS:
- Bước 1: Tải công cụ lshw, ví dụ: `wget https://ftp.tu-chemnitz.de/pub/linux/dag/redhat/el6/en/x86_64/rpmforge/RPMS/lshw-2.15-1.el6.rf.x86_64.rpm`.
- Bước 2: Cài đặt: `rpm -ivh lshw-2.15-1.el6.rf.x86_64.rpm`.
- Bước 3: Xem thông tin cấu hình bằng lệnh `lshw` hoặc `lshw |more`.
Công cụ này cung cấp đầy đủ thông tin phần cứng gồm BIOS, RAM (loại RAM, số khe trống), ổ đĩa cứng (HDD) và CPU. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/xem-cau-hinh-may-tren-linux-2/

**Hỏi: Cách xem file log trên VPS/Server Linux như thế nào?**
Trả lời: Toàn bộ log file của server được lưu trong thư mục `/var/log`, truy cập bằng lệnh `cd /var/log/`. Các file log chính (xem danh sách bằng `ls -l`):
- auth.log: thông tin xác thực người dùng
- boot.log: hoạt động khởi động hệ thống
- cron: các tác vụ tự động được lên lịch
- dmesg: nhật ký bộ đệm hệ thống
- message: thông tin chung của hệ thống
- httpd/: thư mục chứa log Apache
- maillog: hoạt động dịch vụ mail
- secure: các sự kiện bảo mật
- wtmp: ghi nhận đăng nhập
- yum.log: thao tác quản lý gói Yum
Cách xem file log: `more -f /var/log/secure` hoặc `tail -n 30 /var/log/secure` (lệnh tail giúp xem các dòng gần nhất, ví dụ 30 dòng cuối). Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cach-xem-file-log-tren-vps/
