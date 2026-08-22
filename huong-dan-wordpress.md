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

**Hỏi: Cách cài đặt WordPress trên CentOS như thế nào?**
Trả lời: WordPress là mã nguồn mở viết bằng PHP trên cơ sở dữ liệu MySQL, chiếm khoảng 43% thị phần website toàn cầu, cho phép quản lý nội dung dễ dàng không cần biết lập trình, hỗ trợ hàng nghìn theme/plugin tùy chỉnh. Cần cài đặt sẵn LEMP hoặc LAMP stack trước khi thực hiện. Các bước cài đặt:
- Bước 1: Tải WordPress phiên bản mới nhất vào thư mục `/usr/share/nginx/html/`.
- Bước 2: Giải nén file đã tải.
- Bước 3: Tạo cơ sở dữ liệu MySQL (tên database, username, password).
- Bước 4: Sao chép file cấu hình mẫu và chỉnh sửa thông tin database.
- Bước 5: Di chuyển toàn bộ file WordPress vào thư mục gốc (root).
- Bước 6: Đổi quyền sở hữu thư mục cho user nginx.
Lưu ý bảo mật: đổi URL quản trị mặc định, cài plugin bảo mật, cập nhật thường xuyên. Lưu ý hiệu năng: cấu hình cache (Redis/Memcached), bật nén gzip; nếu dùng Apache cần tạo virtual host và bật rewrite module. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/cai-dat-wordpress-tren-centos/

**Hỏi: Cách cài đặt WordPress trên Plesk như thế nào?**
Trả lời: Plesk Hosting hỗ trợ cài đặt WordPress có sẵn chỉ qua 2 bước đơn giản, không cần thao tác thủ công. Các bước thực hiện:
- Bước 1: Đăng nhập vào Plesk Hosting bằng thông tin do nhà cung cấp gửi (ví dụ dạng http://domain:8880), chọn tùy chọn "Install wordpress" trong giao diện quản trị.
- Bước 2: Chờ quá trình cài đặt tự động hoàn tất, sau đó đăng nhập vào phần quản trị WordPress trên tên miền của bạn.
Lưu ý quan trọng: tên miền phải được trỏ về IP của hosting trước khi truy cập được. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-wordpress-tren-plesk/

**Hỏi: Xử lý website WordPress bị hack như thế nào?**
Trả lời: 7 cách khắc phục khi website WordPress bị hack:
- Khôi phục từ bản sao lưu: phương pháp tối ưu nhất nếu có sẵn backup trước khi bị tấn công, tuy có thể mất dữ liệu tạo sau lần sao lưu cuối.
- Quét tập tin độc hại: xóa theme không cần thiết (nơi hacker thường ẩn backdoor), cài plugin "Sucuri WordPress Auditing" hoặc "Theme Authenticity Checker" để phát hiện file bị thay đổi — kiểm tra kỹ thư mục theme, plugin, uploads, wp-includes, .htaccess.
- Kiểm tra tài khoản & quyền: xóa người dùng đáng ngờ, đổi username "admin" mặc định, thay đổi mật khẩu tất cả tài khoản quản trị.
- Cấu hình webserver ngăn thực thi PHP từ thư mục uploads: Apache dùng `<Files *.php> deny from all </Files>`; Nginx dùng `location ~* /uploads/.*\.php$ { deny all; }`.
- Bảo mật trang đăng nhập (plugin iThemes Security): đổi URL đăng nhập mặc định, bắt buộc mật khẩu mạnh (8+ ký tự, hỗn hợp chữ-số-ký hiệu), bật Brute Force Protection.
- Bảo mật file hệ thống: bảo vệ wp-config.php/.htaccess/wp-includes, tắt quyền ghi file, vô hiệu hóa PHP trong uploads, bật File Change Detection để nhận email khi file thay đổi.
- Cập nhật & tránh sản phẩm lậu: cập nhật WordPress/plugin/theme thường xuyên, tránh dùng theme/plugin trả phí chia sẻ trái phép (null) vì thường chứa mã độc. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/xu-ly-website-wordpress-bi-hack/

**Hỏi: Cách loại bỏ mã độc khỏi WordPress bị hack triệt để như thế nào?**
Trả lời: Website WordPress bị xâm nhập thường do 3 nguyên nhân chính: plugin không an toàn (có lỗ hổng hoặc không cập nhật), Shared Hosting kém bảo mật (hacker kiểm soát từ website khác trên cùng hosting), hoặc theme không bản quyền (theme chia sẻ miễn phí bị chèn mã độc bên trong). Quy trình xử lý triệt để (6 bước):
- Bước 1-2: Sao lưu dữ liệu hiện tại, sau đó xuất tất cả bài viết thành file XML qua Tools → Export.
- Bước 3: Tải bộ WordPress mới từ WordPress.org, cài trên hosting để có code sạch 100%.
- Bước 4-5: Sao chép thư mục uploads từ backup cũ, loại bỏ các file không phải hình ảnh (.jpg, .gif, .png), rồi tải lên website mới.
- Bước 6: Nhập lại dữ liệu XML qua Tools → Import để khôi phục bài viết.
Sau khi hoàn thành, tạo backup mới và cài đặt plugin Wordfence để tăng cường bảo mật cho lần sau. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-loai-bo-ma-doc-khoi-wordpress-bi-hack-triet-de/

**Hỏi: Cách bảo mật web WordPress bằng .htaccess như thế nào?**
Trả lời: File .htaccess có thể dùng để bảo mật website qua việc quản lý quyền truy cập tệp và ngăn chặn các mối đe dọa phổ biến. Quyền truy cập tệp an toàn (chmod): tệp thông thường dùng 644 hoặc 600, thư mục dùng 755, tệp cấu hình (config.php, wp-config.php) dùng 400 — tránh đặt bất kỳ tệp nào thành 777 vì có toàn quyền đọc/ghi/xóa, nên dùng 666 cho tệp cần quyền ghi và đọc. Các đoạn mã .htaccess hữu ích:
- Bảo vệ tệp wp-config.php: `<files wp-config.php> order allow,deny deny from all </files>`
- Bảo vệ file .htaccess: `<files .htaccess> order allow,deny deny from all </files>`
- Chống đánh cắp tài nguyên (hotlinking): dùng RewriteEngine với RewriteCond kiểm tra HTTP_REFERER và RewriteRule chặn các định dạng ảnh (gif, jpg).
- Chặn tên miền cụ thể: dùng RewriteCond kiểm tra HTTP_REFERER khớp tên miền cần chặn rồi RewriteRule trả về lỗi Forbidden.
- Chống nội dung trùng lặp (canonical URL): dùng RewriteCond kiểm tra HTTP_HOST rồi RewriteRule chuyển hướng 301 về URL chuẩn có www.
- Chống bình luận spam: dùng RewriteCond kiểm tra REQUEST_METHOD POST, REQUEST_URI wp-comments-post.php, HTTP_REFERER và HTTP_USER_AGENT để chặn spam bot gửi bình luận. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/bao-mat-web-bang-htaccess/

**Hỏi: Cách sửa lỗi "Briefly unavailable for scheduled maintenance" của WordPress như thế nào?**
Trả lời: Lỗi này xuất hiện khi cập nhật WordPress hoặc plugin bị gián đoạn. Nguyên nhân: khi WordPress tự động nâng cấp, hệ thống chèn file `.maintenance` vào thư mục gốc để hiển thị thông báo bảo trì — nếu quá trình cập nhật hoàn tất, file này sẽ tự xóa; nhưng nếu bị gián đoạn, file vẫn tồn tại khiến website không truy cập được. Các bước xử lý:
- Bước 1: Đăng nhập vào hosting qua cPanel hoặc công cụ quản lý.
- Bước 2: Hiển thị file ẩn (nếu dùng cPanel).
- Bước 3: Tìm và xóa file `.maintenance` trong thư mục gốc (root) của website.
- Bước 4: Xác nhận website hoạt động bình thường sau khi xóa. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-sua-loi-briefly-unavailable-for-scheduled-maintenance-cua-wordpress/

**Hỏi: Cách lấy lại mật khẩu admin cho WordPress như thế nào?**
Trả lời: Các bước khôi phục mật khẩu admin WordPress khi bị quên:
- Bước 1: Mở file wp-config.php trong thư mục root, tìm dòng `define('DB_NAME', 'ten-database')` để xác định tên database của website.
- Bước 2: Đăng nhập cPanel của hosting, mở phpMyAdmin, chọn database vừa xác định.
- Bước 3: Tìm bảng `wp_users`, tìm tài khoản admin cần reset, chỉnh sửa trường `user_pass`, thay giá trị hiện tại bằng chuỗi hash `$P$B7u/NYhVtuYh/cBLFwjpMmeyImMaRb.`, nhấn Go để lưu.
- Bước 4: Đăng nhập lại — mật khẩu admin lúc này sẽ là `123456`, nên đổi ngay mật khẩu mới sau khi đăng nhập thành công. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cach-lay-lai-mat-khau-admin-cho-wordpress/

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

**Hỏi: Cách tìm file cần sửa của theme trong WordPress như thế nào?**
Trả lời: Khi cài đặt theme mới, việc định vị file cần chỉnh sửa có thể khó khăn vì mỗi nhà phát triển theme viết code khác nhau. Cách đơn giản hóa bằng plugin "What The File":
- Bước 1: Cài đặt và kích hoạt plugin "What The File" — không cần cấu hình hay thiết lập phức tạp.
- Bước 2: Mở trang web muốn chỉnh sửa trong trình duyệt.
- Bước 3: Quan sát góc phải thanh Admin Bar, tìm dòng chữ "What The File", rê chuột vào để xem tên file hiện tại (nếu Admin Bar không hiển thị, vào Dashboard → Users → Your Profile để bật tùy chọn hiển thị).
- Bước 4: Vào Dashboard → Appearance → Editor, tìm file tương ứng và chỉnh sửa.
- Bước 5: Luôn sao lưu code gốc trước khi lưu thay đổi để tránh lỗi không mong muốn. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tim-file-can-sua-cua-theme-trong-wordpress/

**Hỏi: Cách tắt Wp-cron.php để tăng tốc WordPress như thế nào?**
Trả lời: Wp-cron.php là tập lệnh WordPress lên lịch các tác vụ định kỳ (sao lưu, gửi email), mặc định được kích hoạt mỗi khi có người truy cập website, gây tải nặng cho máy chủ. Tắt wp-cron.php mặc định và dùng cron job hệ thống sẽ cải thiện hiệu suất:
- Bước 1: Mở file wp-config.php, thêm dòng `define('DISABLE_WP_CRON', true);`.
- Bước 2: Vào cPanel tạo cron job thủ công, đặt lịch chạy mỗi 6 tiếng, dùng lệnh dạng `cd /home/tenmien.com/public_html; php -q wp-cron.php` (thay đường dẫn thực tế của website).
Lợi ích: giảm tải máy chủ, tăng tốc độ trang, cải thiện trải nghiệm người dùng và tỷ lệ chuyển đổi. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tat-wp-cron/

**Hỏi: Cách kiểm tra plugin nào làm chậm WordPress như thế nào?**
Trả lời: 3 phương pháp xác định plugin gây chậm hiệu suất website:
- Query Monitor: cài plugin miễn phí Query Monitor, truy cập một trang/bài đăng, kiểm tra phần "Queries" và "Scripts & Styles Loaded" trong quản trị — plugin tốn nhiều tài nguyên có khả năng cao là plugin làm chậm website.
- P3 (Plugin Performance Profiler): cài plugin P3, thực hiện scan (Start Scan → Auto), chờ hoàn tất và xem kết quả để xác định plugin tiêu tốn tài nguyên nhất.
- Kiểm tra thủ công: dùng công cụ đo thời gian như WebPagetest hoặc Pingdom, vô hiệu hóa từng plugin một, kiểm tra lại tốc độ tải sau mỗi lần vô hiệu hóa để xác định plugin nào gây cải thiện hiệu suất khi tắt.
Nên luôn tìm hiểu kỹ về plugin trước khi cài để tránh ảnh hưởng đến website. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/kiem-tra-plugin/

## WordPress Multisite

**Hỏi: WordPress Multisite là gì?**
Trả lời: WordPress Multisite là tính năng cho phép tạo mạng lưới nhiều website từ một mã nguồn WordPress duy nhất, có sẵn từ phiên bản 3.0 trở đi, phù hợp khi cần tạo nhiều website liên kết với nhau trên cùng một máy chủ (ví dụ các trang con dạng design.tenmien.com, code.tenmien.com...). Lợi ích: quản lý cập nhật dễ dàng vì tất cả website con chia sẻ một mã nguồn, tạo số lượng website con không giới hạn với phân quyền linh hoạt, tiết kiệm tài nguyên máy chủ. Nhược điểm: tất cả website con chỉ dùng chung một IP của website mẹ, bắt buộc chia sẻ plugin/theme (không thể cài riêng), dùng chung một database duy nhất. Phù hợp cho hệ thống website không có sự khác biệt quá lớn về hình thức và không yêu cầu cấu hình đặc thù riêng cho từng site. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/gioi-thieu-ve-wordpress-multisite/

## Tích hợp chat và tiện ích

**Hỏi: Cách tích hợp Facebook Chat vào website WordPress mà không cần Plugin như thế nào?**
Trả lời: Phương pháp thêm Live Chat Facebook vào website bằng mã script, tránh rủi ro bảo mật và không tốn tài nguyên như khi dùng plugin. Ưu điểm: miễn phí, giao diện đẹp gọn nhẹ hỗ trợ Tiếng Việt, tăng tương tác Fanpage, đa số người dùng có sẵn tài khoản Facebook. Nhược điểm: có thể làm chậm trang nếu kết nối quốc tế gặp vấn đề. Các bước thực hiện:
- Bước 1: Truy cập Fanpage, vào "Cài đặt".
- Bước 2: Chọn mục "Nhắn tin".
- Bước 3: Tìm phần "Thêm Messenger vào trang web", nhấn "Bắt đầu".
- Bước 4: Tùy chỉnh ngôn ngữ, lời chào, Guest Mode Status (không yêu cầu đăng nhập Facebook), màu sắc nút Chat.
- Bước 5: Thêm tên miền website, chọn "Tôi sẽ tự cài đặt mã", sao chép mã script.
- Bước 6: Chèn mã script vào file `footer.php` của theme WordPress, ngay trên thẻ đóng `</body>`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tich-hop-facebook-chat-vao-website-wordpress-ma-khong-can-dung-plugin/

**Hỏi: Cách thêm Live Chat cho WordPress với Tawk.to như thế nào?**
Trả lời: Tawk.to là dịch vụ live chat miễn phí giúp giao tiếp thời gian thực với khách hàng. Các bước tích hợp:
- Bước 1 - Tạo tài khoản Tawk.to: truy cập trang chủ Tawk.to, nhấn "Sign UP free", điền email và mật khẩu, nhấn "Signup for free", chọn ngôn ngữ và hoàn tất tạo tài khoản.
- Bước 2 - Thêm trang web: nhập tên và địa chỉ blog, nhấn "Tiếp theo: Team Members", có thể thêm email quản trị viên hoặc bỏ qua.
- Bước 3 - Cài đặt Plugin: đăng nhập WordPress dashboard, cài và kích hoạt plugin Tawk.to Live Chat, vào Settings → Tawk.to, điền thông tin đăng nhập Tawk.to, chọn thuộc tính đã tạo và widget mặc định, nhấn "Use Selected Widget".
Sau khi hoàn thành, nút chat sẽ xuất hiện ở góc dưới bên phải trang web. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-them-live-chat-cho-wordpress-voi-tawk-to/

**Hỏi: Cách cài đặt Plugins cho WordPress như thế nào?**
Trả lời: 2 phương pháp cài đặt plugin cho WordPress, sau khi đăng nhập trang quản trị và vào mục Plugins:
- Phương pháp 1 - Cài từ kho Plugin WordPress: nhấn "Cài mới", dùng khung tìm kiếm để tìm plugin cần thiết, nhấn "Cài đặt" khi tìm thấy, sau khi hoàn tất nhấn "Kích hoạt" để bật plugin.
- Phương pháp 2 - Tải lên plugin từ máy tính: chọn "Choose File" trên giao diện cài đặt, chọn file plugin đã nén dạng ZIP từ máy tính, nhấn "Install Now" để tải lên và cài đặt, sau khi hoàn tất nhấn "Active Plugins" để kích hoạt. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-cai-dat-plugins-cho-wordpress/

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
