# Hướng dẫn sử dụng Plesk, DirectAdmin và IIS

> **Danh mục:** Hướng dẫn kỹ thuật - Plesk / DirectAdmin / IIS
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** plesk, directadmin, iis, ssl, lets encrypt, opcache, rewrite, migration, selinux, memcached, mysql, mariadb, .net core

## Cấu hình và sử dụng Plesk

**Hỏi: Cách cài đặt chứng chỉ SSL cho Domain trên Plesk như thế nào?**
Trả lời: Để cài đặt SSL certificate trên Plesk:
- Bước 1: Đăng nhập vào Plesk Panel bằng tài khoản quản trị và chọn domain cần cài đặt SSL.
- Bước 2: Truy cập mục "SSL/TLS Certificates" trong giao diện quản trị domain.
- Bước 3: Nhấp nút "Manage" sau đó "Add SSL/TLS Certificate" để bắt đầu.
- Bước 4: Điền thông tin gồm tên chứng chỉ, số bit (thường 2048), loại certificate (Self-signed hoặc CSR), và dữ liệu khóa/chứng chỉ nếu có.
- Bước 5: Lưu bằng nút "Request" hoặc "Upload" tùy loại, rồi tải lên Certificate và CA Certificate.
- Bước 6: Xác nhận trong Hosting Settings và kiểm tra qua HTTPS — không gặp lỗi là dấu hiệu SSL đã hoạt động. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-chung-chi-ssl-cho-domain-tren-plesk/

**Hỏi: Cách upload dữ liệu website và database lên Plesk Panel như thế nào?**
Trả lời: Đăng nhập bằng thông tin hosting được cấp để vào giao diện quản trị. Có 2 cách upload file website:
- Dùng File Manager của Plesk: vào File Manager, nhấn Upload, tải file từ máy lên, sau đó dùng "Extract Files" để giải nén nếu là file nén.
- Dùng tài khoản FTP: với phần mềm như FileZilla Client, kết nối qua FTP và upload dữ liệu trực tiếp vào thư mục "httpdocs" trên hosting.
Tạo database: từ trang chủ chọn "Database", sau đó "Add Database", nhập thông tin cần thiết để tạo database mới.
Import dữ liệu database (file phải ở định dạng .sql): dùng phpMyAdmin (chọn database, dùng chức năng Import để tải file SQL lên) hoặc công cụ Import Dump Tool (cho phép import file đã upload sẵn hoặc upload trực tiếp từ thiết bị). Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-upload-du-lieu-website-va-database-tren-plesk-panel/

**Hỏi: Cách cấu hình Opcache cho Domain trên Plesk như thế nào?**
Trả lời: Để kích hoạt và cấu hình Opcache cho một tên miền trên Plesk:
- Bước 1: Đăng nhập vào tài khoản hosting Plesk.
- Bước 2: Truy cập "Domains" > chọn tên miền > tab "Dashboard" > chọn "PHP" để xem phiên bản PHP đang dùng.
- Bước 3: Kiểm tra cài đặt Opcache — xác nhận "opcache.enable" được đặt thành "on", đảm bảo "disable_functions" không chứa các hàm Opcache (mặc định `opcache_get_status` bị tắt).
Lưu ý: để vô hiệu hóa Opcache cho tên miền, chuyển "opcache.enable" thành "off". Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-opcache-on-domain-plesk/

**Hỏi: Cách cấu hình rewrite cho tên miền trên Plesk như thế nào?**
Trả lời: Để cấu hình URL rewrite cho tên miền trên Plesk:
- Bước 1: Truy cập vào bộ điều khiển quản lý, điều hướng đến mục "Domains" (Tên miền).
- Bước 2: Chọn tên miền tương ứng.
- Bước 3: Truy cập tab "Hosting & DNS".
- Bước 4: Vào "Apache and Nginx Settings" để cấu hình rewrite — Apache dùng công cụ cấu hình Apache, Nginx dùng công cụ cấu hình Nginx tương ứng. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-rewrite/

**Hỏi: Lỗi không thể Migration trên Plesk Linux, nguyên nhân và cách khắc phục?**
Trả lời: Lỗi này thường xuất hiện khi máy chủ hoạt động ở chế độ "Enforcing" của SELinux, với thông báo lỗi "rsync: ERROR: cannot stat destination... Permission denied (13)", khiến quá trình sao chép dữ liệu giữa các máy chủ bị chặn. Cách khắc phục:
- Bước 1: Kiểm tra trạng thái SELinux bằng lệnh `getenforce`.
- Bước 2: Chọn 1 trong 2 cách xử lý:
  - Cách 1 - Tắt SELinux: chỉnh sửa file `/etc/selinux/config`, đổi `SELINUX=enforcing` thành `SELINUX=disable`, lưu lại.
  - Cách 2 - Cấp quyền cho rsync: chạy `setsebool -P rsync_full_access=1`, xác nhận bằng `getsebool -a | grep rsync_full_access` — nếu hiển thị "on" là thành công.
- Bước 3: Quay lại Plesk và chạy lại quá trình Migration. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/loi-khong-the-migration-tren-plesk-linux/

**Hỏi: Cách backup toàn bộ hosting trên Plesk như thế nào?**
Trả lời: Để tạo bản sao lưu đầy đủ dữ liệu hosting trên Plesk:
- Bước 1: Đăng nhập vào giao diện quản lý Plesk.
- Bước 2: Tìm mục "Backup Manager" trên giao diện chính.
- Bước 3: Nhấp vào tùy chọn "Backup".
- Bước 4: Chọn "Backup Full" để sao lưu toàn bộ dữ liệu hosting.
- Bước 5: Nhấn "Ok" để bắt đầu quá trình sao lưu.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-backup-hosting-plesk/

**Hỏi: Cách nâng cấp MySQL 5.5 lên 5.6/5.7 hoặc MariaDB 5.5 lên 10.x trên Linux như thế nào?**
Trả lời: Để giải quyết các lỗi phát sinh từ phiên bản MySQL/MariaDB cũ (ví dụ Migration trên Plesk bị lỗi không nhận database):
- Chuẩn bị: sao lưu toàn bộ dữ liệu trước khi nâng cấp.
- Bước 1: Dừng dịch vụ MariaDB bằng lệnh `service mariadb stop`.
- Bước 2: Xoá gói bổ sung: `rpm -e --nodeps mariadb-bench`.
- Bước 3: Sao chép cơ sở dữ liệu sang vị trí an toàn.
- Bước 4: Kiểm tra và xoá các gói MySQL cũ nếu có.
- Bước 5: Tạo file repository MariaDB tại `/etc/yum.repos.d/MariaDB.repo` với cấu hình kho lưu trữ phù hợp.
- Bước 6: Cài đặt các gói mới: MariaDB-client, MariaDB-server, MariaDB-compat, MariaDB-shared.
- Bước 7: Khởi động lại MariaDB.
- Bước 8: Nâng cấp cơ sở dữ liệu bằng lệnh `mysql_upgrade`.
- Bước 9: Khởi động lại dịch vụ.
- Bước 10: Với Plesk, chạy thêm lệnh `plesk sbin packagemng -sdf` để cập nhật.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-nang-cap-mysql-5-5-len-5-6-5-7-hoac-mariadb-5-5-len-10-x-tren-linux/

**Hỏi: Cách cài đặt Plesk Panel trên Windows Server như thế nào?**
Trả lời: Cài Plesk Panel giúp đơn giản hoá quản lý máy chủ, tăng cường bảo mật và tự động hoá các tác vụ quản trị thường ngày trên Windows Server. Trước khi cài, cần kiểm tra hệ thống đáp ứng yêu cầu của Plesk, đảm bảo port 8443 không bị chặn bởi firewall, và tắt các dịch vụ web đang xung đột. Các bước cài đặt:
- Bước 1: Tải trình cài đặt từ trang cài đặt tự động.
- Bước 2: Chạy file `plesk-installer.exe`.
- Bước 3: Truy cập giao diện thiết lập qua trình duyệt tại `localhost:8447` và đăng nhập bằng tài khoản quản trị.
- Bước 4: Chọn "Install or Upgrade Product".
- Bước 5: Chọn các gói cài đặt cần thiết rồi tiếp tục.
- Bước 6: Đặt mật khẩu cho tài khoản admin Plesk.
- Bước 7: Hoàn tất cài đặt (mất khoảng 5-10 phút).
Sau khi cài đặt: đổi mật khẩu admin ngay lập tức, cấu hình firewall, lên lịch backup định kỳ, và luôn cập nhật phần mềm với các bản vá bảo mật mới nhất. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cai-dat-plesk-panel-tren-windows-server/

**Hỏi: Cách hiển thị lỗi PHP trên Plesk hosting như thế nào?**
Trả lời: Để bật hiển thị lỗi PHP trên hosting Plesk, giúp dễ dàng xác định và khắc phục lỗi mã nguồn website:
- Bước 1: Truy cập bảng điều khiển Plesk, chọn Subscription cần hiển thị lỗi, sau đó chọn mục "PHP setting".
- Bước 2: Trong cài đặt PHP, tìm và bật tùy chọn "Display_errors".
- Bước 3: Nhấn OK để lưu thay đổi. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-hien-loi-php-tren-plesk-hosting/

**Hỏi: Cách tăng giới hạn dung lượng upload file trên hosting như thế nào?**
Trả lời: Để tăng dung lượng file tối đa được phép upload lên hosting:
- Bước 1: Đăng nhập vào hosting control panel, tìm mục "PHP Setting".
- Bước 2: Tìm tùy chọn giới hạn upload file và điều chỉnh lên mức dung lượng mong muốn. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tang-upload-file-tren-hosting/

**Hỏi: Cách cài đặt SSL Let'''s Encrypt trên Plesk Panel như thế nào?**
Trả lời: Để cài đặt và kích hoạt chứng chỉ SSL Let'''s Encrypt (miễn phí, loại Domain Validation) trên Plesk Panel:
- Bước 1: Đăng nhập vào Plesk Panel bằng thông tin quản trị hosting.
- Bước 2: Chọn tab "Website & Domain", nhấn "show more" để hiển thị thêm tính năng.
- Bước 3: Nhấn vào biểu tượng "Let'''s Encrypt" để bắt đầu cài đặt, điền thông tin yêu cầu — sau khi hoàn tất, chứng chỉ được cài nhưng chưa kích hoạt.
- Bước 4: Để kích hoạt, vào "Hosting settings" → chọn "SSL/TLS support" → chọn Let'''s Encrypt làm Certificate.
- Bước 5: Kiểm tra bằng cách truy cập website qua "https://" — nếu thấy biểu tượng ổ khoá màu xanh trên thanh địa chỉ là đã kích hoạt thành công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-ssl-lets-encrypt-tren-plesk-panel/

**Hỏi: Cách cài đặt WordPress trên Plesk như thế nào?**
Trả lời: Quy trình 2 bước đơn giản để cài WordPress trên Plesk Hosting:
- Bước 1: Đăng nhập vào Plesk Hosting bằng thông tin nhà cung cấp gửi (ví dụ dạng http://tenmien:8880), sau đó chọn tùy chọn cài đặt WordPress.
- Bước 2: Sau khi quá trình tự động cài đặt hoàn tất, đăng nhập vào trang quản trị WordPress (site admin).
Lưu ý quan trọng: tên miền phải được trỏ đúng về địa chỉ IP của hosting trước khi có thể truy cập trang quản trị WordPress. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-wordpress-tren-plesk/

## DirectAdmin

**Hỏi: Cách tùy chỉnh phiên bản PHP cho domain trên DirectAdmin như thế nào?**
Trả lời: Để thay đổi phiên bản PHP cho từng domain riêng biệt trên DirectAdmin (áp dụng cho giao diện cũ Enhanced):
- Bước 1: Đăng nhập tài khoản User cần thay đổi phiên bản PHP.
- Bước 2: Sau khi vào User level, chọn "Domain Setup" từ giao diện chính.
- Bước 3: Trong danh sách domain, chọn domain muốn thay đổi phiên bản PHP.
- Bước 4: Tại mục "PHP Version Selector", chọn phiên bản PHP mong muốn từ drop-down list các phiên bản đã cài sẵn.
- Bước 5: Nhấn "Save" để lưu cấu hình — thay đổi sẽ áp dụng trong khoảng 1 phút.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tuy-chinh-phien-ban-php-cho-domain-tren-directadmin/

**Hỏi: Cách cài đặt Let's Encrypt cho domain trên DirectAdmin như thế nào?**
Trả lời: Để thiết lập chứng chỉ SSL Let's Encrypt bảo mật kết nối HTTPS cho domain trên DirectAdmin:
- Bước 1: Đảm bảo domain (bao gồm cả phiên bản www) đã trỏ về đúng IP của hệ thống DirectAdmin.
- Bước 2: Trong giao diện DirectAdmin, chọn "SSL Certificates", nhập Common Name (domain cần cài SSL), chọn Key Size (2048 hoặc 4096 bits), chọn Certificate Type là SHA256, và bật "Force SSL" để bắt buộc sử dụng HTTPS khi truy cập tên miền. Nhấn Save để hoàn thành.
- Bước 3: Quay về Home, kiểm tra mục SSL Certificates để xác nhận SSL đã được bật cho tên miền này.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-lets-encrypt-cho-domain-tren-directadmin/

**Hỏi: Cách xử lý lỗi DST Root CA X3 hết hạn của Let's Encrypt như thế nào?**
Trả lời: Từ ngày 30/09/2021, chứng chỉ DST Root CA X3 trong chuỗi tin cậy của Let's Encrypt đã hết hạn, gây lỗi xác thực bảo mật trên các thiết bị/hệ thống cũ chưa nhận diện được chứng chỉ mới ISRG Root X1. Các hệ thống bị ảnh hưởng: Windows trước XP SP3, Windows 7 (chưa cập nhật chứng chỉ), macOS trước 10.12.1, iOS dưới bản 10, Android từ 2.3.6 đến 7.1.0, Ubuntu/Debian cũ hơn 16.04/8. Ba cách khắc phục:
- Cách 1 - Cập nhật trình duyệt: Firefox tự quản lý kho chứng chỉ riêng nên cập nhật Firefox sẽ giải quyết được lỗi; các trình duyệt khác (Chrome, Safari, Edge, Opera) phụ thuộc vào chứng chỉ hệ thống nên cần khắc phục ở cấp hệ điều hành.
- Cách 2 - Cập nhật hệ thống: Windows truy cập `https://valid-isrgrootx1.letsencrypt.org/` để tự động cài ISRG Root X1; macOS/iOS khởi động lại thiết bị và thử lại; CentOS/Linux chạy `yum update -y`; Windows Server chạy Windows Update thông thường.
- Cách 3 - Mua chứng chỉ SSL trả phí: giải pháp lâu dài, có thể mua từ nhiều nhà cung cấp khác nhau.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cach-xu-ly-dst-root-ca-x3-het-han-va-letsencrypt/

**Hỏi: Cách cài Memcached cho Direct Admin như thế nào?**
Trả lời: Để cài đặt Memcached module (phiên bản 3) trên Direct Admin, giúp tăng tốc độ tải trang, đặc biệt cho ứng dụng chạy PHP 7:
- Bước 1: Tải xuống Memcached module từ pecl.php.net và thư viện hỗ trợ bắt buộc libmemcached.
- Bước 2: Cài đặt Libmemcached — giải nén, chuyển vào thư mục, chạy `./configure` rồi `make && make install`.
- Bước 3: Cài đặt Memcached module — tương tự, chạy `phpize`, `./configure`, `make && make install`.
- Bước 4: Cấu hình PHP — tìm file php.ini bằng lệnh `php -i | grep php.ini`, thêm extension memcached vào cuối tệp, sau đó khởi động lại Apache.
- Bước 5: Cài đặt memcached service bằng `yum install memcached -y`, khởi động dịch vụ và kích hoạt tự động khởi động.
- Bước 6: Kiểm tra hoạt động bằng cách tạo file PHP test hoặc dùng telnet để xác minh memcached đang lưu trữ dữ liệu đúng cách.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-memcached-cho-direct-admin/

## IIS

**Hỏi: Cách chạy .NET Core trên IIS như thế nào?**
Trả lời: Để cấu hình và chạy ứng dụng .NET Core trên máy chủ IIS (Internet Information Services):
- Bước 1: Cài đặt .NET Core vào hệ thống.
- Bước 2: Vào IIS Manager, chọn Site cần cấu hình, truy cập "Handler Mappings", thêm Script handler mới hoặc chọn Script handler có sẵn phù hợp.
Lưu ý: đây là hướng dẫn cơ bản, có thể cần thêm các bước bổ sung tùy theo phiên bản .NET Core và cấu hình máy chủ cụ thể. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-chay-net-core-tren-iis/

**Hỏi: Cách xử lý lỗi "No space left on device" khi start Apache trên DirectAdmin như thế nào?**
Trả lời: Lỗi `(28)No space left on device: AH02478: failed to create proxy mutex` xảy ra khi máy chủ hết semaphores. Cách xử lý:
- Bước 1: Kiểm tra lỗi Apache bằng lệnh `tail /var/log/httpd/error_log`.
- Bước 2: Xem semaphores đang sử dụng bằng lệnh `ipcs -s`.
- Bước 3: Xóa các semaphores — cách 1: `for whatever in \`ipcs -s | awk '{print $2}'\`; do ipcrm -s $whatever; done`; cách 2 (nếu cách 1 không hiệu quả): dừng httpd (`/etc/init.d/httpd stop`), xóa semaphores của user nobody (`ipcs -s | grep nobody | gawk '{ print $2 }' | xargs -n 1 ipcrm sem`), rồi khởi động lại httpd (`/etc/init.d/httpd start`).
- Bước 4: Tăng giới hạn semaphore vĩnh viễn — thêm vào file `/etc/sysctl.conf`: `kernel.msgmni = 512` và `kernel.sem = 250 128000 32 512`, sau đó chạy `sysctl -p` để áp dụng.
Các bước này sẽ khôi phục khả năng khởi động Apache. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-xu-ly-loi-khi-start-apache-direct-admin-no-space-left-on-device/

**Hỏi: Cách cài đặt Memcached cho DirectAdmin như thế nào?**
Trả lời: Hướng dẫn cài Memcached phiên bản 3 (hỗ trợ PHP 7.0-7.1) trên DirectAdmin:
- Bước 1: Tải tài nguyên libmemcached-1.0.18.tar.gz và memcached-3.0.4.tgz từ nguồn chính thức.
- Bước 2: Cài libmemcached — giải nén, configure, biên dịch thư viện bằng `make && make install`.
- Bước 3: Cài Memcached Module — biên dịch module PHP bằng phpize và các lệnh tiêu chuẩn.
- Bước 4: Cấu hình PHP — thêm đường dẫn extension vào file php.ini: `extension = "/usr/local/lib/php/extensions/.../memcached.so"`.
- Bước 5: Khởi động dịch vụ — cài memcached service qua yum, khởi động và bật chế độ tự động.
- Bước 6: Kiểm tra hoạt động — dùng script PHP tạo key-value hoặc telnet để xác minh kết nối port 11211.
Lợi ích: cải thiện tốc độ tải trang khi kết hợp với plugin caching như W3 Total Cache. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-memcached-cho-direct-admin/

**Hỏi: Cách upgrade và downgrade PHP trong VestaCP như thế nào?**
Trả lời: Các bước thay đổi phiên bản PHP trên VestaCP (upgrade lên PHP 7.2 hoặc downgrade xuống phiên bản cũ hơn):
- Upgrade PHP: cài đặt EPEL và Remi Repository bằng wget rồi cập nhật gói bằng `rpm -Uvh`; cài yum-utils để quản lý repositories; kích hoạt repository PHP bằng lệnh enable remi-php72; chạy `yum update -y` rồi restart httpd (Apache) hoặc nginx/php-fpm; kiểm tra bằng `php -v`.
- Downgrade PHP: liệt kê module PHP hiện tại bằng `yum list installed | grep php`; xóa PHP cũ bằng `yum remove php-*`; enable remi-php72 và cài lại php; cài thêm các module bổ sung (php-fpm, php-gd, php-json, php-mbstring...).
Lưu ý: hệ thống tự động sao lưu file php.ini và cấu hình pool, cho phép khôi phục sau khi nâng cấp. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-upgrade-va-downgrade-php-trong-vestacp/

**Hỏi: Cách tạo Reseller và Hosting trên DirectAdmin (VPS Linux) như thế nào?**
Trả lời: Quy trình: tạo gói reseller → tạo reseller → tạo gói hosting → tạo user (hosting). Các bước:
- Bước 1: Truy cập giao diện quản trị DirectAdmin bằng thông tin admin được cung cấp khi mua VPS.
- Bước 2: Tạo gói reseller bằng cách nhập các thông số theo nhu cầu.
- Bước 3: Tạo tài khoản reseller với đầy đủ thông tin (tên người dùng, email).
- Bước 4: Đăng nhập vào tài khoản reseller vừa tạo, tạo gói hosting với thông số tương ứng.
- Bước 5: Tạo tài khoản người dùng (hosting) từ gói hosting đã thiết lập để sử dụng dịch vụ. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-reseller-hosting-tren-directadmin-vps-linux/

**Hỏi: Cách tùy chỉnh phiên bản PHP cho domain trên DirectAdmin như thế nào?**
Trả lời: Với giao diện Enhanced (cũ):
- Bước 1: Đăng nhập vào tài khoản User cần thay đổi phiên bản PHP.
- Bước 2: Truy cập mục "Domain Setup".
- Bước 3: Chọn domain muốn thay đổi phiên bản PHP.
- Bước 4: Tìm phần "PHP Version Selector" — danh sách thả xuống chứa các phiên bản PHP có sẵn trên máy chủ.
- Bước 5: Chọn phiên bản PHP mong muốn.
- Bước 6: Nhấn "Save" để lưu thay đổi.
Lưu ý: cài đặt có hiệu lực khoảng 1 phút sau khi lưu. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tuy-chinh-phien-ban-php-cho-domain-tren-directadmin/

**Hỏi: Cách cài đặt Let's Encrypt cho domain trên DirectAdmin như thế nào?**
Trả lời: Các bước cài SSL Let's Encrypt miễn phí trên DirectAdmin:
- Bước 1: Trỏ domain cần cài SSL về IP của hệ thống trong DirectAdmin, bao gồm cả record www.
- Bước 2: Ở giao diện User Level, chọn "SSL Certificates", điền Common Name (domain cần cài), Key Size (chọn 2048 hoặc 4096 bits), Certificate Type (chọn SHA256), bật "Force SSL with https redirect" để bắt buộc HTTPS, rồi nhấn Save.
- Bước 3: Sau khi lưu, hệ thống hiển thị xác nhận — quay về Home, chọn SSL Certificates để kiểm tra trạng thái "SSL is currently enabled for this domain". Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-lets-encrypt-cho-domain-tren-directadmin/

**Hỏi: Cách cài đặt Plesk Panel trên Windows Server như thế nào?**
Trả lời: Plesk Panel là công cụ quản lý máy chủ web với giao diện thân thiện, giúp đơn giản hóa các tác vụ quản trị phức tạp và tăng cường bảo mật. 7 bước cài đặt:
- Bước 1: Tải bộ cài đặt tự động từ http://autoinstall-win.pp.parallels.com/plesk-installer.exe.
- Bước 2: Mở và chạy file "plesk-installer" vừa tải.
- Bước 3: Nhập thông tin tài khoản Admin của máy chủ, hoặc truy cập http://localhost:8447.
- Bước 4: Chọn "Install or Upgrade Product".
- Bước 5: Chọn các gói cài đặt phù hợp và tiếp tục.
- Bước 6: Đặt mật khẩu cho tài khoản Admin quản trị Plesk.
- Bước 7: Chờ quá trình hoàn tất (khoảng 5-10 phút) cho đến khi nhận thông báo thành công.
Lưu ý quan trọng: kiểm tra yêu cầu hệ thống (phần cứng, RAM, dung lượng ổ đĩa, cổng 8443 không bị Firewall chặn); tắt các dịch vụ web/FTP/database đang chạy trước khi cài; đổi mật khẩu ngay sau cài đặt và cấu hình tường lửa; thiết lập sao lưu định kỳ và cập nhật bản vá bảo mật thường xuyên. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cai-dat-plesk-panel-tren-windows-server/

**Hỏi: Cách cài đặt Plesk Panel trên Linux như thế nào?**
Trả lời: Yêu cầu hệ thống: CentOS 8.x/7.x/6.x, gói wget, dịch vụ selinux và iptables phải tắt. Các bước cài đặt:
- Bước 1-3: Kiểm tra trạng thái selinux (`sestatus`) và iptables, tắt nếu đang bật; gỡ bỏ các gói xung đột (httpd, sendmail, mysql, postfix); cài đặt wget.
- Bước 4: Tải installer: `wget http://autoinstall.plesk.com/plesk-installer`.
- Bước 5: Cấp quyền và chạy: `chmod +x plesk-installer` rồi `./plesk-installer`.
- Bước 6-8: Theo hướng dẫn trên màn hình, chọn gói Plesk mong muốn (đề xuất "Recommended"), quá trình cài kéo dài 5-10 phút.
- Bước 9: Truy cập `https://<IP>:8443` với Username: root, Password: mật khẩu tài khoản root.
Gỡ cài đặt bằng lệnh: `plesk installer --remove-everything`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cai-dat-plesk-panel-tren-linux/

**Hỏi: Cách cấu hình Backup trên Plesk Panel như thế nào?**
Trả lời: 6 bước thiết lập backup trên Plesk Panel:
- Bước 1-2: Đăng nhập với tư cách Admin/Reseller/Customer qua cổng 8880 hoặc 8443, chọn Tools and Settings → Backup Manager.
- Bước 3-4: Vào Settings để cấu hình thông số cơ bản như số file backup tối đa lưu trữ, số quy trình backup đồng thời, dung lượng đĩa tối thiểu cần thiết trước khi backup.
- Bước 5-6: Chọn Schedule để đặt lịch backup — kích hoạt tác vụ backup, chọn tần suất (Daily/Weekly/Monthly), bật backup tăng dần (chỉ sao lưu thay đổi), đặt tần suất Full Backup định kỳ, cấu hình thời gian lưu trữ và thông báo lỗi qua email.
Hệ thống sẽ tự động backup theo lịch biểu đã đặt. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-backup-tren-plesk-panel/

**Hỏi: Cách sử dụng bảng điều khiển DirectAdmin như thế nào?**
Trả lời: Đăng nhập bằng thông tin từ email đăng ký hoặc truy cập qua `http://tenmiencuaban:2222/`. Giao diện chính chia làm 4 khu vực: Khu 1 (quản lý cơ bản: mật khẩu, thống kê, file, database), Khu 2 (quản lý email), Khu 3 (công cụ mở rộng: phpMyAdmin, SSL), Khu 4 (hiển thị thông tin sử dụng tài nguyên hiện tại). Các tính năng chính: Quản lý Database (tạo/xóa MySQL databases, import/export qua phpMyAdmin); File Manager (upload, download, chỉnh sửa quyền truy cập — thư mục quan trọng là public_html chứa code website); Tên Miền (quản lý domain chính và subdomain qua Domain Setup và Subdomain Management); PHP (đổi phiên bản qua mục "Select PHP version"). Lưu ý bảo mật: không đánh dấu "Main database account" khi đổi mật khẩu để tránh lỗi website. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-host-directadmin/

**Hỏi: Cách sử dụng Vesta Control Panel để tạo một WordPress website như thế nào?**
Trả lời: 4 bước thiết lập website WordPress trên Vesta Control Panel:
- Bước 1 - Thêm Web: click vào "Web" để thêm website mới, thư mục mặc định là /home/admin/web/your-domain. Advanced Options gồm Aliases, Proxy Support, SSL Support, Web Statistics (Webalizer hoặc Awstats) và Additional FTP để chia sẻ truy cập mà không cấp quyền admin.
- Bước 2 - Tạo Database: từ menu DB, tạo database mới và ghi lại mật khẩu để dùng khi cấu hình WordPress.
- Bước 3 - Thiết lập website: tải WordPress và giải nén; tạo file config bằng lệnh `cp ./wordpress/wp-config-sample.php ./wordpress/wp-config.php`; chỉnh sửa file config với thông tin database (DB_NAME, DB_USER, DB_PASSWORD).
- Bước 4 - Đẩy file lên server: dùng Filezilla hoặc FTP client upload toàn bộ file WordPress vào thư mục /home/admin/web/your-domain/public_html, sau đó truy cập website để hoàn tất cài đặt. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-vesta-control-panel-tao-mot-wordpress-website/

**Hỏi: Cách chuyển giao diện WHMCS Admin sang tiếng Việt như thế nào?**
Trả lời: Các bước:
- Bước 1: Đăng nhập vào tài khoản WHMCS.
- Bước 2: Truy cập phần "My Account".
- Bước 3: Cuộn xuống tìm mục cài đặt ngôn ngữ.
- Bước 4: Chọn "Vietnamese" từ danh sách ngôn ngữ có sẵn.
- Bước 5: Nhập mật khẩu tài khoản để xác minh.
- Bước 6: Nhấp "Save Change" để lưu thay đổi.
Có thể thực hiện tương tự để chuyển ngược lại tiếng Anh hoặc ngôn ngữ khác nếu cần. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-chuyen-whmcs-admin-qua-tieng-viet/

**Hỏi: Cách cài đặt DirectAdmin cho VPS Thế Giới Số như thế nào?**
Trả lời: Hệ thống hỗ trợ FreeBSD (5.x đến 9, 64-bit) và Debian (4.0 đến 6.0, có bản 64-bit cho 5.0 và 6.0). Cấu trúc phân vùng ổ đĩa đề xuất: /boot 500MB, swap gấp 2 lần RAM, / dùng không gian còn lại. DirectAdmin sẽ tự cài đặt các thành phần: Apache (1.3-2.4), PHP (4.4-5.4), MySQL, Exim, Dovecot, ProFTPD và nhiều dịch vụ khác (FTP, Sendmail...). Các bước cài đặt:
- Bước 1: Cài đặt thành phần hỗ trợ (CentOS/Fedora) bằng yum — các gói cần thiết như wget, gcc, bind, openssl.
- Bước 2: Tải file setup: `wget http://www.directadmin.com/setup.sh`.
- Bước 3: Cấp quyền: `chmod 755 setup.sh`.
- Bước 4: Thực hiện cài đặt: `./setup.sh`, nhập thông tin UID, LID, hostname khi được yêu cầu — quá trình tự động hoàn tất. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-direct-admin-cho-vps-tgs/

**Hỏi: Cách tạo Cron Jobs trên Cpanel như thế nào?**
Trả lời: Cron Jobs là chức năng thực thi định kỳ một lệnh trong khoảng thời gian xác định trước, ứng dụng phổ biến gồm sao lưu tự động và gửi email theo lịch. Các bước tạo:
- Bước 1: Truy cập Cpanel → Advanced → Cron Jobs.
- Bước 2: Điền email để nhận thông báo kết quả thực hiện.
- Bước 3: Tại "Add a New Cron Job", thiết lập chu kỳ chạy qua các trường Minute, Hour, Day, Month, Weekday.
- Bước cuối: Nhấp "Add New Cron Job" để kích hoạt (đảm bảo tất cả các trường đều được điền).
Ví dụ: chạy script PHP mỗi 5 phút dùng lệnh `php /home/user/public_html/demo.php`; backup MySQL hàng ngày lúc 12:00 AM dùng lệnh `mysqldump -u user -ppass_word database_name > backup.sql`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-cron-jobs-tren-cpanel/

**Hỏi: Cách tạo email theo tên miền trên DirectAdmin như thế nào?**
Trả lời: 5 bước thiết lập email trên DirectAdmin:
- Bước 1: Chọn tên miền (nếu hosting quản lý nhiều tên miền) muốn tạo email, ví dụ tgs.com.vn.
- Bước 2: Từ bảng điều khiển, chọn "E-Mail Accounts".
- Bước 3: Nhấp "Create mail account".
- Bước 4: Điền thông tin cần thiết theo hướng dẫn trên màn hình để tạo tài khoản.
- Bước 5: Cấu hình DNS — trỏ bản ghi "mail" loại A đến IP hosting, và bản ghi "@" loại MX trỏ đến "mail.tên-miền-của-bạn" với độ ưu tiên 1.
Sau khoảng 15 phút, hệ thống cập nhật và có thể truy cập webmail qua địa chỉ email vừa tạo. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-email-theo-ten-mien-tren-direct-admin/

**Hỏi: Cách cài đặt SSL Let's Encrypt trên Plesk Panel như thế nào?**
Trả lời: Let's Encrypt là nhà cung cấp chứng chỉ SSL miễn phí loại Domain Validation, hiển thị biểu tượng khóa xanh trên trình duyệt. Các bước cài đặt trên Plesk Panel:
- Bước 1: Đăng nhập vào Plesk Panel bằng thông tin quản trị hosting.
- Bước 2: Chọn tab "Website & Domain", nhấn "show more" để hiển thị thêm tính năng.
- Bước 3: Nhấn vào biểu tượng Let's Encrypt để bắt đầu quá trình cài đặt.
- Bước 4: Điền thông tin cần thiết — sau khi hoàn tất, hệ thống thông báo SSL đã cài đặt nhưng chưa kích hoạt.
- Bước 5: Vào "Hosting settings" → chọn "SSL/TLS support" → chọn Let's Encrypt làm Certificate.
Kiểm tra bằng cách truy cập website qua https:// — nếu không lỗi và có biểu tượng khóa xanh, chứng chỉ đã được cấp phát thành công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-ssl-lets-encrypt-tren-plesk-panel/

**Hỏi: Cách thay đổi ngày hết hạn (Expiration date) trong Plesk Panel như thế nào?**
Trả lời: Các bước cập nhật ngày hết hạn subscription trong Plesk:
- Bước 1-2: Truy cập Plesk, vào "Subscription → example.com", nhấn biểu tượng mở rộng thanh bên phải.
- Bước 3: Trong phần "Account" của thanh bên, chọn "Customize".
- Bước 4: Vào tab "Resources", đặt "Expiration date" theo nhu cầu.
- Bước 5: Nhấn "Update & Lock" để hoàn tất (hoặc "Apply" nếu subscription đã bị khóa). Lưu ý: subscription sẽ bị khóa nếu có thay đổi tiếp theo trong Service Plan. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cach-thay-doi-ngay-het-han-trong-plesk-panel/
