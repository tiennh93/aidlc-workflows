# Kế Hoạch Thực Thi

## Tóm Tắt Phân Tích

### Đánh Giá Tác Động
- **Thay đổi với người dùng**: Có — API mới hoàn toàn
- **Thay đổi cấu trúc**: Có — dự án làm lại từ đầu với lớp routes, models, engine
- **Thay đổi mô hình dữ liệu**: Có — Các Pydantic request/response models cho mọi tính năng
- **Thay đổi API**: Có — API REST đầy đủ với 7 nhóm routes + health check
- **Tác động NFR**: Có — hiệu suất, độ chính xác, và yêu cầu độ bao phủ kiểm thử

### Đánh Giá Rủi Ro
- **Mức Rủi Ro**: Thấp — phạm vi được xác định tốt, ứng dụng đơn lẻ, không có phụ thuộc ngoài thư viện chuẩn Python (stdlib)
- **Độ Phức Tạp Khi Quay Lui (Rollback)**: Dễ — dự án mới, không làm hỏng hệ thống hiện có
- **Độ Phức Tạp Kiểm Thử**: Vừa — nhiều chức năng giới hạn miền.

## Sơ Đồ Quy Trình

```
Giai đoạn 1: INCEPTION
  [x] Phát Hiện Không Gian ... HOÀN THÀNH
  [ ] Kỹ Nghệ Đảo Ngược ...... BỎ QUA (greenfield)
  [x] Phân Tích Yêu Cầu ...... HOÀN THÀNH
  [ ] User Stories ........... BỎ QUA
  [x] Kế Hoạch Quy Trình ..... ĐANG LÀM
  [ ] Thiết Kế Ứng Dụng ...... THỰC THI
  [ ] Tạo Đơn Vị ............. BỎ QUA

Giai đoạn 2: CONSTRUCTION
  [ ] Thiết Kế Chức Năng ..... BỎ QUA
  [ ] Yêu Cầu NFR ............ BỎ QUA
  [ ] Thiết Kế NFR ........... BỎ QUA
  [ ] Thiết Kế Hạ Tầng ....... BỎ QUA
  [ ] Tạo Mã Nguồn ........... THỰC THI
  [ ] Build và Test .......... THỰC THI
```

## Các Giai Đoạn Sẽ Thực Thi

### GIAI ĐOẠN KHỞI TẠO (INCEPTION)
- [x] Phát Hiện Không Gian Làm Việc (HOÀN TẤT)
- [x] Phân Tích Yêu Cầu (HOÀN TẤT) 
- [x] Kế Hoạch Quy Trình (ĐANG LÀM)
- [ ] Thiết Kế Ứng Dụng — **THỰC THI**
- [ ] User Stories — **BỎ QUA**
- [ ] Tạo Đơn Vị — **BỎ QUA**

### GIAI ĐOẠN XÂY DỰNG (CONSTRUCTION)
- [ ] Thiết Kế Chức Năng — **BỎ QUA**
- [ ] Yêu Cầu NFR — **BỎ QUA**
- [ ] Thiết Kế NFR — **BỎ QUA**
- [ ] Thiết Kế Hạ Tầng — **BỎ QUA**
- [ ] Tạo Mã Nguồn — **THỰC THI**
- [ ] Xây Dựng và Kiểm Thử — **THỰC THI**

## Dòng Thời Gian Ước Tính
- **Giai đoạn cần chạy**: 3 (Thiết Kế Ứng Dụng, Tạo Mã Nguồn, Xây Dựng và Kiểm Thử)
- **Giai đoạn bỏ qua**: 7

## Tiêu Chí Thành Công
- **Mục tiêu**: Xây dựng thành công API Máy Tính Khoa Học, mọi endpoint đều hoạt động
- **Thành quả**: Toàn bộ mã nguồn, test suite, pyproject.toml
- **Chất lượng**: 100% test pass, độ phủ ≥90%, qua kiểm định ruff
