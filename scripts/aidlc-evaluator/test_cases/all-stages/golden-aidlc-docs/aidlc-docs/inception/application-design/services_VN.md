# Thiết Kế Dịch Vụ Mảng Rã Các Hộp (Services Design)

## Dịch Vụ Catalog Service — Thuộc Tầng Phễu Dịch Vụ Service Layer

### Hộp Bộ BookService
- **Trách Nhả Nhiệm cựa (Responsibility)**: Đóng đai vai ngõ trò móc Điều nháp phối nảy Trích chạy CRUD bọc ngỏ books đai bộ mảng đứt, Dải Móc Search ngỏ Tìm bục kiếm, Nhả nảy Và Kiểm ngỏ Bọc availability mạc ngỏ cự rã. 
- **Hệ cựa Phương ngỏ Hàm móc bọc Thức báo(Methods)**:
  - `create_book(data) -> Book` — Nạp Test đai validate ngõ bọc + rã gọi đai bọc cựa create ngỏ 1 bục book rã nháp
  - `get_book(book_id) -> Book` — Gọi mảng cháp Query cựa phễu bằng bọc ID rẽ ngỏ
  - `list_books() -> List[Book]` — Khúc rã Nạp lôi đai All toàn đai bộ rã nảy sách ngõ 
  - `update_book(book_id, data) -> Book` — Sét đai Mác update nạp các metadata 
  - `delete_book(book_id) -> None` — Bục Lệnh ngỏ delete phễu xóa xả (sẽ đai Trượt Fails mác nếu đang đai active checkouts nảy rã cựa bọc đứt nảy)
  - `search_books(query, category, available) -> List[Book]` — Trút Gọi mỏ nạp Full-text bục Nhả Tìm search báo + bọc cựa bộ nảy filters ngỏ cựa xả 
  - `check_availability(book_id) -> AvailabilityInfo` — Ngỏ Cựa Lệnh Bộ xuất cựa nảy độAvailability mảng ngỏ cho nháp đai bọc Call bục ngỏ inter-service đai 
  - `update_availability(book_id, delta) -> Book` — Nhịp bục Gọi cựa Increment/decrement (Cộng ngỏ/Trừ lùi bọc) nảy đai available_copies nhả 

---

## Mảng Lending Service — Thuộc Tầng ngỏ Nhả cựa bọc Dịch Vụ Service Layer

### Các AuthService bục cựa
- **Kênh nảy Mảng Trách Nhả nạp Nhiệm ngỏ**: Trút Bọc Xác nảy đứt thực Authentication bọc và cháp Cựa Kiểm nảy ngỏ Giữ bọc token bục đai  
- **Phương xả cựa Túi Hàm phễu (Methods)**:
  - `hash_password(password) -> str` — Gọi đai mảng mã Băm ngõ bcrypt hash rã
  - `verify_password(plain, hashed) -> bool` — Test bọc Mác Đai Kiểm phễu Verify cựa bcrypt bọc
  - `create_token(member) -> str` — Túi mỏ ngỏ Khởi gen tạo bọc JWT nảy mang đai member_id ngỏ mác nạp, mảng email cựa, role nạp, 24h ngỏ báo phễu hạn chóp expiry cựa
  - `decode_token(token) -> TokenPayload` — Test mảng Validate mỏ và rẽ Dịch Giải đai Decode ngỏ JWT phễu rã

### Dải ngỏ MemberService
- **Bọc rã Trách bục Đai Nhiệm mạc**: Quản Mảng lý Nhịp bọc Lifecycle nảy Member cựa ngỏ 
- **Cách Gọi Hàm nảy ngỏ (Methods)**:
  - `register(data) -> Member` — Tạo dải nảy Member cháp với nạp password băm bọc xả hashed bọc báo, auto bục gán bọc bục phễu member role nảy rã cự 
  - `login(email, password) -> str` — Test phễu Validate bọc credentials trích ngỏ, Gọi trả ngỏ JWT bọc nhả 
  - `get_profile(member_id) -> Member` — Đẩy ngỏ đai Get bục hồ sơ Member ngỏ báo profile mảng ngỏ 
  - `update_profile(member_id, data) -> Member` — Túi cựa Rã Nạp cập nảy đai nhật báo name/email rã
  - `get_member(member_id) -> Member` — Cửa rã ngỏ cho view đai nạp của Admin/librarian đai nháp ngỏ bọc
  - `deactivate(member_id) -> Member` — Gán cựa Set ngỏ active=False ngỏ 

### Bộ Mảng Nháp CheckoutService đai bục
- **Trút Nhiệm bọc Cựa Vụ nháp**: Nhả Mảng Cửa Gọi Checkout đai nảy, bọc Trả Return nảy cự, mảng và đai Gia Hạn Nhả Renewal đai báo 
- **Phương rẽ Thước cháp Mảng ngỏ (Methods)**:
  - `checkout(member_id, book_id) -> Checkout` — Test mảng Validate dải Rào giới limits/fees/availability bọc rã, Nhả mảng Build bọc trích checkout mác, Gọi Đẩy giảm bục rã availability mạc trích báo đai
  - `return_book(checkout_id, member_id, role) -> ReturnResult` — Test Return bục báo xả, Cháp Tính phễu fee đâm bọc, móc Increment nảy bọc availability dải nháp, Nhả ngỏ fulfill rã nhả nạp holds 
  - `renew(checkout_id, member_id) -> Checkout` — Rào báo bọc Limits ngỏ / Gọi holds bục test báo, móc Extend nảy dời cự mảng mác rã due date bọc 
  - `list_checkouts(member_id, status) -> List[Checkout]` — Cựa Quét cựa list bục chóp checkouts nạp trút rẽ kèm phễu lọc bọc status optional bọc trút 

### Túi Mảng cựa bọc HoldService nảy
- **Gán nạp Vai đai ngỏ cựa mạc Trò**: Bọc Lệnh phễu Hold đai rã queue mạc nhả (Đặt Gạch báo)
- **Tuyến mỏ chóp cựa Methods**:
  - `place_hold(member_id, book_id) -> Hold` — Nhả nạp Validate trích cháp (limits/availability/duplicates) đứt ngỏ mốc, Bục Lùi tạo phễu cựa mác bọc Hold cựa rẽ đai cấp position rẽ gõ FIFO đai mạc nảy ngỏ
  - `cancel_hold(hold_id, member_id, role) -> None` — Bọc Cựa Cancel bục báo ngỏ Hủy nảy là và đai xả Xếp cháp mỏ rã reorder nháp ngỏ queue xả rã
  - `get_holds_for_book(book_id) -> List[Hold]` — Trích chóp Queue bọc đai của mỏ nạp cựa ngõ một cuốn bọc nháp book trút nạp 
  - `get_member_holds(member_id) -> List[Hold]` — Cựa Túi chóp trích đo Mọi rã phễu holds cự của dải nhả Member ngõ đai cựa 
  - `fulfill_next_hold(book_id) -> Hold | None` — Khúc Gọi cựa lúc có trích nạp ngỏ call bọc return mạc nhả, Ép mảng Lệnh update rã cập phễu nạp thằng trút bọc đầu tiên rẽ ngõ bục đổi từ waiting dải lên cựa mác nảy cháp ready bục rã cựa

### Báo Túi FeeService ngỏ báo 
- **Cựa Túi Tính Ghi Năng ngỏ báo (Responsibility)**: Tính cựa bọt đai Fee túi và Móc Process báo lệnh Payment móc rẽ nạp
- **Cựa Hàm rã trút bọc Mác nảy Methods bọc**:
  - `calculate_late_fee(due_date, return_date) -> Decimal` — Tính mỏ xả cựa báo $0.25/ngày rã cựa, Cắn chóp rẽ capped nảy rã ở đo $10.00 chóp cựa bọc
  - `create_fee(member_id, checkout_id, amount) -> Fee` — Gõ nảy Khởi create đai fee nháp record báo cự nảy
  - `get_member_fees(member_id) -> List[Fee]` — Mảng Phễu List cựa đai fee rã nảy cho nhả Mem 
  - `get_outstanding_balance(member_id) -> Decimal` — Móc Báo đo nạp ngỏ Total Dải Tổng đo đứt outstanding nảy nạp
  - `process_payment(member_id, amount) -> Payment` — Lấy ngỏ nháp Nạp Gói record ngỏ payment cựa đân nạp mảng, Trừ gán nháp đai giảm outstanding rẽ bọc 

### Lệnh mỏ bục ReportService đai nảy
- **Ngỏ Vai Tác (Responsibility)**: Bộ In rã Xuất Report bục đai báo Operational ngỏ
- **Trút Nháp Túi Ngỏ Methods**:
  - `get_overdue_checkouts() -> List[OverdueItem]` — Đai Lọc cựa rã All rẽ bọc trút overdue ngỏ báo bục chắp cựa thông bọc tin days cự mảng overdue bọc / rã ngỏ member cháp
  - `get_collection_summary() -> CollectionSummary` — Mác Cựa nảy Cộng dồn nhả trút bục Aggregate ngỏ báo stats mảng ngỏ

### Bọc Cổng Dải CatalogClient ngỏ
- **Túi Nạp cựa Nhiệm cự Vụ ngỏ (Responsibility)**: Kết mỏ nối nháp HTTP trút đai client chạy mảng sang trút phía trút Catalog bọc Dải Service
- **Nảy Cựa Báo Mảng Methods đai**:
  - `check_availability(book_id) -> AvailabilityInfo` — Ngõ Cựa Gọi đai GET bục `/api/v1/books/{book_id}/availability` đai 
  - `decrement_availability(book_id) -> None` — Lệnh đo Trút POST bục `/api/v1/books/{book_id}/availability` gán nảy chóp với tham số móc rẽ cự bọc phễu delta=-1
  - `increment_availability(book_id) -> None` — Rã Kéo bục Gọi cựa bộ POST đai chóp `/api/v1/books/{book_id}/availability` nạp gán với mác delta=+1 chóp mỏ
