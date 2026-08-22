# Hướng dẫn quản lý và trỏ tên miền

> **Danh mục:** Hướng dẫn kỹ thuật - Tên miền
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** dns, trỏ tên miền, cloudflare, godaddy, vestacp, epp code, trạng thái tên miền

## Trỏ DNS và quản lý DNS

**Hỏi: Cách tạo record DNS trên trang quản lý my.tgs.com.vn như thế nào?**
Trả lời: Để tạo và quản lý bản ghi DNS cho tên miền trên nền tảng my.tgs.com.vn của Thế Giới Số:
- Bước 1: Truy cập https://my.tgs.com.vn/index.php và đăng nhập bằng thông tin tài khoản được cung cấp.
- Bước 2: Sau khi đăng nhập, chọn vào ô "Tên miền" để quản lý các tên miền hiện có.
- Bước 3: Chọn tên miền mong muốn — lưu ý nhấn ra ngoài khoảng trắng, nếu nhấn thẳng vào tên miền sẽ bị chuyển đến website; sau đó nhấn nút "Quản lý" để chỉnh sửa DNS.
- Bước 4: Nhập giá trị DNS cần tạo và nhấn nút "Add" để thêm record mới. Hệ thống sẽ xác nhận thêm thành công và hiển thị các bản ghi DNS đã tạo.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cach-tao-record-dns-tren-trang-quan-ly-my-tgs-com-vn/

**Hỏi: Cách trỏ tên miền về IP Web Server (hosting) như thế nào?**
Trả lời: Để trỏ tên miền về IP Web Server bằng cách cấu hình bản ghi DNS:
- Bước 1: Truy cập trang quản lý tên miền tại `http://tenmien.tgs.com.vn`.
- Bước 2: Đăng nhập bằng thông tin (username/password) được kỹ thuật viên cung cấp khi mua tên miền.
- Bước 3: Tạo bản ghi DNS loại A cho "@" (root domain) và trỏ về địa chỉ IP của hosting.
- Bước 4: Tạo bản ghi DNS cho "www" và trỏ về cùng địa chỉ IP hosting đó.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tro-ten-mien/

**Hỏi: Cách trỏ DNS cho Google Mail (Gmail) như thế nào?**
Trả lời: Để cấu hình DNS sử dụng dịch vụ Gmail cho tên miền:
- Bước 1: Truy cập trung tâm khách hàng, chuyển đến "Quản lý tên miền" → "Tên miền đã đăng ký".
- Bước 2: Chọn tên miền cần cấu hình dịch vụ thư điện tử Google.
- Bước 3: Tạo 5 bản ghi MX với thông số: Host `@`, TTL 3600, Type MX, Priority 1, giá trị `ASPMX.L.GOOGLE.COM.`; Host `@`, TTL 3600, Type MX, Priority 5, giá trị `ALT1.ASPMX.L.GOOGLE.COM.`; Host `@`, TTL 3600, Type MX, Priority 5, giá trị `ALT2.ASPMX.L.GOOGLE.COM.`; Host `@`, TTL 3600, Type MX, Priority 10, giá trị `ALT3.ASPMX.L.GOOGLE.COM.`; Host `@`, TTL 3600, Type MX, Priority 10, giá trị `ALT4.ASPMX.L.GOOGLE.COM.`.
- Bước 4: Chờ khoảng 1-2 phút để DNS cập nhật trước khi kiểm tra chức năng gửi và nhận thư.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tro-dns-cho-google-mail/

**Hỏi: Cách trỏ tên miền trên CloudFlare về hosting như thế nào?**
Trả lời: Để cấu hình tên miền qua CloudFlare kết nối đến hosting (kết hợp lợi ích bảo mật và hiệu năng):
- Bước 1: Đăng nhập vào https://dash.cloudflare.com, chọn tên miền cần chỉnh sửa từ danh sách.
- Bước 2: Vào mục DNS, tạo "A record" bằng cách nhập địa chỉ IP máy chủ (hệ thống hiển thị "points to [địa chỉ IP]"). Có thể chỉnh sửa bản ghi có sẵn bằng cách nhấn trực tiếp vào bản ghi đó.
Lợi ích chính của CloudFlare: tăng tốc độ tải trang nhờ cache CDN, bảo mật website chống DDoS và mã độc, hoạt động ổn định nhờ cache nội dung, quản lý DNS đơn giản qua giao diện trực quan.
Lưu ý: nội dung tĩnh sẽ được cache, cần xóa cache khi cập nhật thường xuyên; CloudFlare cung cấp SSL miễn phí nhưng mã hóa đầu-cuối cần mua thêm; DNS được quản lý qua CloudFlare thay vì nhà cung cấp hosting; có tùy chọn tạm thời bỏ qua CloudFlare khi bảo trì.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tro-ten-mien-tren-cloudflare-ve-hosting/

**Hỏi: Cách quản lý, trỏ tên miền ở GoDaddy như thế nào?**
Trả lời: Hướng dẫn quản lý và cấu hình tên miền tại GoDaddy, bao gồm đổi nameserver, bản ghi DNS, gia hạn, cài đặt riêng tư và cập nhật thông tin liên hệ:
- Đổi Nameserver: chọn tên miền, chọn "Nameservers" → "Set Nameservers", chọn "Standard" (dùng server của GoDaddy) hoặc "Custom" (dùng nameserver bên ngoài), nhập thông tin nameserver mới rồi lưu.
- Trỏ tên miền về IP hosting: cần nameserver GoDaddy đang hoạt động, vào "Manage DNS" từ cài đặt tên miền, hỗ trợ các loại bản ghi A, AAAA, CNAME, MX, TXT, SRV, NS — chỉnh sửa bản ghi có sẵn hoặc thêm mới qua liên kết "ADD"; tạo subdomain bằng cách thêm bản ghi A với tên subdomain và IP đích.
- Gia hạn tên miền: thủ công bằng cách nhấn vào ngày hết hạn, chọn thời hạn gia hạn rồi thanh toán; hoặc bật gia hạn tự động — tắt bằng cách bỏ chọn "Auto-Renew" để tránh phát sinh phí ngoài ý muốn.
- Đăng ký riêng tư (Privacy): thêm "Private Registration" qua tài khoản Domains By Proxy để ẩn thông tin chủ sở hữu khỏi tra cứu WHOIS — cần tài khoản và thanh toán riêng.
- Cập nhật thông tin chủ sở hữu: vào "Contacts" → "Contact Information", dùng "Edit" để chỉnh sửa. Lưu ý quan trọng: tránh cập nhật trong vòng 60 ngày trước khi chuyển tên miền để không bị khóa tên miền.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-quan-ly-tro-ten-mien-o-godaddy/

**Hỏi: Cách cài đặt VestaCP trên server/VPS như thế nào?**
Trả lời: Yêu cầu trước khi cài: server/VPS mới, chưa cài phần mềm webserver nào; cần quyền root/sudo; hệ điều hành hỗ trợ CentOS 5/6/7, Debian 6/7/8, hoặc Ubuntu 12.04-15.10. Các bước cài đặt:
- Bước 1: Truy cập SSH vào máy chủ với quyền root.
- Bước 2: Tải gói cài đặt: `cd` rồi `curl -O http://vestacp.com/pub/vst-install.sh`.
- Bước 3: Vào trang vestacp.com, kéo xuống phần "Advanced Install Settings", giữ nguyên các tùy chọn WEB/DNS/FTP/Firewall/MAIL/DB nếu không rõ, thiết lập Hostname dạng subdomain (ví dụ sv.tenmien.com), nhập email quản trị và mật khẩu.
- Bước 4: Bấm "Generate Install Command" để lấy đoạn lệnh cài đặt.
- Bước 5: Dán lệnh vào terminal, xác nhận bằng phím Y, chờ khoảng 10-15 phút.
- Bước 6: Sau khi cài xong, truy cập `https://<IP-máy-chủ>:8083` bằng thông tin admin đã cấu hình để đăng nhập. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-vestacp/

**Hỏi: Cách tạo DNS riêng (Personal DNS Server) để trỏ tên miền về server như thế nào?**
Trả lời: Các bước tạo DNS riêng cho tên miền trên VestaCP:
- Bước 1 - Đăng ký Nameserver: truy cập quản lý tên miền, đảm bảo nameserver trỏ về `ns1.domain-của-bạn` và `ns2.domain-của-bạn`; trong phần Advanced DNS tìm "Personal DNS Server", nhấn "Add Nameservers", thêm ns1 và ns2 kèm IP máy chủ tương ứng, kiểm tra bằng nút Search. Lưu ý: DNS cần vài giờ đến 1 ngày để hoạt động.
- Bước 2 - Thiết lập DNS trên VestaCP: đổi địa chỉ DNS mặc định của tất cả user (kể cả admin) sang nameserver mới; vào mục Web thêm domain chính (chọn IP công khai, không phải IP nội bộ), vào DNS chọn template `child-ns` cho domain chính này; sửa địa chỉ DNS mặc định trong tất cả package thành nameserver mới.
- Bước 3 - Kiểm tra DNS: ping 2 nameserver để xác nhận trả về đúng IP máy chủ, truy cập domain chính để xem trang chào mừng VestaCP, dùng intodns.com để kiểm tra cấu hình (không được có lỗi đỏ ở các mục Parent, DNS, SOA, WWW).
- Bước 4 - Trỏ domain khác: các domain thêm sau này chỉ cần sửa DNS trỏ về 2 nameserver đã tạo, không cần lặp lại toàn bộ quy trình. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tao-dns-rieng-de-tro-ten-mien-ve-server/

**Hỏi: Cách thêm domain mới vào trình quản lý VestaCP như thế nào?**
Trả lời: Để thêm một domain mới vào VestaCP trên VPS:
- Bước 1: Đăng nhập vào trang quản lý VestaCP.
- Bước 2: Vào menu "Web" trên thanh menu để tìm các tùy chọn quản lý domain.
- Bước 3: Nhấn vào nút dấu "+" để thêm domain mới.
- Bước 4: Điền domain mới vào ô "domain" trong biểu mẫu hiện lên.
- Bước 5: Nhấn "Add" để thêm domain vào hệ thống quản lý.
Sau khi hoàn thành, domain sẽ được thêm vào VestaCP thành công và sẵn sàng sử dụng.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/them-domain-moi-vao-trinh-quan-ly-vestacp/

## Đăng ký và chuyển tên miền

**Hỏi: Khách hàng cần cung cấp thông tin gì khi đăng ký tên miền .VN?**
Trả lời: Khách hàng cần cung cấp các thông tin chủ thể tên miền cơ bản: Tên chủ thể (tên công ty, tổ chức hoặc cá nhân — nếu là cá nhân cần kèm số Chứng minh nhân dân hoặc Hộ chiếu), địa chỉ liên hệ, số điện thoại, địa chỉ email. Không yêu cầu khách hàng cung cấp quá nhiều thông tin chi tiết; các yêu cầu có thể khác nhau tùy loại tên miền (cấp 2, cấp 3, hoặc cấp 3 dành cho tổ chức giáo dục/địa phương). Để được tư vấn thêm, khách hàng có thể liên hệ hotline 1900.6119 hoặc 08.7309.7379 (Ext 1).
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/khach-hang-can-cung-cap-thong-tin-gi-khi-dang-ki-ten-mien-vn/

**Hỏi: Khách hàng cần cung cấp thông tin gì khi đăng ký tên miền quốc tế?**
Trả lời: Với các tên miền phổ biến (.com, .net, .biz), nhà cung cấp không đòi hỏi nhiều thông tin xác nhận đặc biệt — chỉ cần điền đầy đủ thông tin khi tạo tài khoản. Với tên miền đặc biệt, yêu cầu thêm thông tin riêng: .us (Hoa Kỳ) cần khai báo Nexus Category (mối liên hệ với Mỹ: công dân, cư trú, hoạt động kinh doanh, văn phòng), Nexus Country (mã quốc gia thường trú), Application Purpose (mục đích sử dụng: thương mại, phi lợi nhuận, cá nhân, giáo dục, chính phủ); .asia (Châu Á) cần Legal Type (loại pháp nhân: cá nhân, công ty, chính phủ, xã hội, tổ chức giáo dục), Identity Form (loại chứng thực: Passport, chứng nhận, pháp luật), Identity Number (số xác thực tương ứng); .ca (Canada) và .de (Đức) yêu cầu thông tin liên quan của chủ thể như mã bưu chính và mã số thuế, riêng .ca còn yêu cầu đáp ứng điều kiện có mặt tại Canada.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/khach-hang-can-cung-cap-thong-tin-gi-khi-dang-ki-ten-mien-quoc-te/

**Hỏi: Khách hàng chuyển tên miền quốc tế về Thế Giới Số như thế nào?**
Trả lời: Quy trình chuyển tên miền quốc tế (.com, .net, .biz, .org...) sang quản lý tại Thế Giới Số gồm các bước:
- Bước 1 - Chuẩn bị thông tin: xác định chủ sở hữu qua email đăng ký tại whois.com hoặc enom.com, lấy mã EPP (mã chuyển tên miền, còn gọi là authorization code) từ nhà đăng ký hiện tại.
- Bước 2 - Đặt hàng: cung cấp mã EPP khi đặt hàng chuyển tên miền.
- Bước 3 - Xác nhận: chủ sở hữu phải xác nhận đồng ý vào đường link Enom được gửi qua email đăng ký tên miền.
- Bước 4 - Hoàn tất: quá trình hoàn tất trong vòng 3-10 ngày, tên miền được tự động gia hạn thêm 1 năm.
Điều kiện tiên quyết: tên miền phải đã đăng ký hoặc gia hạn trên 60 ngày, không bị khóa (unlock status), có mã EPP chính xác, và nhận được xác nhận qua email từ chủ sở hữu.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/khach-hang-chuyen-ten-mien-quoc-te-ve-the-gioi-so-nhu-the-nao/

## Trạng thái tên miền

**Hỏi: Các trạng thái của tên miền .COM và .NET là gì?**
Trả lời: Các trạng thái tên miền .COM và .NET do Verisign quản lý gồm:
- ACTIVE/OK: trạng thái tên miền hoạt động bình thường sau khi đăng ký.
- REGISTRY-HOLD: nhà cung cấp registry tạm thời đình chỉ tên miền.
- REGISTRY-LOCK: nhà cung cấp registry khóa tên miền.
- REGISTRAR-HOLD: nhà đăng ký (registrar) tạm thời khóa tên miền.
- REGISTRAR-LOCK: nhà đăng ký khóa tên miền (thường theo yêu cầu của registrar).
- clientHold: tên miền hết hạn hoặc cần xác minh quyền sở hữu qua email.
- RedemptionPeriod: tên miền đã hết hạn và có 30 ngày để chuộc lại (redemption), phát sinh thêm phí.
- PendingRestore: chủ sở hữu đã chuộc lại tên miền, đang chờ kích hoạt lại.
- PendingDelete: tên miền đã hết hạn 45 ngày, đang chờ registry xóa trước khi có thể đăng ký lại.
- clientTransferProhibited: tên miền bị khóa, không thể chuyển sang nhà cung cấp khác.
- clientUpdateProhibited: không thể cập nhật thông tin DNS và chủ sở hữu.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cac-trang-thai-cua-ten-mien-com-va-net/
