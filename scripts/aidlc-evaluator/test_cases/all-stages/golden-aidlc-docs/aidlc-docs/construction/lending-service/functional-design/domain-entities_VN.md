# Thiết kế Chức năng (Functional Design) — Lending Service — Các Thực thể Mô hình (Domain Entities)

## Thực thể: Thành viên (Member)

| Trường (Field) | Kiểu (Type) | Ràng buộc (Constraints) | Mô tả (Description) |
|-------|------|-------------|-------------|
| id | str (UUID) | Tự động sinh (Auto-generated) | Mã định danh duy nhất của Member |
| name | str | Bắt buộc (Required), mác max 100 chữ | Tên đầy đủ (full name) rã thành viên |
| email | str | Required, vách mác unique (duy nhất), max 255 chữ, email chuẩn đai báo format | Mã Email báo log login đai cự ngỏ |
| password_hash | str | Do máy rã System System-managed tự gán quản | Mác Mã Băm đai Bcrypt trích tút nạp hash Password bục |
| role | str (Dạng mảng enum) | "admin", "librarian", móc "member" | Quyền Mác Cựa RBAC Ngỏ Rã |
| is_active | bool | Ngầm đai Default: true | Cháp Mảng Đo Active bọc mỏ trút status ngỏ bọc Account |
| created_at | datetime xả (UTC) | Bọc Auto tự đai ngỏ sinh (Auto-generated) | Tính Mốc cựa rẽ Registration nảy mác bọc timestamp |

## Rã Thực Thể: Giao dịch Mượn Sách (Checkout)

| Cựa Trường Field | Gõ Kiểu rã Type | Rã Bọc Ràng Buộc (Constraints) | Miêu Tóm Rã Báo (Description) |
|-------|------|-------------|-------------|
| id | str báo (UUID) | Gọi bục đứt tự chạy (Auto-generated) | Cự Mã ngỏ Unique trích vách chóp identifier |
| member_id | str | Buộc Cựa Required, ngỏ Khóa trút Ngoại rã FK chiếu mảng Member | Khứa Mem Nhả Khách bục Xả Mượn (Borrowing) |
| book_id | str | Mảng Bọc Trích Buộc (Required) | Cựa Sách (Book bục mạc) trích nạp ngõ xẻ từ bục mảng Catalog đai Service |
| checkout_date | date trích datetime (UTC) | Nhả Auto-generated phễu rã đai | Thời nảy đai điểm Rã khi đai phễu Mượn nháp Checkout nảy bọc |
| due_date | bục datetime (báo UTC) | Lấy ngỏ rã `checkout_date + 14 ngày Mác days` | Thời Rã mảng Điểm báo Đến ngỏ nạp Hạn báo Trả (due) |
| return_date | datetime mảng bọc (UTC) | Cự Null rã mốc Mảng báo Null Phễu Mảng Móc tới rẽ lúc phễu return | Hồi Cựa mác Giờ Mốc ngỏ khi nạp Đã bọc Trả rã nảy Return |
| status | str (cháp Enum) | "active", nhả mảng "returned" | Mác Cựa Trạng ngỏ Thái bục đai checkout rã |
| renewal_count | int | Điểm Default: 0 cựa, nạp móc Max trích lên đai báo 2 | Ngỏ Điểm bục số Lần đai mảng Gia gán bọc nảy Hạn rẽ mạc |

## Bộ Thực Thể Tên: Gạch Sách Giữ Chỗ (Hold)

| Điểm Dải Rã Field | Kiểu gán mảng (Type) | Nhả Mảng Ràng cựa Buộc vách Constants | Giải xé Miêu báo Tả (Description) |
|-------|------|-------------|-------------|
| id | str (Mạc bọc UUID) | Ngỏ Auto nháp mảng gán generated | Mác Định nạp Dạnh ngỏ bọc DUy vách cựa Độc đai Nhất ngỏ  |
| member_id | str | Yêu Buộc mảng (Required), FK trút Ngoại cựa Mác tới rã Member | Người Mảng Bọc Nảy Nhả cựa Nháp Đi bọc trích Đặt (Requesting member cựa đai rã) |
| book_id | str | Buộc chóp Required | Gán mảng Sách (Book) nạp nhả ngõ mỏ từ trích dải bọc Catalog trích rã |
| hold_date | datetime mảng nháp (UTC) | Nhả Auto tự nảy phễu mảng ngõ sinh (generated | Thời cự rã nhả Gian Mảng nạp Bọc Khi gán Hold bục cháp Cắm xả (placed) |
| status | Mác str lốc mảng (enum) | "waiting", chóp đâm "ready", và bọc cựa "cancelled", mảng nạc trích đâm "fulfilled" | Nháp Mảng Của Trạng Hold nạp bọc status |
| queue_position | mảng Cựa bục Int | `>= 1` bọc cựa đai móc rẽ  | Túa Điểm rẽ Vị Nhả nạp cháp trí bọc nạp của ngỏ queue trút rã Mảng đo (Theo rã bục FIFO cựa rẽ) |

## Tổ Mảng Thực Thể Tên: Khoản Nợ Phí Phạt (Fee)

| Cựa Tên Trường Field | Bộ Type gán mảng (Kiểu) | Ngỏ Ràng mác Trích Đo Rã (Constraints) | Miêu Giải rẽ Description báo ngỏ cựa |
|-------|------|-------------|-------------|
| id | Ngỏ str rã (UUID) | Sinh ngỏ tút Auto (Auto-generated bọc) | Khóa ngỏ đai ID nhả bọc Duy Nhất xả rẽ |
| member_id | str | Có Bục Gán Required, trích mỏ Khóa rã FK vào rã Member | Nhả Mem mảng Thành báo Khách trút này là bục ngỏ có đai dính ngỏ fee |
| checkout_id | str | Báo Required, gán trút cháp FK chiếu ngỏ dải báo Checkout | Mạc Nảy Nhả Liên Đi nạp Nhã rã (Related rẽ) Checkout nảy rẽ mảng chóp |
| amount | Dạng bọc dải Decimal phễu | Mảng nháp lớn Hơn `> 0`, dải rã Max cài ngỏ móc hãm nạp rã chặn đai là `$10.00` | Tổng Trị rã Tiền cự của Fee nảy rã |
| status | bục str mạc xả (enum) |  "outstanding" cựa rã ngỏ nạp móc, "paid" bọc nảy | Túi Mác Rã Status Fee bục đai Của Mạc Trạng Thái nạp Phí rẽ |
| created_at | datetime xả đai (UTC) | Móc gõ System ngỏ Auto nhả rã | Tích Cựa Rẽ Khi rã mảng Móc nảy Fee vách cựa nảy được created đai ngỏ |

## Bộ Thực Thể Rẽ Tên: Đóng Tiền Mác Gán Thanh Nhả Toán Nháp (Payment)

| Cựa Mảng Trường nạp Field | Loại trích Type | Mảng Vách Ràng bọc Buộc ngỏ nảy cựa (Constraints) | Tính Mảng Nạp Giải ngỏ Báo Description rã đo |
|-------|------|-------------|-------------|
| id | Ngỏ cựa str báo (UUID) | Ngỏ Auto-generated | Mảng Unique ngỏ báo identifier |
| member_id | str | Required bọc đai, FK trích ngõ chiếu tới mảng phễu bọc Member ngỏ | Người ngỏ Rã nháp đang móc Paying đai nạp cựa nháp Member |
| amount | Decimal mảng ngỏ rẽ | Kéo bọc mỏ Lớn cựa Hơn `> 0` nảy mạc | Nạp Lượng Tiền chóp nảy Amount Payment mỏ cựa rã |
| payment_date | cháp datetime phễu (UTC) | Auto rã mảng generated by System bục đứt xả ngỏ | Thời đo bọc mác Khi mảng nháp Tiền chóp Payment bọc đai đã xả Ngỏ nảy process (Processed bọc ngỏ nảy) |

## Giá Trị Mô Hình Mảng Value Object: TokenPayload (Cục Payload Mang Chuỗi JWT)

| Tên đai Field | Loại Type | Mảng bọc Miêu Giải nháp cựa (Description) |
|-------|------|-------------|
| member_id | str | Khóa Mãng mã Nhả Member cựa rã nảy UUID |
| email | str ngỏ | Phễu Member cựa email |
| role | mác str bọc | Cấp rã Mác Role ngỏ Cựa role vách MEMBER đai ngỏ nạp |
| exp | datetime rã | Giờ nảy cựa Điểm nhả bọc Đứt ngỏ mảng EXP Token báo trút expiration |

## Phễu Tính Mảng Ngỏ Value Object rã: ReturnResult (Bọc Nhả Bộ Gán Gói mảng lúc Nhả Call Trả Return nảy Sách)

| Ngõ Trống Field rẽ bọc | Cái Type | Ghi Nháp Giải đai nảy Description |
|-------|------|-------------|
| checkout | Đai kiểu Checkout ngỏ | Cái rã Ngõ Checkout đai báo record cài ngập Record đai xả bục checkout rã đã nảy Update đai |
| fee | Bọc dạng Fee cựa | Trút None đai Nhả Không có (đai Nếu mốc Không báo phạt). Còn báo Lại Nếu nảy Applicable là Có đai Tính Khóa Phí Late Fee bọc  |
| hold_fulfilled | Mảng Type Hold ngỏ | Null None cựa (Nếu ngỏ bục Không chóp móc Đụng báo Hàng Hold cựa nảy). Hoặc cựa 1 báo nạp túi Hold đo nháp nào nảy được đai Giải Cứu mạc (fulfilled mạc nếu rã trút Applicable áp cựa). |
