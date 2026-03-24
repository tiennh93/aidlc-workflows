# Phương Thức Của Các Thành Phần Bọc (Component Methods)

## Phân Hệ Catalog Service

### Các Hàm Của bục BookService ngỏ

| Hàm (Method) | Đai Đầu ngõ Vào (Input) | Lối Ra bọc (Output) | Tác Dụng cựa (Purpose) |
|--------|-------|--------|---------|
| `create_book` | `BookCreateRequest(title, author, isbn, category, total_copies)` | `Book` | Tạo cuốn Book mới mảng, tự cấu available_copies nảy bằng total_copies ngỏ |
| `get_book` | `book_id: str` | `Book` | Truy mảng lấy Book đai bằng mã bục ID hay nảy móc nảy raise phễu NOT_FOUND lỗi |
| `list_books` | Rỗng None đai | `List[Book]` | Trả nháp dải Toàn Bộ cựa nảy Sách |
| `update_book` | `book_id: str, BookUpdateRequest` | `Book` | Nháp Cập Nhật Mảng gán thông tin, móc phải Verify test báo cho cựa total_copies mảng >= số đai đang checked_out cựa |
| `delete_book` | `book_id: str` | `None` | Cựa Xóa nháp Trích Sách (nếu bọc Ngõ Không rã Ai nảy active checkouts) |
| `search_books` | `query: str, category: str, available: bool` | `List[Book]` | Check khớp ngỏ chuỗi Substring bọc vô bục title/author, cộng cựa có mác optional lọc đai filters ngỏ |
| `check_availability` | `book_id: str` | `AvailabilityInfo(book_id, title, total_copies, available_copies)` | Cổng nảy API chóp Ẩn mảng nội bộ cho túi Lending Service truy nháp |
| `update_availability` | `book_id: str, delta: int` | `Book` | Nảy Móc cộng trừ Adjust rã đai báo available_copies của 1 ngỏ (+1 trút hay xả báo bục -1 cựa) |

### Các Hàm Của dải BookRepository bục

| Chóp Hàm (Method) | Cự Đầu Vào trích (Input) | Nhả Loại Ra ngõ (Output) | Nháp Lệnh Làm xả Gì (Purpose) |
|--------|-------|--------|---------|
| `create` | `Book` | `Book` | Mảng Lưu xuống đai Persist rã mảng cuốn Sách bọc mới |
| `get_by_id` | `book_id: str` | `Book | None` | Mác Gọi nhả nảy Truy Cựa Retrieve đo bằng nảy Primary rẽ Khóa key cựa |
| `list_all` | mảng None | `List[Book]` | Gọi bọc Đọc bọc Hết dải nảy records mạc |
| `update` | `Book` | `Book` | Nạp Cập báo Đai Nhật Bản record cựa đâm đã tồn xả tại đo |
| `delete` | `book_id: str` | `None` | Nảy Hủy Xóa Remove nảy Móc record bục |
| `search` | `query: str, category: str, available: bool` | `List[Book]` | Cửa cựa Tìm bọc bục Quét trích kèm mảng bộ Lọc rã filters |

---

## Phân Hệ Lending Service

### Các Hàm Của mảng AuthService phễu

| Hàm (Method) | Ngõ Đầu trích Cáp Vào (Input) | Nhả Rã Lõi Báo (Output) | Túi Nảy Mục Đích rã (Purpose) |
|--------|-------|--------|---------|
| `hash_password` | `password: str` | `str` | Nạp Băm Bcrypt ngỏ hash đai |
| `verify_password` | `plain: str, hashed: str` | `bool` | Kê Xét mảng pass Verify cự password rẽ |
| `create_token` | `member_id: str, email: str, role: str` | `str` | Đẩy JWT ra trích kèm 24h cựa nảy hạn đai expiry |
| `decode_token` | `token: str` | `TokenPayload(member_id, email, role)` | Test Đai nảy Validate rã nháp kèm Phân đai Giải đo decode |

### Các Hàm Của MemberService mảng / MemberRepository bọc

| Bước Hàm (Method) | Đầu mác Gắn Dải (Input) | Khóa nảy Trả (Output) | Ngỏ Mục báo Trích Đích (Purpose) |
|--------|-------|--------|---------|
| `register` | `MemberRegisterRequest(name, email, password)` | `Member` | Nảy Lệnh Tạo đứt cựa tài khoản bục có nạp băm bcrypt bọc hash |
| `login` | `email: str, password: str` | `str (JWT)` | Trích Thẩm Authenticate xả và gán Trích nảy nhả Token bục |
| `get_profile` | `member_id: str` | `Member` | Tự ngõ mảng nạp xem Self-service móc cự profile chóp |
| `update_profile` | `member_id: str, data` | `Member` | Trút Cập ngỏ đổi Tên đai Update xả name/email cựa |
| `deactivate` | `member_id: str` | `Member` | Ngắt Cháp cự Set is_active = đai False |

### Các Hàm Của bục CheckoutService ngỏ

| Nhả Phương Thức (Method) | Bộ Input nạp Xả (Input) | Nhả Output xả Móc (Output) | Ngõ Tác mạc Dụng (Purpose) |
|--------|-------|--------|---------|
| `checkout` | `member_id: str, book_id: str` | `Checkout` | Trọn bọc gói Full cự validation nảy kèm móc create xả mượn |
| `return_book` | `checkout_id: str, member_id: str, role: str` | `ReturnResult` | Móc nạp Lệnh Trả báo Return mỏ + đo kéo fees nạp cựa + vách lôi bộ holds ra xử |
| `renew` | `checkout_id: str, member_id: str` | `Checkout` | Nới nạp cựa Rộng nhả ngày mác due date ngỏ bục |
| `list_checkouts` | `member_id: str, status: str` | `List[Checkout]` | Cự Trích Filter rẽ mảng lọc bằng đo Ngỏ status báo |

### Các Hàm Của bọc HoldService nảy

| Trút Hàm cựa (Method) | Giá Input bọc nảy | Đáp Ra nảy bọc (Output) | Giải ngõ Đích bọc (Purpose) |
|--------|-------|--------|---------|
| `place_hold` | `member_id: str, book_id: str` | `Hold` | Nạp Test Validate bọc nhả + Tự nảy tạo rã dải xếp hàng FIFO |
| `cancel_hold` | `hold_id: str, member_id: str, role: str` | `None` | Phễu Cancel hủy rẽ + móc cựa Xếp nảy lại reorder hệ queue rã |
| `get_holds_for_book` | `book_id: str` | `List[Hold]` | Trích mạc Hàng cựa chờ trích Queue rẽ mảng dành nạp cho book đo |
| `get_member_holds` | `member_id: str` | `List[Hold]` | List cựa Mác bộ ngỏ holds đứt thuộc rã bọc cựa Member |
| `fulfill_next_hold` | `book_id: str` | `Hold | None` | Đai Cự Kéo Nhả Chóp Thằng số Nhất first Mảng waiting cựa lên → đẩy rã ready |

### Các Hàm Của bọc FeeService nảy

| Lệnh cựa (Method) | In nạp đai (Input) | Đóng Bọc cựa (Output) | Mác Vai bọc Trò nảy (Purpose) |
|--------|-------|--------|---------|
| `calculate_late_fee` | `due_date, return_date` | `Decimal` | Quy cựa $0.25/mỗi nảy ngày, móc Cắn gán Chặn Cap là bọc $10 |
| `create_fee` | `member_id, checkout_id, amount` | `Fee` | Xuất Ghi đai mảng fee ngỏ rẽ Record nạp |
| `get_member_fees` | `member_id: str` | `List[Fee]` | Ráp xả List phễu báo fee ngỏ |
| `get_outstanding_balance` | `member_id: str` | `Decimal` | Tính nảy Phễu Tổng xả Rẽ Nợ bục Chưa Đóng Total rã |
| `process_payment` | `member_id, amount` | `Payment` | Khâu cựa Ghi đai nháp Lệnh Mảng Ứng Apply payment đai bục |
