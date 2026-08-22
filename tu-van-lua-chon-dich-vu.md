# Tư vấn lựa chọn dịch vụ hạ tầng (VPN, Proxy, E-commerce Hosting, Server)

> **Danh mục:** Tư vấn kỹ thuật - Lựa chọn dịch vụ
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** vpn, proxy, e-commerce hosting, chọn cấu hình server, dedicated server

## Mạng và bảo mật

**Hỏi: VPN là gì? Ưu nhược điểm của mạng riêng ảo VPN?**
Trả lời: VPN (Virtual Private Network) là công nghệ mạng giúp tạo kết nối mạng an toàn khi sử dụng Internet công cộng, cho phép người dùng truy cập tài nguyên mạng nội bộ từ xa một cách bảo mật, làm thiết bị hoạt động như đang nằm trên cùng một mạng nội bộ. Ưu điểm chính: chi phí thấp (tiết kiệm hơn so với thiết lập đường kết nối riêng biệt), truy cập từ xa (cho phép nhân viên kết nối an toàn với mạng doanh nghiệp khi đi công tác hoặc làm việc ngoài văn phòng), bảo mật duyệt web (mã hóa dữ liệu truyền qua mạng công cộng không an toàn), vượt qua giới hạn địa lý (truy cập được các trang web bị chặn theo khu vực), ẩn danh (che giấu hoạt động duyệt web của người dùng). Nhược điểm chính: quản lý QoS hạn chế (VPN không thể đảm bảo chất lượng dịch vụ trên Internet, có rủi ro mất mát dữ liệu), khả năng bảo mật có giới hạn (nhà cung cấp VPN không thể ngăn chặn hoàn toàn các cuộc tấn công hoặc lỗ hổng bảo mật).
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/vpn-la-gi-uu-nhuoc-diem-cua-mang-rieng-ao-vpn/

**Hỏi: Cách cấu hình Proxy của Thế Giới Số trên trình duyệt Chrome như thế nào?**
Trả lời: Để thiết lập proxy server của Thế Giới Số trên Chrome:
- Bước 1: Mở Chrome, vào Options → Setting → Advanced.
- Bước 2: Chọn "Open your computer's proxy settings" để truy cập cài đặt proxy hệ thống.
- Bước 3: Nhập địa chỉ IP proxy được cấp và port 8080.
- Bước 4: Chọn Save để hoàn tất cấu hình.
Sau khi hoàn thành, proxy server của Thế Giới Số sẽ được kích hoạt, cho phép định tuyến lưu lượng internet qua máy chủ proxy được chỉ định.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-proxy-cua-the-gioi-so/

**Hỏi: Proxy là gì và cách cài đặt proxy trên các trình duyệt/hệ điều hành phổ biến như thế nào?**
Trả lời: Proxy hoạt động như một trung gian trong quá trình gửi truy vấn đến web server, mang lại hai lợi ích chính: ẩn địa chỉ IP thật của người dùng, và tăng tốc độ truy cập nhờ khả năng cache của proxy server. Cách cài đặt proxy trên các nền tảng phổ biến:
- Firefox: được khuyến khích khi cần dùng nhiều proxy cùng lúc vì cho phép tạo các profile riêng biệt với IP proxy khác nhau (khác với Chrome).
- Internet Explorer: thao tác cài đặt tương tự các trình duyệt khác.
- Chrome: thiết lập qua phần cài đặt proxy hệ thống của trình duyệt.
- Windows (hệ điều hành): vào Settings → Network & Internet → Proxy, nhập thông số proxy và lưu lại. Lưu ý: khi thêm proxy qua hệ điều hành Windows, các trình duyệt sẽ yêu cầu nhập Username và Password.
- Kiểm tra sau khi cài đặt: truy cập các trang kiểm tra IP như ping.eu để xác nhận IP hiển thị đúng là của proxy server.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-proxy/

**Hỏi: Cách tạo SOCKS Proxy trên VPS như thế nào?**
Trả lời: Yêu cầu hệ thống: RAM trên 512MB, đường truyền từ 10Mbps trở lên, CentOS 6 hoặc 7. Các bước cài đặt Squid để tạo SOCKS Proxy:
- Bước 1: Cài đặt: `yum install -y squid`.
- Bước 2: Kích hoạt tự khởi động: `chkconfig squid on` rồi `service squid start`.
- Bước 3: Cấu hình cơ bản — sửa file `/etc/squid/squid.conf` (port mặc định 3128), đổi `http_access deny all` thành `http_access allow all`.
- Bước 4 (tùy chọn): Tạo xác thực người dùng — cài `httpd-tools` bằng `yum install -y httpd-tools`, tạo file `touch /etc/squid/squid_passwd`, cấp quyền `chown squid /etc/squid/squid_passwd`, tạo tài khoản bằng `htpasswd /etc/squid/squid_passwd user_cuaban`, rồi thêm cấu hình xác thực vào đầu file squid.conf.
- Bước 5 (tùy chọn): Ẩn dấu hiệu proxy — thêm các dòng `via off`, `forwarded_for off` và các header rule vào cuối file cấu hình.
- Bước 6: Khởi động lại: `service squid restart`.
- Gỡ cài đặt nếu cần: `yum remove squid -y`.
Squid được khuyến khích sử dụng vì tính ổn định, độ tin cậy cao và miễn phí. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-socks-proxy-tren-vps/

**Hỏi: Vì sao nên thuê VPS chất lượng cao cho doanh nghiệp?**
Trả lời: VPS chất lượng cao cho phép mỗi máy chủ là một hệ thống riêng biệt, có quyền điều hành và thao tác độc lập, giúp bảo vệ dữ liệu và ngăn chặn tấn công lan rộng — nếu một VPS gặp sự cố, các website khác trên cùng hệ thống vật lý không bị ảnh hưởng. Đây là công nghệ máy chủ ảo dựa trên nền tảng cloud, tính phí linh hoạt theo giờ giúp tối ưu chi phí, đi kèm hỗ trợ kỹ thuật 24/7. Khách hàng có thể đăng ký VPS trực tiếp qua Portal.tgs.com.vn. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/thue-vps-chat-luong-cao-day-tien-ich-cho-doanh-nghiep/

## Lựa chọn giải pháp lưu trữ cho thương mại điện tử

**Hỏi: Hosting thương mại điện tử (E-commerce hosting) là gì?**
Trả lời: Hosting thương mại điện tử là gói hosting được tạo ra dành riêng cho các website thương mại điện tử, thiết kế để đáp ứng nhu cầu cao của các nền tảng bán hàng trực tuyến với cấu hình tài nguyên vượt trội so với hosting tiêu chuẩn. Đặc điểm chính của website thương mại điện tử: dữ liệu khổng lồ (text, hình ảnh, âm thanh lớn), chức năng nâng cao (hệ thống đặt hàng, quản lý đơn, thanh toán trực tuyến), lưu lượng cao (xử lý hàng ngàn giao dịch đồng thời), cập nhật liên tục (dữ liệu sản phẩm thay đổi thường xuyên), yêu cầu bảo mật cao (bảo vệ thông tin cá nhân khách hàng), nền tảng nặng (Magento, OpenCart, Prestashop, Joomla + Virtuemart, WordPress + plugin ecommerce). E-commerce hosting cung cấp tài nguyên cao hơn đáng kể, giúp trang tải nhanh hơn khoảng 1.5 lần và bảo mật tốt hơn so với hosting thông thường.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/hosting-thuong-mai-dien-tu-la-gi/

**Hỏi: Nên chọn E-commerce hosting, VPS hay Dedicated Server?**
Trả lời: So sánh theo 4 tiêu chí: Chi phí — Dedicated Server có giá cao nhất, tiếp theo là VPS, còn E-commerce hosting rẻ nhất nhưng vẫn được tối ưu tài nguyên CPU, RAM, MySQL, I/O cao cho nhu cầu cụ thể. Tài nguyên — E-commerce hosting tích hợp sẵn ổ cứng SSD, RAID 10 và công nghệ AntiDDOS, dùng LiteSpeed thay vì Apache/Nginx nên xử lý nhanh hơn gấp nhiều lần. Quản trị hệ thống — E-commerce hosting chạy CloudLinux với cPanel được cấu hình sẵn, trong khi Dedicated Server và VPS yêu cầu người dùng tự cài đặt và quản lý. Yêu cầu kỹ thuật — E-commerce hosting không đòi hỏi kiến thức quản trị máy chủ, phù hợp người dùng không có nền tảng kỹ thuật. Kết luận: đối với website thương mại điện tử vừa và nhỏ, E-commerce hosting là lựa chọn phù hợp nhất.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/nen-chon-e-commerce-hosting-vps-hay-dedicated-server/

## Chọn cấu hình server

**Hỏi: Tiêu chí chọn cấu hình mua server máy chủ phù hợp là gì?**
Trả lời: Khi lựa chọn máy server, cần xem xét các yếu tố quan trọng: số lượng CPU hỗ trợ gắn vào cùng lúc; dung lượng RAM đủ lớn để đảm bảo hiệu suất xử lý; khả năng nâng cấp (máy server tốt cho phép thay thế linh kiện mà không cần tắt máy); hệ thống lưu trữ an toàn với RAID để bảo vệ dữ liệu; hỗ trợ kỹ thuật (cân nhắc phí dịch vụ cài đặt và hỗ trợ kỹ thuật khi mua để đảm bảo thiết lập tối ưu); tính phù hợp nhu cầu (chọn cấu hình dựa trên nhu cầu thực tế và ngân sách doanh nghiệp để đạt hiệu suất ổn định và nhanh chóng).
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/chon-cau-hinh-mua-server-may-chu-phu-hop/

**Hỏi: VPN là gì? Ưu và nhược điểm của mạng riêng ảo VPN?**
Trả lời: VPN (Virtual Private Network) là công nghệ mạng giúp tạo kết nối mạng an toàn khi tham gia vào mạng công cộng như Internet, cho phép người dùng truy cập tài nguyên nội bộ từ xa và duyệt web bảo mật. Ưu điểm: tiết kiệm chi phí (dùng Internet làm cầu nối thay vì xây dựng đường kết nối riêng tốn kém); tính linh hoạt (truy cập mạng doanh nghiệp, mạng gia đình hoặc tài nguyên nội bộ từ bất kỳ vị trí nào); bảo mật dữ liệu (mọi thông tin truyền qua mạng được mã hóa, bảo vệ thông tin trên WiFi công cộng); vượt giới hạn địa lý (truy cập các trang web bị chặn theo vùng). Nhược điểm: quản lý chất lượng hạn chế (VPN không có khả năng quản lý Quality of Service qua môi trường Internet, dẫn đến nguy cơ mất mát dữ liệu); rủi ro bảo mật (có khả năng bị hack hoặc tấn công từ bên ngoài). Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/vpn-la-gi-uu-nhuoc-diem-cua-mang-rieng-ao-vpn/

**Hỏi: Cách cấu hình Proxy của Thế Giới Số trên trình duyệt như thế nào?**
Trả lời: Các bước thiết lập proxy server (ví dụ trên Chrome):
- Bước 1: Mở trình duyệt Chrome, truy cập menu chính.
- Bước 2: Chọn Options → Setting → Advanced.
- Bước 3: Tìm mục proxy settings, chọn "Open your computer's proxy settings".
- Bước 4: Nhập thông tin proxy — địa chỉ IP của proxy server, Port: 8080.
- Bước 5: Lưu cấu hình bằng cách chọn Save.
Sau khi hoàn thành, proxy server của Thế Giới Số được kích hoạt thành công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cau-hinh-proxy-cua-the-gioi-so/
