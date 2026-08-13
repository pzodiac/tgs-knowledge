# Hướng dẫn định dạng tài liệu để Dify (open-source) đọc dễ dàng

> **Danh mục:** Hướng dẫn kỹ thuật
> **Cập nhật lần cuối:** 2026-08-13
> **Người phụ trách:** Phòng kỹ thuật
> **Từ khóa:** dify, knowledge base, định dạng markdown, chunking

## Tổng quan

Tài liệu này hướng dẫn cách viết file Markdown để Dify (bản open-source) có thể nạp (import) vào Knowledge Base và truy xuất (retrieval) chính xác, hiệu quả.

## Vì sao cần định dạng đúng?

Dify chia tài liệu thành các đoạn nhỏ (chunk) — thường theo heading hoặc theo độ dài ký tự — rồi lưu từng chunk thành vector để tìm kiếm ngữ nghĩa. Nếu tài liệu viết không rõ cấu trúc, chunk bị cắt sai chỗ sẽ làm mất ngữ cảnh, khiến bot trả lời sai hoặc thiếu thông tin.

## Nguyên tắc định dạng

### 1. Dùng heading rõ ràng, phân cấp hợp lý

- Mỗi `##`/`###` nên bắt đầu một ý/chủ đề mới, độc lập.
- Tránh nhồi nhiều chủ đề không liên quan vào cùng một heading.

### 2. Mỗi đoạn phải tự đủ nghĩa

- Không dùng đại từ mơ hồ ("nó", "cái đó") tham chiếu về đoạn trước — vì khi bị chunk riêng, đoạn đó sẽ mất ngữ cảnh.
- Nhắc lại chủ thể/tên riêng nếu cần, dù có vẻ lặp từ.

### 3. Ưu tiên đoạn ngắn, súc tích

- Mỗi chunk lý tưởng khoảng 200–500 từ (tuỳ cấu hình chunk size trong Dify).
- Dùng danh sách gạch đầu dòng cho các ý rời rạc, dễ trích xuất từng câu.

### 4. Định dạng Hỏi/Đáp cho nội dung FAQ

```
**Hỏi: [câu hỏi đầy đủ]?**
Trả lời: [câu trả lời đầy đủ, không phụ thuộc câu hỏi/đáp khác]
```

Định dạng này giúp Dify trả lời chính xác vì mỗi cặp Hỏi/Đáp có thể tách thành một chunk độc lập, mang đủ ngữ cảnh.

### 5. Thêm metadata ở đầu file (tuỳ chọn)

Ghi chú danh mục, từ khóa, ngày cập nhật ở đầu file giúp lọc và gắn tag dữ liệu khi quản lý nhiều tài liệu trong Knowledge Base, dù Dify không bắt buộc phần này.

### 6. Tránh các lỗi thường gặp

- Không để bảng biểu (table) quá phức tạp — Dify có thể đọc nhưng khó chunk chính xác.
- Không nhúng ảnh/base64 trực tiếp vào file Markdown dùng cho Knowledge Base.
- Không viết một đoạn văn quá dài không có heading — nên chia nhỏ.

## Quy trình nạp tài liệu vào Dify

1. Vào **Knowledge** > **Create Knowledge** trên Dify.
2. Chọn **Import from file**, upload file `.md`.
3. Chọn chế độ chia đoạn: **Automatic** (Dify tự chunk theo heading) hoặc **Custom** (tự đặt chunk size/overlap).
4. Xem trước các chunk được tạo ra, kiểm tra xem có bị cắt sai ngữ cảnh không.
5. Xác nhận và lưu vào Knowledge Base để dùng cho ứng dụng/chatbot.
