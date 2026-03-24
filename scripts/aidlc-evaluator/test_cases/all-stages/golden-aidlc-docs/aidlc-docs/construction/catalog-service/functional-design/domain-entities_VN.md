# Thiết kế Chức năng (Functional Design) — Catalog Service — Thực Thể Mô Hình (Domain Entities)

## Rã Thực Thể Tên: Book (Sách)

| Trường (Field) | Kiểu gán (Type) | Báo Ràng Khóa Cấu (Constraints) | Miêu Tóm Trích Nạp (Description) |
|-------|------|-------------|-------------|
| id | str (UUID nảy code) | Tự Tút Auto rẽ sinh (Auto-generated) | Mã Cựa Đai Nhận cựa Bục Duy Nảy (Unique identifier) |
| title | str | Buộc nảy Require móc, Tối Móc dải 255 chars | Tên đai nảy nháp Sách (Book title) |
| author | str | Đai Buộc Móc Required, Rẽ dải Max 255 chữ | Ngỏ Tên Tác Nảy Rã Giả bọc (Author name) |
| isbn | str | Rã Tùy Chọn đo (Optional), Max đai 13 cựa tút chars | Mã cự đứt bục ISBN-10 bạo hoặc mảng báo ISBN-13 rã |
| category | str | Bọc ngỏ Bục Yêu Báo (Required), mác Max chóp nạp 100 nảy chars | Loại Nạp Mảng Phân đai (Book category) |
| total_copies | int mảng | Bọc Required rã, Gán Lệnh `>= 1` nảy | Cục Số gán móc Sách Bản Dải Tổng Thật nảy Rã cựa (Total physical copies) |
| available_copies | bục int | Máy Auto nảy giữ, gán bọc `>= 0`, đo bọc `<= total_copies` | Sách Này Còn đai mảng Nạp Trống Rút rẽ Hiện (Currently available) |
| created_at | datetime mảng bọc (UTC) | Bục tự đai ngỏ sinh mác (Auto-generated) | Mác Thời Đai Mảng Tạo Cựa Nảy (Creation timestamp) |
| updated_at | datetime xả (UTC) | Bọc Auto tự Update đai cài bọc | Móc Giờ vách Mới đâm (Last update) |

### Lệnh Ràng Bất Biến Rập Lệnh (Invariants)
- `available_copies` bắt trích mảng đo luồn rập đai ngỏ `>= 0`. 
- `available_copies` buộc cái cháp đai rã vách rẽ mác `<= total_copies`. 
- `total_copies` cự nạp rã bục mảng móc gán `>= 1`. 
- Ở rã lúc ngỏ đai `total_copies` đai Trích Update chóp nâng gán đai, Nó mác báo phải đâm rã giữ trút được `>= (total_copies - available_copies)` cựa (có ngỏ nảy cự móc tút Nghĩa đai là KHÔNG nhả Được bục Đánh Xuống rẽ Dưới móc nảy bọc Độ Mượn xả đai checked-out bọc phễu gán count). 

## Rã Túi Thực Thể Tên: AvailabilityInfo (Chóp Gán Đối Tượng Rã Lệnh Tính Giá Trị Value Object)

| Trích Trường rã (Field) | Type Khóa Bục nảy | Trút Miêu Mảng Báo (Description) |
|-------|------|-------------|
| book_id | str rã bục | Định mảng Trích ID của ngỏ Sách bọc book rã mảng |
| title | ngỏ str | Dải Book Title cựa nạp ngỏ sách |
| total_copies | int | Gán Số Đai Trích Tổng bọc bản rã cựa copies |
| available_copies | nạp đai int | Gán Móc Sách Rã Ở mảng Available đai nảy Copies còn ngỏ |
