# Hướng dẫn quản trị Oracle Database

> **Danh mục:** Hướng dẫn kỹ thuật - Oracle
> **Cập nhật lần cuối:** 2026-08-22
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** oracle, oracle db, backup, datafile, listener, sqlplus, checkdisk, xóa file cũ

## Backup và dọn dẹp dữ liệu

**Hỏi: Cách tạo Backup DB Oracle trên máy chủ Linux như thế nào?**
Trả lời: Để tạo bản sao lưu (backup) cho cơ sở dữ liệu Oracle trên máy chủ Linux:
- Bước 1: Tạo thư mục backup và phân quyền cho user oracle: `mkdir /u04/HIS` sau đó `chown -R oracle.oinstall /u04/HIS`.
- Bước 2: Cấu hình thư mục trong Oracle — chuyển user bằng `su – oracle`, truy cập SQL bằng `sqlplus / as sysdba`, tạo thư mục bằng `CREATE OR REPLACE DIRECTORY backup_dir AS '/u04/HIS'`, cấp quyền bằng `GRANT READ, WRITE ON DIRECTORY backup_dir TO system`.
- Bước 3: Tạo script tự động bằng bash — cập nhật biến môi trường Oracle (ORACLE_HOME, ORACLE_SID), thay đổi mật khẩu system, sao lưu cấu trúc tablespace qua SQL, backup từng schema bằng lệnh `expdp`, di chuyển các tệp .dmp và .log đến thư mục đích. Script hỗ trợ backup nhiều schema và có thể loại trừ các bảng cụ thể khỏi quá trình sao lưu.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tao-backup-db-oracle-tren-may-chu-linux/

**Hỏi: Cách tự động xóa file cũ trên máy chủ Linux như thế nào?**
Trả lời: Để tự động xóa các file cũ hơn một khoảng thời gian nhất định trong thư mục chỉ định trên Linux:
- Bước 1: Tạo file script bash tên `xoafile.sh`.
- Bước 2: Thêm lệnh `find /u02/backup/* -mtime +1 -exec rm {} \;` vào script — thay `/u02/backup/` bằng đường dẫn thư mục cần xóa; tham số `+1` nghĩa là giữ lại file trong 1 ngày, các file cũ hơn sẽ bị xóa.
- Bước 3: Lập lịch tự động chạy script theo chu kỳ mong muốn qua crontab.
Ghi chú: có thể tùy chỉnh khoảng thời gian bằng cách thay đổi số ngày trong tham số thời gian.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/tu-dong-xoa-file-cu-tren-may-chu-linux/

**Hỏi: Cách xóa file cũ trong folder trên Windows như thế nào?**
Trả lời: Để tự động xóa các tệp cũ hơn 1 ngày trong thư mục chỉ định theo lịch trình hàng ngày trên Windows Server:
- Bước 1: Truy cập Task Scheduler (công cụ lập lịch tác vụ trên Windows Server).
- Bước 2: Tạo tác vụ mới với lịch trình xóa hàng ngày.
- Bước 3: Trong phần Action, nhập Program/Script là `ForFiles`, và Add arguments là `/p "E:\backups" /s /d -1 /c "cmd /c del /f /q @file"` (có thể điều chỉnh đường dẫn thư mục E:\backups theo nhu cầu).
Script dùng lệnh ForFiles tự động loại bỏ các tệp vượt quá 1 ngày tuổi, chạy theo lịch đã cấu hình, giúp quản lý không gian lưu trữ hiệu quả.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-xoa-file-cu-trong-folder/

## Vận hành và giám sát dịch vụ Oracle DB

**Hỏi: Cách kiểm tra các datafile đang chạy và sử dụng trong Oracle DB như thế nào?**
Trả lời: Để theo dõi và quản lý các datafile đang hoạt động trong Oracle Database qua Oracle SQL Developer:
- Bước 1: Mở Oracle SQL Developer và kết nối với cơ sở dữ liệu Oracle.
- Bước 2: Tìm tab "DBA" ở bên trái cửa sổ SQL Developer — nếu không thấy, kích hoạt qua menu View → DBA.
- Bước 3: Mở rộng thư mục "Storage" trong tab DBA sau khi kết nối thành công.
- Bước 4: Nhấp vào "Datafiles" để hiển thị danh sách các tập tin đang sử dụng.
- Bước 5: Xem thông tin chi tiết gồm tên file, tablespace liên quan, dung lượng và trạng thái hoạt động.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-kiem-tra-cac-datafile-dang-chay-va-su-dung-trong-oracle-db/

**Hỏi: Cách start lại dịch vụ Oracle DB trên máy chủ Windows như thế nào?**
Trả lời: Để khôi phục dịch vụ Oracle Database khi không chạy sau khi khởi động lại máy chủ Windows:
- Bước 1: Kiểm tra trạng thái dịch vụ — mở Services (Start → Run → `services.msc`), tìm dịch vụ Oracle và kiểm tra đang chạy hay không.
- Bước 2: Khởi động Listener — mở CMD, chạy `lsnrctl status` để kiểm tra, chạy `lsnrctl start` nếu dừng hoặc lỗi.
- Bước 3: Kết nối Oracle — gõ `sqlplus / as sysdba`; nếu không vào được, chạy `SET ORACLE_SID=orcl` trước, hoặc dùng `sqlplus sys as sysdba` rồi nhập mật khẩu.
- Bước 4: Kiểm tra trạng thái database bằng câu lệnh SQL `SELECT INSTANCE_NAME, STATUS, DATABASE_STATUS FROM V$INSTANCE`.
- Bước 5: Nếu trạng thái không Active, chạy lần lượt: `shutdown abort;`, `startup nomount;`, `alter database mount;`, `alter database open;`.
- Bước 6: Xác nhận bằng cách chạy lại câu lệnh kiểm tra ở bước 4, sau đó mở phần mềm HIS để xác minh kết nối.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-start-lai-dich-vu-oracle-db-tren-may-chu-windows/

**Hỏi: Cách start lại dịch vụ Oracle DB trên máy chủ Linux như thế nào?**
Trả lời: Để khởi động lại dịch vụ Oracle Database trên máy chủ Linux khi database không hoạt động hoặc gặp lỗi:
- Bước 1: SSH vào máy chủ Oracle DB, kiểm tra trạng thái Listener bằng `lsnrctl status` — nếu hiển thị "stop" hoặc lỗi, khởi động lại bằng `lsnrctl start`. Hoặc vào `sqlplus / as sysdba` và chạy câu lệnh kiểm tra trạng thái database; nếu hiển thị "inactive", chuyển sang bước 2.
- Bước 2: Kết nối với quyền quản trị `sqlplus / as sysdba`, chạy lần lượt: `shutdown abort;` (buộc tắt), `startup nomount;` (khởi động không mount), `alter database mount;` (mount database), `alter database open;` (mở database). Xác nhận kích hoạt thành công khi instance status trả về "ACTIVE" và database status là "OPEN".
- Bước 3: Xác minh qua ứng dụng — mở ứng dụng HIS trên desktop và đăng nhập để kiểm tra hoạt động.
Xử lý thêm nếu vẫn còn lỗi session: truy cập IIS và dừng các dịch vụ (MOS, LIS, SDA), sau đó khởi động lại theo thứ tự: SDA → ACS → MOS → các dịch vụ còn lại.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-start-lai-dich-vu-oracle-db-tren-may-chu-linux/

## Giám sát tự động (Check Disk, Down Server)

**Hỏi: Cách cấu hình check disk, giám sát server down trên Oracle Linux như thế nào?**
Trả lời: Hướng dẫn xử lý các tác vụ quản trị máy chủ gồm kiểm tra disk, giám sát server down, và xóa log trên Oracle Linux. Tạo file archive xóa log:
- Bước 1: Khởi tạo thư mục với quyền truy cập: `mkdir -p /u01/script` rồi `chmod -R 777 /u01/script/`.
- Bước 2: Đăng nhập Oracle user và tải script: `su – oracle`, `cd /u01/script/`, `wget [link script]`, `unzip scriptslinux.zip`, `cp scriptslinux/* /u01/script/`, `chmod -R 777 /u01/script/*`.
- Bước 3: Lập lịch chạy tự động lúc 22h hàng ngày qua `crontab -e` với dòng `0 22 * * * sh /u01/script/archive.sh`.
Check disk server: chỉnh sửa file `checkdisk.sh`, thay IP và tên server, lập lịch chạy lúc 3 giờ sáng qua crontab.
Giám sát server DB down: sửa file `checkdown.sh` thêm các IP máy chủ, đặt chạy 10 phút 1 lần qua crontab, sau đó restart crond.
Kiểm tra script: chạy các lệnh test để xác nhận script hoạt động đúng trước khi áp dụng chính thức.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/check-disk-ping-oracle-linux/

**Hỏi: Cách tự động check disk, giám sát down server Oracle trên Windows như thế nào?**
Trả lời: Hướng dẫn cung cấp script tự động giám sát tình trạng đĩa cứng và kết nối mạng trên máy chủ Oracle Windows để phát hiện sớm sự cố hệ thống. Kiểm tra đĩa full:
- Chuẩn bị: tạo thư mục `C:\script\`, tải file script từ link được cung cấp và giải nén vào thư mục này.
- Cấu hình: mở file `checkdisk.ps1`, tại dòng số 5 (từ dưới lên) thay đổi tên server và địa chỉ IP theo đúng chức năng, truy cập Task Scheduler để tạo tác vụ mới, lập lịch chạy script lúc 3 giờ sáng.
Kiểm tra Ping:
- Cấu hình: mở file `ping.ps1`, sửa đổi dòng 5, 6, 7 để nhập địa chỉ IP của ứng dụng và cơ sở dữ liệu cần kiểm tra, tạo tác vụ trong Task Scheduler, đặt lịch chạy lúc 3 giờ sáng, cộng thêm tùy chọn chạy lại mỗi 10 phút.
- Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-tu-dong-check-disk-down-server-oracle-windows/

**Hỏi: Cách xử lý check disk, giám sát down server và xóa archive log trên Oracle Linux như thế nào?**
Trả lời: 3 tác vụ quản trị máy chủ Oracle Linux:
- Xóa Archive Log: tạo file archive.rman (chứa lệnh RMAN xóa archive log cũ hơn 7 ngày) và archive.sh (script shell gọi file RMAN), đặt lịch cron chạy lúc 22:00 hàng ngày: `0 22 * * * sh /home/oracle/scripts/archive.sh`.
- Kiểm tra dung lượng Disk: script checkdisk.sh (chạy quyền root) giám sát sử dụng ổ đĩa với ngưỡng cảnh báo 80%, gửi thông báo Telegram nếu vượt ngưỡng, chạy lúc 03:00 mỗi ngày qua cron: `0 3 * * * sh /u01/script/checkdisk.sh`.
- Kiểm tra máy chủ Down: script checkdown.sh ping các IP máy chủ (Standby, App chính, App phụ), gửi cảnh báo Telegram khi mất kết nối, chạy mỗi 15 phút: `*/15 * * * * sh /u01/script/checkdown.sh`.
Lưu ý: sau mỗi thay đổi cron, cần khởi động lại dịch vụ bằng lệnh `systemctl restart crond`. Xem chi tiết đầy đủ (kèm hình ảnh minh họa) tại: https://tailieu.tgs.com.vn/huong-dan-xu-ly-check-disk-down-xoa-log-tren-oracle-linux/
