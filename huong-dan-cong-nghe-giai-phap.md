# Hướng Dẫn Công Nghệ và Giải Pháp

> **Danh mục:** Công Nghệ và Giải Pháp
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** ddos, brute force, ssh key, raid, load balancing, cloudflare, cloudstorage, lỗi 400, lỗi 404, lỗi 500

## Bảo mật và phòng chống tấn công

**Hỏi: Cách xóa các đường link xấu ra khỏi website như thế nào?**
Trả lời: Để xóa các đường link xấu (backlink độc hại) khỏi website qua Google Search Console:
- Bước 1: Truy cập Google Search Console và chọn website cần xử lý.
- Bước 2: Vào mục "Links" (Liên kết) ở thanh bên trái.
- Bước 3: Kiểm tra mục "Top linked websites", chọn "Add" (Thêm).
- Bước 4: Xuất dữ liệu ra (nên chọn dạng Google Sheets để dễ rà soát).
- Bước 5: Xác định các link đáng ngờ trong bảng dữ liệu.
- Bước 6: Quay lại Search Console để gửi yêu cầu gỡ bỏ.
- Bước 7: Chọn cách gỡ — gỡ từng link riêng lẻ (dán URL vào ô tìm kiếm) hoặc gỡ hàng loạt bằng "Remove all URLs with this prefix" nếu các link có chung đặc điểm.
Lưu ý: cần rà soát kỹ trước khi xóa để tránh gỡ nhầm link hợp lệ; Google cần khoảng 5-8 giờ để crawl và gỡ các link đã đánh dấu. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cach-xoa-cac-duong-link-xau-ra-khoi-website/

**Hỏi: Có những cách nào để phòng chống DDoS cho website?**
Trả lời: Có 6+ cách phòng chống DDoS cho website:
- Chống iframe: dùng đoạn mã JavaScript ngăn chặn việc nhúng iframe từ trang web khác, tránh lạm dụng traffic.
- Chống tải lại có ác ý: triển khai .htaccess kèm file PHP yêu cầu xác nhận chuột, vô hiệu hóa công cụ tấn công tự động.
- Giới hạn kết nối: dùng hàm PHP đặt giới hạn khoảng 1000 người truy cập đồng thời, hiển thị thông báo quá tải khi vượt ngưỡng.
- Cloudflare: có bản miễn phí (hạn chế) và bản trả phí (hiệu quả hơn nhưng chi phí cao).
- Firewall mềm trên VPS: chặn IP gửi quá nhiều yêu cầu, kém khả thi với hosting dùng chung.
- Firewall cứng: phương án tối ưu nhất, gồm pfSense (mã nguồn mở) hoặc thiết bị chuyên dụng (chi phí cao nhất).
Ngoài ra nên tối ưu website qua cache và chọn nhà cung cấp hosting đáng tin cậy. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/6-cach-phong-chong-ddos-cho-trang-web-cua-ban/

**Hỏi: Các phương thức tấn công mạng phổ biến nhất hiện nay là gì?**
Trả lời: Các phương thức tấn công mạng phổ biến nhất gồm:
- Phishing: kẻ tấn công tạo trang web với địa chỉ giả mạo để lừa người dùng nhập thông tin đăng nhập, sau đó điều hướng sang website chứa mã độc.
- Tấn công trực tiếp từ bên trong: tin tặc cài thiết bị vào máy tính người khác hoặc lấy được thông tin đăng nhập để xâm nhập.
- Tấn công gián tiếp: hacker nghe trộm thông tin khi truy cập hệ thống để đánh cắp dữ liệu cá nhân.
- Tấn công qua tệp đính kèm: máy tính bị nhiễm virus (ví dụ Ransomware) ngay sau khi người dùng click vào tệp đính kèm.
- Tấn công ẩn danh: virus xâm nhập qua phần mềm độc hại hoặc ẩn trong quảng cáo.
- Tấn công vào con người: giả danh yêu cầu người dùng đổi mật khẩu hoặc cấu hình hệ thống.
- Tấn công qua thiết bị vật lý: sử dụng USB, đĩa CD, địa chỉ IP, server hoặc đầu vào máy in. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cac-phuong-thuc-tan-cong-mang-pho-bien-nhat/

**Hỏi: Brute Force Attack là gì và làm thế nào để chống cho WordPress?**
Trả lời: Brute Force Attack là phương thức tấn công trong đó hacker nắm trong tay danh sách rất lớn các username và mật khẩu phổ biến, gửi liên tục các truy vấn đăng nhập vào tệp wp-login.php, thử các tổ hợp khác nhau cho đến khi đăng nhập thành công. Website dễ bị tấn công khi: dùng tên đăng nhập phổ biến như "admin", mật khẩu yếu/dễ đoán, đường dẫn đăng nhập không bảo mật, không đổi mật khẩu thường xuyên. Cách phòng chống:
- Chọn tên đăng nhập khó đoán
- Tạo mật khẩu dài, mạnh với ký tự đặc biệt
- Giới hạn số lần đăng nhập sai
- Bảo mật đường dẫn đăng nhập
- Cập nhật mật khẩu định kỳ
- Dùng plugin hỗ trợ: Better WP Security, Login Security Solution, BruteProtect, Limit Login Attempts, KeyCaptcha. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/brute-force-attack-la-gi-va-lam-the-nao-de-chong-cho-wordpress/

**Hỏi: Cách sử dụng SSH Key để bảo mật VPS như thế nào?**
Trả lời: Đăng nhập VPS bằng mật khẩu tiềm ẩn rủi ro (mất quyền truy cập nếu lộ mật khẩu, dễ bị brute force), nên dùng SSH Key thay thế. SSH Key hoạt động theo cơ chế cặp khóa công khai/riêng tư: Private Key giống chìa khóa vật lý, Public Key giống ổ khóa tương ứng — server xác thực bằng cách kiểm tra private key có khớp với public key đã lưu không. 3 thành phần chính: Public Key (đặt trên server tại `~/.ssh/authorized_keys`), Private Key (lưu an toàn trên máy cá nhân), Keyphrase (mật khẩu bảo vệ private key, không bắt buộc nhưng nên có). Các bước triển khai:
- Trên Windows: dùng PuTTY-Gen để tạo cặp khóa, lưu private key an toàn.
- Trên Linux: chạy `ssh-keygen -t rsa` trong Terminal, đặt keyphrase khi được hỏi.
- Cấu hình server: tạo thư mục `.ssh` với quyền 700, thêm public key vào `authorized_keys` với quyền 600, sau đó tắt xác thực bằng mật khẩu trong `/etc/ssh/sshd_config`.
Lưu ý: nên tắt SELinux trước khi triển khai SSH Key để hoạt động đúng. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-ssh-key/

**Hỏi: Cách phát hiện tấn công DDoS hoặc Botnet vào website qua Raw Access Log trên cPanel và cách phòng chống như thế nào?**
Trả lời: Kiểm tra Raw Access Log trong cPanel để phát hiện tấn công — DDoS từ 1 IP là 1 nguồn liên tục yêu cầu cùng 1 URL; Botnet từ nhiều IP là nhiều IP khác nhau nhắm vào cùng endpoint với cùng chuỗi User-Agent. Cách phòng chống:
- Chặn theo địa chỉ IP: dùng .htaccess để deny IP tấn công, ví dụ `deny from 183.80.63.252`.
- Chặn theo User-Agent: xác định chữ ký trình duyệt đáng ngờ trong log; nên chặn theo thành phần nhận diện cụ thể thay vì chặn toàn bộ chuỗi User-Agent để tránh chặn nhầm người dùng hợp lệ.
- Chặn theo Referrer: ngăn các site bên ngoài truy cập tài nguyên của bạn bằng cách chặn nguồn referral trong .htaccess.
Lưu ý: kẻ tấn công thường dùng chung header trên nhiều IP, nên phân tích các thành phần nhận diện riêng trong User-Agent thay vì chặn cả chuỗi để lọc traffic độc hại hiệu quả mà vẫn giữ được truy cập hợp lệ. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/phat-hien-tan-cong-ddos-hoac-botnet-vao-website-bang-cach-xem-raw-access-log-tren-cpanel-va-cach-phong-chong/

**Hỏi: Cách cài đặt và cấu hình chống DDoS trên Linux như thế nào?**
Trả lời: Các bước cài đặt chống DDoS trên Linux:
- Dừng firewall hiện có và reset rule: `service apf stop`, `iptables -F`.
- Triển khai script bảo vệ: tải và chạy script antiDDoS để cấu hình rule iptables chống tấn công từ chối dịch vụ.
- Giám sát kết nối đang hoạt động bằng công cụ như `netstat` để phát hiện traffic đáng ngờ.
Các lưu ý cấu hình quan trọng: luôn cập nhật hệ điều hành và phần mềm với bản vá bảo mật mới nhất; thiết lập firewall bằng iptables hoặc firewalld với rule chặn kết nối đáng ngờ; cài thêm công cụ chuyên dụng như fail2ban, mod_security (Apache), nginx-naxsi (Nginx) hoặc DDoS Deflate; triển khai load balancing trên nhiều server, dùng CDN, và tối ưu tài nguyên hệ thống. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cau-hinh-chong-ddos/

## Xử lý lỗi thường gặp

**Hỏi: Lỗi 400 Bad File Request là gì và cách khắc phục như thế nào?**
Trả lời: Lỗi 400 là mã trạng thái HTTP báo hiệu yêu cầu gửi đến máy chủ đã bị bóp méo hoặc không chính xác. Nguyên nhân phổ biến: URL gõ sai hoặc liên kết trỏ đến địa chỉ lỗi, ký tự không hợp lệ trong URL (ví dụ ký tự %), cookie trình duyệt bị hỏng/cũ, lỗi bộ nhớ đệm DNS hoặc cache trình duyệt. Cách khắc phục:
- Kiểm tra URL: tìm lỗi chính tả và ký tự không hợp lệ.
- Xóa cookie trình duyệt, đặc biệt với dịch vụ Google.
- Xóa bộ nhớ đệm DNS: chạy `ipconfig /flushdns` trên Windows.
- Xóa cache trình duyệt để loại bỏ dữ liệu lưu trữ bị hỏng.
- Kiểm tra khả năng đây là lỗi timeout thay vì yêu cầu sai.
- Liên hệ nhà cung cấp hosting nếu vấn đề nằm ở phía máy chủ.
- Chờ đợi nếu toàn bộ website bị sập, cần khắc phục từ phía máy chủ. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/loi-400-bad-file-request-va-cach-khac-phuc/

**Hỏi: Lỗi 404 là gì và cách khắc phục lỗi 404 như thế nào?**
Trả lời: Lỗi 404 xuất hiện khi người dùng truy cập một URL không tồn tại. 3 nguyên nhân chính: thay đổi URL mà không thông báo cho công cụ tìm kiếm (phổ biến nhất), Mod Rewrite lỗi trong file .htaccess khi chuyển hướng URL gây lỗi hàng loạt, sai code (WordPress, PHP) như dấu ngoặc hoặc chính tả sai. Lỗi 404 nhiều sẽ làm giảm thứ hạng website trên Google và ảnh hưởng điểm chất lượng toàn bộ từ khóa. Giải pháp:
- Dùng file .htaccess với `ErrorDocument 404` để chuyển hướng.
- Tạo file 404.php trong WordPress.
- Dùng Plugin Broken Link Checker để phát hiện liên kết gãy.
- Báo cáo sửa lỗi trong Google Webmaster Tools. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/loi-404-la-gi-va-cach-khac-phuc-loi-404/

**Hỏi: Lỗi 500 Internal Server Error là gì và có thể tự khắc phục không?**
Trả lời: Lỗi 500 là lỗi chung với mã trạng thái HTTP 500, xuất hiện khi máy chủ trang web bị lỗi — không phải lỗi từ trình duyệt hay máy tính người dùng, mà từ máy chủ hosting. Có thể hiển thị dưới dạng "500 Internal Server Error", "HTTP Error 500" hoặc chỉ "500". Cách khắc phục tạm thời cho người dùng: chờ vài phút rồi tải lại trang (F5), tránh tải lại khi đang giao dịch vì có thể gây lỗi thanh toán kép; có thể xem bản lưu cache qua Google Cache hoặc Wayback Machine; nếu vẫn lỗi, liên hệ trực tiếp quản trị viên website qua email/điện thoại/mạng xã hội. Đối với quản trị viên, nguyên nhân thường do: file .htaccess sai cấu hình, quyền hạn file/thư mục không chính xác, phần mềm cần thiết chưa được cài đặt, hoặc hết thời gian chờ kết nối tài nguyên bên ngoài. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/loi-500-internal-server-error-la-gi-ban-co-the-tu-khac-phuc-khong/

## Quản trị hệ thống và hạ tầng

**Hỏi: So sánh giữa máy chủ Linux và máy chủ Windows như thế nào?**
Trả lời: Máy chủ Linux có các ưu điểm: chi phí thấp (phần mềm nguồn mở, có thể dùng miễn phí), tương thích tốt với PHP/MySQL/Perl, hỗ trợ tốt cơ sở dữ liệu MySQL/PostgreSQL, được đánh giá an toàn hơn Windows, linh hoạt khi chuyển đổi và mở rộng. Máy chủ Windows phù hợp khi: cần công nghệ .Net (ASP.Net, VB.Net chỉ chạy trên Windows), cần cơ sở dữ liệu MSSQL/Access (yêu cầu nền tảng Windows), cần các dịch vụ đặc thù của Microsoft; nhược điểm là chi phí cao do bản quyền bắt buộc. Nên chọn Linux cho hầu hết website thông thường, Windows là bắt buộc khi cần công nghệ độc quyền của Microsoft. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/so-sanh-giua-may-chu-linux-va-may-chu-windows/

**Hỏi: Có những phương án nào để làm mát Data Center ở cấp độ phòng, dãy Rack và tủ Rack?**
Trả lời: Có 3 phương án làm mát chính cho trung tâm dữ liệu:
- Làm mát cấp độ phòng: phương pháp truyền thống dùng thiết bị CRAC phân phối khí lạnh đồng đều đến mọi thiết bị trong phòng, kém hiệu quả với mật độ công suất cao (trên 20 kW/rack).
- Làm mát cấp độ dãy Rack: CRAC bố trí riêng cho từng dãy, định hướng luồng khí tốt hơn, phù hợp cho TTDL mới dưới 200 kW hoặc mật độ tải từ 5 kW/rack.
- Làm mát cấp độ tủ Rack: CRAC gắn trực tiếp trong mỗi tủ, tập trung công suất làm mát cho nhu cầu thực tế từng rack, hỗ trợ mật độ lên đến 50 kW/rack.
Các phương án có thể kết hợp (giải pháp lai) để tối ưu hiệu quả cho các trung tâm dữ liệu hiện đại với yêu cầu đa dạng. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/3-phuong-an-lam-mat-cap-do-phong-day-rack-va-tu-rack/

**Hỏi: Cách giảm dung lượng file log MySQL hoặc disable log MySQL như thế nào?**
Trả lời: Các bước giảm dung lượng file log MySQL hoặc disable logging:
- Kết nối SSH và mở file cấu hình MySQL: `vi /etc/my.cnf`
- Để tắt hoàn toàn logging: tìm và comment dòng `log_bin = /var/log/mysql/mysql-bin.log`
- Hoặc để giới hạn thời gian lưu log: thêm 2 dòng cấu hình nếu chưa có: `expire_logs_days = 10` (giữ log 10 ngày) và `max_binlog_size = 100M` (giới hạn mỗi file log tối đa 100MB)
- Khởi động lại MySQL: `service mysql restart`
Có thể chọn tắt hoàn toàn log hoặc giới hạn thời gian lưu trữ tùy theo nhu cầu server — cách giới hạn giúp cân bằng giữa việc giữ log để tra cứu và tránh chiếm quá nhiều dung lượng ổ đĩa. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cach-giam-dung-luong-file-log-mysql-hoac-disable-log-mysql/

**Hỏi: OwnCloud là gì và cách tự dựng lưu trữ đám mây trên máy chủ riêng?**
Trả lời: OwnCloud là nền tảng mã nguồn mở giúp cá nhân và doanh nghiệp nhỏ tự xây dựng máy chủ lưu trữ đám mây riêng, thay vì phụ thuộc vào dịch vụ bên thứ ba như Dropbox hay Google Drive, giúp người dùng toàn quyền kiểm soát hạ tầng dữ liệu. Tính năng chính: dung lượng lưu trữ không giới hạn (tùy khả năng server), hỗ trợ nhiều người dùng với phân quyền tùy chỉnh, có client đa nền tảng (Windows, Mac, iOS, Android), xem trước nhanh file ảnh/tài liệu/audio/video, chia sẻ link công khai có mật khẩu và thời hạn, hỗ trợ nhiều extension/addon, bảo mật tốt. Yêu cầu cài đặt: PHP 5.4 trở lên, cần MySQL, SQLite hoặc PostgreSQL 9.0 trở lên; tương thích với hosting dùng chung Linux và VPS/Dedicated Server chạy LAMP stack. Các bước cài đặt cơ bản: upload file setup-owncloud.php lên server và chạy qua trình duyệt tại tên miền của bạn; trình cài đặt sẽ hướng dẫn chọn thư mục lưu trữ và tạo tài khoản quản trị ban đầu; sau đó có thể tải client desktop/mobile để đồng bộ dữ liệu. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/owncloud-tu-lam-luu-tru-dam-may-tren-may-chu/

**Hỏi: Có những loại RAID lưu trữ nào và đặc điểm ra sao?**
Trả lời: Các loại RAID lưu trữ phổ biến:
- RAID 0 (Striping): tối thiểu 2 ổ đĩa, dữ liệu chia đều trên các ổ để tăng hiệu năng, tốc độ đọc/ghi nhanh gấp đôi bình thường về lý thuyết, nhưng mất 1 ổ là mất toàn bộ dữ liệu; phù hợp ứng dụng cần tốc độ cao như streaming video.
- RAID 1 (Mirroring): tối thiểu 2 ổ đĩa, dữ liệu được sao chép giống hệt trên cả 2 ổ, bảo vệ dữ liệu tốt (1 ổ hỏng vẫn hoạt động bình thường), nhược điểm là dung lượng sử dụng chỉ bằng 1 ổ; phù hợp website nhỏ/vừa và dịch vụ tài chính.
- RAID 10 (kết hợp): tối thiểu 4 ổ đĩa, kết hợp cả striping và mirroring để vừa nhanh vừa an toàn, phù hợp mọi trường hợp sử dụng nhưng chi phí cao, dung lượng sử dụng chỉ 50%.
- RAID 5 (Distributed Parity): tối thiểu 3 ổ đĩa, cân bằng giữa hiệu năng và bảo vệ dữ liệu nhờ phân tán dữ liệu và thông tin parity, là lựa chọn tiết kiệm chi phí hơn RAID 10, phù hợp website và ứng dụng ở mọi quy mô. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tim-hieu-chung-ve-cac-loai-raid-luu-tru/

**Hỏi: Network Load Balancing khác gì với Cluster?**
Trả lời: Network Load Balancing (NLB) và Cluster đều nhằm kết hợp sức mạnh của nhiều máy chủ thành một hệ thống và tăng khả năng chịu lỗi. Điểm khác biệt: NLB — các node có thể dùng chung hoặc tách riêng bộ nhớ lưu trữ, cân bằng traffic TCP và UDP, không cần phần cứng chuyên dụng, thường dùng cho web server, ISA server, VPS, media server, terminal server, hoạt động ở chế độ Active. Cluster — các node bắt buộc phải dùng chung hệ thống lưu trữ tập trung, cung cấp khả năng failover/failback cho ứng dụng, cần thiết bị lưu trữ chuyên dụng đắt tiền (SCSI, Fibre Channel, iSCSI), thường dùng cho SQL Server, Exchange Server, file server, hoạt động ở chế độ Active/Passive. NLB và Cluster thường phối hợp với nhau trong hệ thống mạng: NLB đóng vai trò front-end xử lý truy cập từ bên ngoài (tạo virtual server/IP ảo), Cluster đóng vai trò back-end. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/network-load-balancing/

## Cloudflare và Cloudstorage

**Hỏi: Cách cài đặt Cloudflare cho website như thế nào?**
Trả lời: Cloudflare là dịch vụ proxy trung gian điều phối traffic website và kiểm soát truy cập qua lớp bảo vệ của nó — thay vì người dùng truy cập trực tiếp server, traffic sẽ đi qua hạ tầng Cloudflare trước. Lợi ích chính: tăng tốc độ nhờ cache nội dung website trên 102 trung tâm dữ liệu toàn cầu và phân phối từ server gần nhất về mặt địa lý, nén gzip các tài nguyên tĩnh (ảnh, CSS, JS) để giảm băng thông; cải thiện bảo mật nhờ lọc traffic độc hại, cấp SSL miễn phí, giới hạn theo khu vực địa lý, chặn IP cụ thể, và WAF (Web Application Firewall) ở gói trả phí để chống SQL injection và XSS. Các bước cài đặt:
- Đăng ký tại cloudflare.com/sign-up
- Thêm tên miền và cấu hình bản ghi DNS bằng cách sửa Record A trỏ về IP host của bạn
- Chọn gói Free
- Đổi nameserver của tên miền sang 2 nameserver do Cloudflare cung cấp
- Chờ DNS lan truyền (propagation) hoàn tất
Lưu ý cài đặt: chế độ Development Mode tắt cache khi đang chỉnh sửa CSS/JS, tự động tắt sau 3 giờ; Auto Minify nén HTML/CSS/JS nhưng một số giao diện website có thể lỗi sau khi bật nén CSS/JS; nên dùng mức Cache Level tiêu chuẩn cho hầu hết trường hợp. Cloudflare hiện chưa có Data Center tại Việt Nam nên có thể giảm nhẹ tốc độ cho người dùng trong nước. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-cloudflare-cho-website/

**Hỏi: Cách cấu hình Cloudstorage của Thế Giới Số như thế nào?**
Trả lời: Các bước cấu hình dịch vụ Cloudstorage của Thế Giới Số:
- Bước 1: Tải phần mềm client OwnCloud tương ứng cho Windows hoặc Mac OS.
- Bước 2: Cài đặt — chạy trình cài đặt và tiếp tục với các tùy chọn mặc định cho đến khi hoàn tất.
- Bước 3: Cấu hình — mở ứng dụng OwnCloud từ Desktop hoặc Program Files, nhập `http://cloudstorage.tgs.com.vn` vào ô Server Address, nhập tên đăng nhập và mật khẩu, sau đó chọn thư mục lưu trữ cục bộ.
Dữ liệu trong thư mục đã chọn sẽ tự động đồng bộ (sync) và upload lên hệ thống CloudStorage để backup, cho phép người dùng sao lưu và truy cập dữ liệu từ nhiều thiết bị qua nền tảng đám mây. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-cloudstorage-the-gioi-so/
