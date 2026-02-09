# Tiêu chuẩn Biểu đồ ASCII

## BẮT BUỘC: Chỉ sử dụng ASCII Cơ bản

**QUAN TRỌNG**: LUÔN LUÔN sử dụng các ký tự ASCII cơ bản cho các biểu đồ (khả năng tương thích tối đa).

### ✅ ĐƯỢC PHÉP: `+` `-` `|` `^` `v` `<` `>` và văn bản chữ và số

### ❌ BỊ CẤM: Các ký tự vẽ hộp Unicode

- KHÔNG: `┌` `─` `│` `└` `┐` `┘` `├` `┤` `┬` `┴` `┼` `▼` `▲` `►` `◄`
- Lý do: Hiển thị không nhất quán trên các phông chữ/nền tảng khác nhau

## Các mẫu Biểu đồ ASCII Tiêu chuẩn

### QUAN TRỌNG: Quy tắc Chiều rộng Ký tự

**Mỗi dòng trong một hộp PHẢI có CHÍNH XÁC cùng số lượng ký tự (bao gồm cả khoảng trắng)**

✅ ĐÚNG (tất cả các dòng = 67 ký tự):

```
+---------------------------------------------------------------+
|                      Component Name                           |
|  Description text here                                        |
+---------------------------------------------------------------+
```

❌ SAI (chiều rộng không nhất quán):

```
+---------------------------------------------------------------+
|                      Component Name                           |
|  Description text here                                   |
+---------------------------------------------------------------+
```

### Mẫu Hộp

```
+-----------------------------------------------------+
|                                                     |
|              Calculator Application                 |
|                                                     |
|  Provides basic arithmetic operations for users     |
|  through a web-based interface                      |
|                                                     |
+-----------------------------------------------------+
```

### Hộp Lồng nhau

```
+-------------------------------------------------------+
|              Web Server (PHP Runtime)                 |
|  +-------------------------------------------------+  |
|  |  index.php (Monolithic Application)             |  |
|  |  +-------------------------------------------+  |  |
|  |  |  HTML Template (View Layer)               |  |  |
|  |  |  - Form rendering                         |  |  |
|  |  |  - Result display                         |  |  |
|  |  +-------------------------------------------+  |  |
|  +-------------------------------------------------+  |
+-------------------------------------------------------+
```

### Mũi tên và Kết nối

```
+----------+
|  Source  |
+----------+
     |
     | HTTP POST
     v
+----------+
|  Target  |
+----------+
```

### Luồng Ngang

```
+-------+     +-------+     +-------+
| Step1 | --> | Step2 | --> | Step3 |
+-------+     +-------+     +-------+
```

### Luồng Dọc có Nhãn

```
User Action Flow:
    |
    v
+----------+
|  Input   |
+----------+
    |
    | validates
    v
+----------+
| Process  |
+----------+
    |
    | returns
    v
+----------+
|  Output  |
+----------+
```

## Xác thực

Trước khi tạo biểu đồ:

- [ ] Chỉ ASCII cơ bản: `+` `-` `|` `^` `v` `<` `>`
- [ ] Không vẽ hộp Unicode
- [ ] Khoảng trắng (không dùng tab) để căn chỉnh
- [ ] Các góc sử dụng `+`
- [ ] **TẤT CẢ các dòng hộp cùng chiều rộng ký tự** (đếm ký tự bao gồm khoảng trắng)
- [ ] Kiểm thử: Xác minh các góc thẳng hàng theo chiều dọc trong phông chữ đơn cách (monospace)

## Thay thế

Đối với các biểu đồ phức tạp, hãy sử dụng Mermaid (xem `content-validation.md`)
