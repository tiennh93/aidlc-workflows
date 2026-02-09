# Các Quy tắc Xác thực Nội dung

## BẮT BUỘC: Xác thực Nội dung Trước khi Tạo Tệp

**QUAN TRỌNG**: Tất cả nội dung được tạo PHẢI được xác thực trước khi ghi vào tệp để ngăn lỗi phân tích cú pháp (parsing).

## Tiêu chuẩn Biểu đồ ASCII

**QUAN TRỌNG**: Trước khi tạo BẤT KỲ tệp nào có biểu đồ ASCII:

1. **TẢI** `common/ascii-diagram-standards.md`
2. **XÁC THỰC** từng biểu đồ:
   - Đếm ký tự trên mỗi dòng (tất cả các dòng PHẢI có cùng chiều rộng)
   - CHỈ sử dụng: `+` `-` `|` `^` `v` `<` `>` và khoảng trắng
   - KHÔNG dùng ký tự vẽ hộp Unicode
   - Chỉ khoảng trắng (KHÔNG dùng tab)
3. **KIỂM THỬ** căn chỉnh bằng cách xác minh các góc hộp thẳng hàng theo chiều dọc

**Xem `common/ascii-diagram-standards.md` để biết các mẫu và danh sách kiểm tra xác thực.**

## Xác thực Biểu đồ Mermaid

### Các bước Xác thực Yêu cầu

1. **Kiểm tra Cú pháp**: Xác thực cú pháp Mermaid trước khi tạo tệp
2. **Escape Ký tự**: Đảm bảo các ký tự đặc biệt được escape đúng cách
3. **Nội dung Dự phòng**: Cung cấp văn bản thay thế nếu Mermaid không vượt qua xác thực

### Quy tắc Xác thực Mermaid

```markdown
## TRƯỚC KHI tạo bất kỳ tệp nào có biểu đồ Mermaid:

1. Kiểm tra các ký tự không hợp lệ trong ID nút (chỉ sử dụng chữ số + gạch dưới)
2. Escape các ký tự đặc biệt trong nhãn: " → \" và ' → \'
3. Xác thực cú pháp lưu đồ: các kết nối nút phải hợp lệ
4. Kiểm thử phân tích cú pháp biểu đồ với xác thực đơn giản

## DỰ PHÒNG: Nếu xác thực Mermaid thất bại, sử dụng biểu diễn quy trình làm việc dựa trên văn bản
```

### Mẫu Triển khai

````markdown
## Trực quan hóa Quy trình làm việc

### Biểu đồ Mermaid (nếu cú pháp hợp lệ)

```mermaid
[nội dung biểu đồ đã xác thực]
```
````

### Văn bản Thay thế (luôn bao gồm)

```
Giai đoạn 1: INCEPTION
- Bước 1: Phát hiện Workspace (ĐÃ HOÀN THÀNH)
- Bước 2: Phân tích Yêu cầu (ĐÃ HOÀN THÀNH)
[tiếp tục với biểu diễn văn bản]
```

## Xác thực Nội dung Chung

### Danh sách kiểm tra Xác thực Trước khi Tạo

- [ ] Xác thực các khối mã nhúng (Mermaid, JSON, YAML)
- [ ] Kiểm tra escape ký tự đặc biệt
- [ ] Xác minh tính chính xác của cú pháp markdown
- [ ] Kiểm thử khả năng tương thích phân tích nội dung
- [ ] Bao gồm nội dung dự phòng cho các phần tử phức tạp

### Quy tắc Ngăn ngừa Lỗi

1. **Luôn xác thực trước khi sử dụng công cụ/lệnh để ghi tệp**: Không bao giờ ghi nội dung chưa được xác thực
2. **Escape ký tự đặc biệt**: Đặc biệt là trong các biểu đồ và khối mã
3. **Cung cấp các lựa chọn thay thế**: Bao gồm các phiên bản văn bản của nội dung trực quan
4. **Kiểm thử cú pháp**: Xác thực các cấu trúc nội dung phức tạp

## Xử lý Thất bại Xác thực

### Khi Xác thực Thất bại

1. **Ghi nhật ký lỗi**: Ghi lại những gì không vượt qua xác thực
2. **Sử dụng nội dung dự phòng**: Chuyển sang thay thế dựa trên văn bản
3. **Tiếp tục quy trình làm việc**: Không chặn do lỗi xác thực nội dung
4. **Thông báo cho người dùng**: Đề cập rằng nội dung đơn giản hóa đã được sử dụng do hạn chế phân tích cú pháp
