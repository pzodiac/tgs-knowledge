# Hướng dẫn kỹ thuật WordPress

> **Danh mục:** Hướng dẫn kỹ thuật - WordPress
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** wordpress, bảo mật wordpress, sitemap, litespeed cache, wp-cli, bot telegram, malware

## Bảo mật và khắc phục sự cố

**Hỏi: Cách bảo mật cho trang web WordPress như thế nào?**
Trả lời: Để bảo vệ website WordPress khỏi tấn công hacker, SQL injection, upload mã độc và truy cập trái phép, thực hiện các biện pháp sau:
- Tắt chỉnh sửa file: thêm dòng `define( 'DISALLOW_FILE_EDIT', true );` vào file wp-config.php để ngăn kẻ tấn công chỉnh sửa file nếu chiếm được quyền truy cập dashboard.
- Gỡ bỏ plugin và theme không sử dụng: plugin/theme lỗi thời có thể bị khai thác để truy cập dashboard và upload phần mềm độc hại — nên dọn dẹp định kỳ để giảm bề mặt tấn công.
- Đổi tiền tố database: sửa `$table_prefix` trong wp-config.php từ mặc định `wp_` sang tiền tố tùy chỉnh (ví dụ `m4tb40w1k1_`), chạy lệnh SQL RENAME cho toàn bộ 12 bảng chuẩn của WordPress, cập nhật các giá trị option_name và meta_key có chứa tiền tố cũ trong bảng options và usermeta.
- Thiết lập quyền thư mục hợp lý: wp-config.php dùng chmod 644 (hoặc 640/600 để bảo mật chặt hơn); .htaccess nên dùng chmod 644 để tránh xung đột plugin.
- Cài đặt plugin bảo mật: Wordfence Security hoặc Solid Security.
- Hạn chế chỉnh sửa plugin/theme: thêm vào wp-config.php `define('DISALLOW_FILE_MODS', true);` và `define('DISALLOW_FILE_EDIT', true);`.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-bao-mat-cho-trang-web-wordpress/

**Hỏi: Cách sửa lỗi website bị Google cảnh báo chứa phần mềm độc hại như thế nào?**
Trả lời: Cách khắc phục tình trạng website bị Google cảnh báo "chứa phần mềm độc hại" (hiệu quả lên đến 90%). Nguyên nhân chính: website bị tấn công hoặc nhiễm malware khi tin tặc chèn mã độc vào các tệp hiện có hoặc tạo tệp lạ. Các bước thực hiện:
- Xác định và loại bỏ mã độc: dùng plugin WordPress như Wordfence hoặc iThemes Security để quét toàn bộ website tìm file nguy hiểm; hoặc dùng công cụ online Sucuri để quét và tìm tập tin mã độc trên website. Luôn sao lưu (backup) website trước khi xóa file.
- Gỡ bỏ cảnh báo khỏi Google: truy cập Google Search Console, chọn "Bảo mật và thao tác thủ công" → "Vấn đề bảo mật", nhấn "Yêu cầu xem xét lại" và thông báo cho Google rằng đã khắc phục xong, chờ 1-2 ngày để Google duyệt.
- Tắt cảnh báo trên trình duyệt (tạm thời): Chrome vào `chrome://settings` → Dịch vụ Sync → tắt "Safe Browsing"; Firefox vào Tùy chọn → Quyền riêng tư & Bảo mật → bỏ chọn cảnh báo bảo mật.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-sua-loi-website-bi-canh-bao-chua-phan-mem-doc-hai/

**Hỏi: Nguyên nhân các trang web WordPress bị hack là gì?**
Trả lời: Không chỉ WordPress mà tất cả các hệ thống quản lý nội dung (CMS) đều dễ bị tấn công, nhưng WordPress là mục tiêu phổ biến vì hơn 40% các trang web trên toàn thế giới sử dụng nền tảng này. 4 lý do chính khiến website WordPress bị hack:
- Plugin và Theme lỗi thời: đây là lý do đầu tiên làm website kém bảo mật — khi phát hiện lỗ hổng, hacker khai thác hàng loạt website cùng lúc. Nên cập nhật thường xuyên, chọn plugin từ nhà phát triển uy tín, tránh plugin "lậu" (nulled).
- Mật khẩu yếu: nhiều quản trị viên dùng mật khẩu thô sơ như "admin123" hoặc "654321". Cần đặt mật khẩu mạnh cho tất cả tài khoản (WordPress, MySQL, FTP, email) kết hợp chữ hoa, thường, số và ký tự đặc biệt.
- Không cập nhật WordPress: khoảng 55-61% website sử dụng phiên bản WordPress cũ do lo sợ cập nhật gây lỗi tương thích.
- Bỏ qua bảo mật: nhiều người tập trung vào thiết kế và SEO nhưng bỏ qua vấn đề bảo mật, dẫn đến hoảng loạn khi bị tấn công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/nguyen-nhan-cac-trang-web-wordpress-bi-hack/

## Tối ưu hiệu năng và SEO

**Hỏi: Cách tạo Sitemap cho website WordPress như thế nào?**
Trả lời: Sitemap là một tệp XML liệt kê các URL của trang web, tổ chức theo cấu trúc cây, giúp công cụ tìm kiếm như Google, Bing thu thập dữ liệu và lập chỉ mục trang web thông minh, hiệu quả hơn. Các bước thực hiện:
- Bước 1: Vào Dashboard → Plugin → Cài mới, tìm plugin "Yoast SEO" (được khuyên dùng), nhấn Cài đặt → Kích hoạt.
- Bước 2: Truy cập Yoast SEO → Thiết lập, tìm mục "Sơ đồ trang XML", bật tính năng "Enable feature".
- Bước 3: Truy cập Google Search Console, chọn mục "Sitemap" ở cột trái, nhập đường dẫn `sitemap_index.xml`, nhấn Submit.
Sau khi hoàn tất, Google sẽ tự động lập chỉ mục các URL trong sitemap.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-sitemap-cho-website-wordpress/

**Hỏi: Cách cài đặt Plugin LiteSpeed Cache cho WordPress như thế nào?**
Trả lời: LiteSpeed Cache giúp cải thiện hiệu suất trang WordPress bằng cách tận dụng bộ nhớ cache của máy chủ LiteSpeed, giúp nội dung được lưu trữ và tạo ra nhanh hơn, rút ngắn thời gian tải trang. Các bước cài đặt:
- Bước 1-2: Đăng nhập WordPress với quyền quản trị viên, truy cập mục Plugins trên thanh bên trái.
- Bước 3-4: Chọn "Add New" và tìm kiếm "litespeed cache" trong hộp tìm kiếm.
- Bước 5: Xác định LiteSpeed Cache và nhấp "Cài đặt" (Install).
- Bước 6: Sau khi cài đặt hoàn tất, nhấp "Kích hoạt" (Activate).
Cấu hình plugin qua LiteSpeed Cache → Settings với các tab: General (thiết lập TTL cho từng loại nội dung), Cache (kiểm soát bộ nhớ đệm cho nội dung đặc biệt), Purge (xóa cache, tự động khi cập nhật plugin/theme), Excludes (nội dung không lưu cache), Optimize (tối ưu hóa trang), CDN (cài đặt mạng phân phối nội dung), Advanced & Debug (tùy chọn nâng cao/gỡ lỗi). Nhấp "Save Changes" để lưu cấu hình.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-plugin-litespeed-cache-cho-wordpress/

## Quản trị nội dung và công cụ dòng lệnh

**Hỏi: Cách tạo bài viết trong WordPress đơn giản nhất như thế nào?**
Trả lời: Các bước cơ bản để tạo và xuất bản bài viết trên WordPress:
- Bước 1 - Truy cập và đăng nhập: truy cập trang quản trị WordPress bằng tên đăng nhập/email và mật khẩu được quản trị viên cung cấp, có thể chọn "ghi nhớ đăng nhập" cho lần sau.
- Bước 2 - Soạn thảo bài viết: sau khi đăng nhập, chọn "Bài viết" rồi nhấn "Thêm mới", có thể viết trực tiếp trên WordPress hoặc sao chép nội dung từ Word rồi dán vào.
- Bước 3 - Lưu và xuất bản: trước khi đăng, nên chọn chuyên mục phù hợp, thêm thẻ tag, xem trước bài viết để kiểm tra định dạng và bố cục trước khi xuất bản chính thức.
Khuyến nghị: viết trên Word trước, kiểm tra chính tả, rồi tải lên WordPress để dễ thực hiện hơn.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/dang-bai-viet-wordpress/

**Hỏi: Cách sử dụng WP-CLI để quản lý website WordPress như thế nào?**
Trả lời: WP-CLI giúp quản lý các tác vụ WordPress hiệu quả hơn, tiết kiệm thời gian và tự động hóa những thao tác không thể thực hiện qua giao diện quản trị thông thường. Cài đặt:
- Bước 1: Tải file `wp-cli.phar` bằng lệnh curl hoặc wget.
- Bước 2: Cấp quyền thực thi và di chuyển vào `/usr/local/bin/` bằng `chmod +x wp-cli.phar`.
- Bước 3: Đổi tên thành `wp` để chạy lệnh gọn hơn.
Các nhóm lệnh chính: WordPress Core (kiểm tra/cập nhật phiên bản, tối ưu và sửa database, xác minh checksum); Quản lý Theme (tìm kiếm, cài đặt, kích hoạt, cập nhật hoặc xóa theme qua lệnh wp-cli); Quản lý Plugin (cài đặt, kích hoạt, vô hiệu hóa, cập nhật hoặc gỡ plugin, liệt kê các bản cập nhật có sẵn); Database & Backup (export database ra file SQL, tạo file nén website WordPress bằng công cụ tar). Lưu ý quan trọng: cần di chuyển vào thư mục WordPress trước, sau đó chạy lệnh với quyền user web server phù hợp (nginx, apache hoặc www-data) bằng `sudo -u [username]`.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-su-dung-wp-cli-quan-ly-website-wordpress/

**Hỏi: Cách tạo Bot Telegram và gửi thông báo đơn giản nhất như thế nào?**
Trả lời: Bot Telegram là tính năng tương tự robot được tích hợp sẵn, giúp quản lý và điều khiển các công cụ, nhận tin tức, tích hợp với các dịch vụ khác. Phần 1 - Tạo bot:
- Bước 1: Mở Telegram, tìm kiếm BotFather.
- Bước 2: Tạo bot mới bằng lệnh `/newbot` và đặt tên.
- Bước 3: Tạo username, phải kết thúc bằng "bot".
- Bước 4: Lưu token nhận được từ BotFather.
Phần 2 - Cấu hình gửi thông báo:
- Bước 1: Tạo group và thêm bot vào.
- Bước 2: Khởi động bot bằng lệnh `/my_id @username_bot`.
- Bước 3: Lấy Chat ID qua API: `https://api.telegram.org/bot<token>/getUpdates`.
- Bước 4: Gửi thông báo qua URL: `https://api.telegram.org/bot<token>/sendMessage?chat_id=<id>&text=<nội_dung>`.
Có thể tích hợp gửi thông báo vào ứng dụng web bằng PHP hoặc cURL.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-bot-telegram/
