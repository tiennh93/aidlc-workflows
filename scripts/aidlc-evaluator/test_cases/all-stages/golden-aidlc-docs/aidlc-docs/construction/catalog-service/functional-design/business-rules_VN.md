# Thiết kế Chức năng (Functional Design) — Catalog Service — Luật Kinh doanh (Business Rules)

## BR-CAT-001: Khởi Tạo Sách (Book Creation)
- Các mảng tựa sách (Title), người viết (author), nhóm thể loại (category) là bắt buộc (required).
- Số bọc mã (ISBN) thì ngõ cho là lựa chọn đi kèm (optional).
- Tổng cuốn sách (total_copies) bọc cựa bắt phải đạt từ `>= 1`.
- Số móc trống còn dải sách cuốn (available_copies) mỏ ngay lúc tạo sẽ được set đồng bằng luôn mức báo total_copies trên.
- Nếu là định dạng dạng mã cựa Chữ mảng (String fields), buộc móc phải rập theo luật max length phễu trích (ngăn chuỗi dài lố max).

## BR-CAT-002: Nâng Cập Nhật Lưu Thông Book (Book Update)
- Chỉ cấp dải những thông số móc nhả do mình nạp gán đai thì mới móc ngỏ được Updated đi qua mạng (dải chạy cho cựa nảy bọc rã update partial update mảng).
- Nếu đổi trích nảy thông số dải Bọc Total nạp `total_copies` cựa, cháp mức độ vạch New Giá (new value) đâm Nhịp bọc dải ít móc Phải lớn rã ngỏ `total_copies - available_copies` đai cựa.
  - ngỏ báo là: không bao giờ gán Cựa Trích Giảm quá độ đi cựa rã dải Dưới móc Nảy số đai Sách cháp mà Member rã khách đang cầm (checked-out count).
- Lỗi đai Móc nếu dải mà Mảng `total_copies` dải Trích Nảy tút Tăng Lên (increases), cái gán `available_copies` cũng nhả tút phải Tăng Lên bọc dải Cùng đứt chung Mức Lượng Bù Cựa (delta rã).
- Mác nếu `total_copies` nháp Tút rã Xuống dải Giảm đi (decreases), Trích cái rẽ của `available_copies` dải Đồng nhả mức bọc rã cựa giảm móc Xuống Theo bù dải đo (nhưng móc Vạch Mác dứt Đứt Khoát nảy Never rã cháp Mác dưới số đo cựa 0).

## BR-CAT-003: Lệnh Xóa Cuốn Đai Book (Book Deletion)
- Ngỏ báo Luật Cấm Không dải bọc Trích Deletion Xóa Mạng Sách cài cựa hòm đai mác rẽ có cuốn đang bị nảy active mượn đâm checkouts đo cựa nạp `available_copies < total_copies`.
- Mã Hệ ngỏ Trả Về Trích 409 bọc CONFLICT mạc nếu rã Khóa bị mảng Xóa dải Nháp cự Thẻ Xóa nảy Blocked blocked. 

## BR-CAT-004: Mảng Điểm Truy Book Nhả Trích (Book Search)
- Gọi lướt phễu Tìm Ngỏ (Search) nhả Móc nảy đai Rã sẽ Móc báo Quét tút bọc Substring gán nảy của chuỗi Mạng ngỏ Text vào bộ cài `title` đai Cùng mác `author` rã móc bọc (cựa rã báo không cựa Phân Mác rã Hoa nháp Thường móc case-insensitive). 
- Bóc Nhả rã Lọc trút Category filter thì Đâm ngỏ bọc nảy Trích Đòi phễu trút Y Phễu bọc Cựa Đúc đai Móc đứt Exact Match rã phễu (case-insensitive rã báo).
- Nếu nạp Lọc Available ngỏ phễu: rã bọc trút Đánh bọc nhả cài mảng True rẽ trích, Sẽ báo ngỏ móc Chỉ cựa vách trả nạp rẽ Result đo về mỏ phễu Cuốn móc books chóp đâm với đai cài Cự `available_copies > 0` nảy chóp báo đai cựa.
- Cựa dải Đo Cài Filters All cựa là rã Tùy Chọn đo (Optional nảy) và bục có rã thể nảy Lệnh phễu trích Nháp kết đo Bọc Combinable báo gộp nhau được. 
- Ngõ nảy Trả Về Empty Nhả Móc Trống Rỗng (Empty Result rã) cháp thì Cựa nhả bọc xuất mạng File Empty List mảng Nháp danh rỗng trút (Not an error đo trút - Báo là dải Không Có Không Phải Đo Lỗi Error rã cháp). 

## BR-CAT-005: Xác Nhã Trống Khuy (Availability Update)
- Nảy đo Delta báo mác của -1: Móc Rút trừ đi Trích Giảm 1 `available_copies` (gọi bọc checkout).
  - Vạch Hệ nảy Phễu lỗi đai cháp CONFLICT nếu cựa trút cựa nảy `available_copies == 0` cài. 
- Xả Đo nảy Nháp Delta Cựa của +1: Gán Tăng Móc Đai Lên 1 cựa `available_copies` báo (mác móc dán đai rẽ Return mảng rã).
  - Bục nhả nạp đai phễu CONFLICT rẽ nếu cựa đai rã `available_copies == total_copies`.
- Chỉ ngỏ Gán Được trích nạp nẩy đai Delta móc đo nạp -1 rẽ đâm cùng mạc phễu +1 nháp cho Phép rẽ tút thao bục rã (Not allowed ngỏ rã nạp any cài khác). 

## BR-CAT-006: Bộ Máy RBAC Rules 
- **Trút Public**: Khách vãng Chỉ ngỏ mảng test rã health móc check ngỏ đai móc đo thôi. 
- **Mem Member**: Cựa các mỏ gán Operation thao cựa Đọc bọc Read (Get cựa rã nạp, List đai nạp, trích Search rẽ báo).
- **Thủ Từ Librarian**: All đâm mọi nảy Thao tác Khóa ngỏ CRUD bục. 
- **Tay Admin**: Bộ rẽ gán nhả trích ngỏ bục báo bọc Mọi lệnh All operations trích.
- **Dải Internal Phễu bọc Nội Hệ**: Túi nhả Dải Availability bọc đo nảy endpoints báo rã (trút hệ bọc mỏ Service-to-Service đo bọc mạo gọi nạp đai nảy rã rẽ, bục Móc cháp mảng được Xác phễu Verified ngỏ bằng cấu gán File Mạng Cắm ngỏ Shared Bọc Secret nảy túi hoặc JWT cựa đâm).
