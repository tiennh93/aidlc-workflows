# Các Thành Phần Ứng Dụng đai (Application Components)

## Nhóm Dịch Vụ 1: Bộ Catalog Service

### Thành Phần cựa: BookRepository
- **Lý Trích Do cựa (Purpose)**: Làm mảng ổ Lớp đai Data Access bọc layer nảy thao rẽ tác lưu ngõ Book (sách)
- **Đai Phận Trách Nhiệm bọc (Responsibilities)**: Thao dải tác xử CRUD cự chạy records ngỏ bộ Book, nhả xử lý cựa queries đai nháp Tìm Kiếm ngỏ, Và cựa Nháp Updates bộ nhả Availability bọc sẵn có.
- **Túi Giao Nhả Diện (Interface)**: Pattern gán chóp repository trừu tượng bọc Abstract (Giúp nạp ngõ gỡ ráp nảy Mạng đổi đai qua db đai khác bục lúc swapping bọc)

### Thành Phần ngõ: BookService
- **Cựa Mục Báo Đích ngỏ (Purpose)**: Bộ rã Não Lõi Business gán logic cự xả cho hoạt động trút catalog cựa
- **Nháp Trách Nhiệm cự (Responsibilities)**: Test Validate cự trích sách bục ngỏ, Trút Nhịp điều phối đai lệnh gọi Search ngõ báo, Nạp Kiểm quản ngỏ Quản bọc Availability rã, Thiết Móc Đai Ép bục Các Enforcement rã bộ luật Business rule đứt chóp đai
- **Trút Nháp Kết Nảy Bọc (Interface)**: Được nạp đai Call báo Gọi bởi móc nháp dải các Cựa hàm nhả Handlers bọc xả mảng của API Route rã.

### Thành Phần ngỏ: CatalogAPI (Bộ Tuyến Routes)
- **Cựa Tính phễu Năng (Purpose)**: Trút Là các hàm ngỏ chóp REST API mã endpoint đai chóp nảy handlers bọc cho Catalog Service nháp
- **Trút Phạm rã Vi (Responsibilities)**: Xắt Parsing bục xả Request bọc đai, Format rẽ mạc mốc Response nhả bọc, Bóp ngỏ Đai Khống chế phễu RBAC nạp qua lệnh đai Middleware phễu, Nhả Đai Định nghĩa xẻ bộ Routes
- **Tuyến Endpoints**:
  - `POST /api/v1/books` — Ghi mạc sách cựa Add book (Librarian gán, Admin bọc)
  - `GET /api/v1/books` — Đọc dải List báo books (Cần nảy Auth ngỏ bọc authenticated)
  - `GET /api/v1/books/{book_id}` — Chặn Get rã báo book (Cần xấp Auth)
  - `PUT /api/v1/books/{book_id}` — Nạp Cập báo Update rã ngỏ book (Librarian cựa, Admin phễu)
  - `DELETE /api/v1/books/{book_id}` — Rã Cựa Xóa nhả ngỏ Delete book cự (Librarian nảy, Admin)
  - `GET /api/v1/books/search` — Nạp Tìm ngỏ Search báo chóp books (Auth ngỏ)
  - `GET /api/v1/catalog/health` — Báo test Health check chóp khỏe (Nhả nạp Mở bục public)
  - Bục Chạy Internal (Nội Bộ): `GET /api/v1/books/{book_id}/availability` — Nạp Mỏ Check Nhả độ Trống availability bục móc (Dải cự Giao đai Tiếp service-to-service)
  - Cựa Đai Internal: `POST /api/v1/books/{book_id}/availability` — Báo Update mác nhả availability cựa (Service-to-service trích móc đai)

### Thành Phần bọc: Cấu AuthMiddleware đai (Phía Catalog rã)
- **Tác Dụng cựa (Purpose)**: Nảy Bóc Kiểm Validate JWT cự đai và Mạc Tách cự role ngỏ extraction bọc nạp cho mạng Catalog Service
- **Nạp Nhiệm rã Vụ đai (Responsibilities)**: Nảy Test báo xác đứt Verification mã cựa Token xả, Che phủ phễu Rào nảy bục Protection ngỏ route đai rã bằng bộ role-based đai, Gắn cựa Làm bục Rõ Enrichment nạp thêm cho túi mác Request ngỏ mảng context xả đo cựa

---

## Ổ Dịch Vụ 2: Lending Service

### Thiết Mảng Thành Phần: MemberRepository
- **Giải báo Cựa Mục Đích đai (Purpose)**: Data Access nạp mảng rẽ mốc trích đai tầng Records ngỏ Member cháp
- **Tiêu Trách Nhiệm ngỏ đai (Responsibilities)**: Chóp nảy mảng Thao Bộ CRUD cự báo record rã đứt cho mảng bọc Member nạp, Ép cựa Lệnh nảy Mảng Độc Nhất đâm enforcement cháp đứt nạp Email cự uniqueness đai ngỏ

### Cựa Mảng Thành Phần: MemberService
- **Ngỏ Lý Do ngỏ (Purpose)**: Cựa Bọc Lõi Nhả đứt Business Logic trút cho mảng Bộ nhả Member ngỏ cựa Management móc đai quản lý  
- **Tiêu Trách Nhiệm bục đai**: Đai Lệnh cựa Khâu bọc Register, Quản nảy Profile trích ngỏ, Cháp mảng Nhả Nảy Băm móc Password trút hash, Đai Set ngắt cựa tài nảy khoản ngỏ Deactivation đai 

### Đai Mảng Thành Phần bọc: AuthService
- **Ngõ Nhiệm cựa Vụ bọc (Purpose)**: Khâu cựa Gán nảy Authentication xác đai thực rã đai và Cục ngỏ Quản Trút JWT token đai 
- **Vai trò chóp Đai Thao Tác (Responsibilities)**: Cựa Nhả Gọi Test Login phễu validate đai bọc, Gen mảng Phát đai Sinh rã JWT nhả nảy, Test rẽ Check nạp Verify đứt JWT đai ngỏ cựa, Tính rã bọc Băm cựa password gõ đứt bằng xả bcrypt cựa

### Tiêu Thành Phần ngỏ: CheckoutRepository
- **Lý ngỏ đai bục Do (Purpose)**: Đầu Data ngỏ Access mác đai cho Cựa Record ngỏ Checkout nảy trích 
- **Biến Trách Nhiệm (Responsibilities)**: Lệ CRUD ngỏ đai nảy cho Checkout nảy records bọc, Queries mạng móc Đọc Nhả các trích Mảng Checkout đai Active/ cựa nhả Hay Trễ nạp (overdue bục ngỏ).

### Đầu Mũi Thành Phần cựa: CheckoutService
- **Dải Nhả Đích nháp (Purpose)**: Thiết bục Business logic phễu chóp cho Checkout, gán Nảy Móc Return cựa bục, và Khâu nạp Renewal đâm
- **Lệnh đai Thao (Responsibilities)**: Nảy Test Validation bọc checkout ngỏ đai (Rào limits đứt cựa, Đứt Fees bọc rã, Nhả Xả Gói Availability mạc), Làm cựa Process nảy khâu cựa Nhã Return rẽ (Tính dải nhả fees, Cựa Báo nháp Tít mảng Trigger đai kích nạp khâu hold rã mạc), Xả Xác nháp nhận đứt Renewal mạ c cựa (Rào Limits đai nảy, bọc Kiểm tra đâm holds nạp cự), Bơm rã Lệnh Gọi Call ngỏ rã Inter-service đai phễu trút nảy sang dải mảng Catalog Service bọc.

### Máy Component trích: HoldRepository
- **Nháp Lệnh Mục nảy (Purpose)**: Trút Ngõ Data access cựa mạc cho Records rẽ Hàng ngỏ Hold đai nảy 
- **Túi Trách Nhiệm bọc (Responsibilities)**: Khâu nạp CRUD rã nảy mảng cho hold records, Bộ nảy Gán Quản mỏ Queue hàng nảy FIFO, Quét cựa Queries nảy cho mác Vị nạp Queue position rẽ đo bọc ngỏ.

### Túi Component ngỏ: HoldService
- **Đai Tính Bộ (Purpose)**: Mảng Business bục Xả rẽ đai nảy logic gõ cho Quản phễu Nhả Hold cựa 
- **Ngỏ Vai rã mác (Responsibilities)**: Rào Validate bục gán Mác Đặt bọc nạp (Rào bọc Mảng limits ngỏ đai, Gọi đai bọc check availability, Dò đai trùng lặp đai duplicates xả cựa), Phễu Gọi nạp Cancel mảng Hủy móc nạp với cựa Nháp Đôn rã nảy Đứt bọc reorder dải queue, Test phễu rã Móc Fulfillment ngỏ mảng xả lúc Return bọc.

### Nảy Thành Phần cựa: FeeRepository
- **Móc Công bọc Năng Phễu (Purpose)**: Bọc Khâu nạp Data Access nhả ngỏ fee mạc bọc phạt cựa bọc và cháp Thanh ngỏ Payment toán mạc
- **Bộ Nhiệm Nảy (Responsibilities)**: Ngõ CRUD cựa fee móc trút records ngỏ, Tra Track nảy khâu mảng payment cựa bọc, Lệnh rã Check đai Outstanding phễu ngỏ cựa balance nạp đâm balance (nợ rã bục nảy đứt) 

### Component nạp đai: FeeService
- **Ngỏ Vận Mảng Hành bục (Purpose)**: Business dải cựa logic chóp cho Nhóm gõ Fee nảy mảng management
- **Cựa Hàm rã Nhả bọc Lệnh**: Bộ ngỏ đai Nảy Sinh nạp create báo fee khi trút nảy Trễ đai Return mảng rẽ ngỏ, Code process đứt cho Payment nảy, Nạp Của đai Tính cháp Cựa Balance nảy báo 

### Bọc Thành Phân đứt: ReportService
- **Cựa Chức Mảng Trích Năng rẽ (Purpose)**: Mảng Bộ gán Business rã phễu đai nạp logic báo cho cháp nảy Reporting
- **Trút Nháp Gán Gọi đai (Responsibilities)**: Trích Đai Nạp Gom nảy Cục Overdue checkout bọc, Tính nảy Bộ Summary dải nạp của bọc đứt Toàn bộ cựa bục bọc Collection ngỏ báo. 

### Bộ Công Cụ Mảng: CatalogClient
- **Lý Trút cựa Bục Đo (Purpose)**: Mảng Nháp Gọi cựa HTTP ngỏ Client bọc phễu để bắn nạp sang ngõ Catalog đai Service trích ngỏ đo. 
- **Cựa Nảy Trách báo Nhiệm rẽ**: Bục Nhả Đút mã verify nảy check đứt sách bọc tồn nảy Existence cựa tút, Móc Nhả Truy độ nảy Availability cháp bọc bục, Đai nạp Móc Cập báo Nhật nảy Xả Availability trích Updates nạp bọc ngỏ cự (increment/decrement phễu bù) 

### Phễu API rã: LendingAPI (Bộ Routes)
- **Tác Mục Dụng Tiêu đai**: Mảng Hàm rã Nhả REST đai API xả endpoint chóp handlers cựa gõ cho mảng Lending nảy Service rẽ đai móc
- **Trút Bộ Cựa Trách bọc Nhiệm nảy**: Mác Khâu rã xả Parse báo, Nạp Mạng Format nảy responses đai móc trút, Khâu ngỏ Ép ngõ nạp RBAC rã bọc enforcement, Tuyến rã Bộ đai Route mảng Definitions ngỏ.
- **Tuyến mỏ Endpoints nháp**:
  - `POST /api/v1/members/register` — Đăng xả bọc ký Register nạp phễu (public ngõ cựa mở trích)
  - `POST /api/v1/members/login` — Test gọi Login (bọc Mở nảy)
  - `GET /api/v1/members/me` — Nạp Hồ sơ rã Cá xả nảy cựa nhân bục My profile (Buộc bọc Auth)
  - `PUT /api/v1/members/me` — Đai Update nạp cựa mảng profile mác (Cắm rã Auth nạp)
  - `GET /api/v1/members/{member_id}` — Soi cựa nảy Thông Member ngỏ (Mác Admin đai, Librarian đâm rẽ)
  - `PUT /api/v1/members/{member_id}/deactivate` — Nãy Tắt gõ Active nạp mảng báo (Admin rã phễu)
  - `POST /api/v1/checkouts` — Ngỏ Tính Checkout đai mảng xả (Buộc bọc có cựa Auth)
  - `POST /api/v1/checkouts/{checkout_id}/return` — Mác đai Trả nảy Return bọc cự (Auth báo)
  - `POST /api/v1/checkouts/{checkout_id}/renew` — Đứt Trích Sinh gia cựa hạn Renew nảy rẽ (Cắm Auth ngõ)
  - `GET /api/v1/checkouts` — List bọc cục xả cự mỏ checkouts báo cựa (Nháp Có ngỏ nạp Auth rã)
  - `POST /api/v1/holds` — Đai Đặt mác Hold phễu chờ rẽ mảng (Có Auth)
  - `DELETE /api/v1/holds/{hold_id}` — bục nạp Hủy Cancel cựa Hold (Có ngỏ Auth dải)
  - `GET /api/v1/holds` — Đai List ngỏ holds (Auth bọc cựa)
  - `GET /api/v1/holds/me` — Cựa Phễu mỏ Xem đai bục My holds ngỏ rã (Auth ngỏ nạp)
  - `GET /api/v1/fees/me` — ngỏ Dải My xả cựa xả fees bọc xả (Auth bọc đứt nảy)
  - `GET /api/v1/fees` — List bọc cựa fees ngõ đai (Cựa Admins phễu, Librarian bục)
  - `POST /api/v1/fees/payments` — Móc Gắn Process nảy phễu cựa payment bục toán đai (Amin, Librarian)
  - `GET /api/v1/reports/overdue` — Bọc Cựa In báo mạc cáo dải cựa overdue nhả móc (Admins, Librarians ngõ cự)
  - `GET /api/v1/reports/summary` — mảng Báo Cáo đứt nhả rã cựa ngỏ summary (Admin móc)
  - `GET /api/v1/lending/health` — dải Nảy Gọi mảng ngỏ Health check nảy báo cựa (Mở mạc trút cựa public ngỏ bọc)

### Mảng Thành Phần bọc: AuthMiddleware (Phục Dải Phía ngõ nạp Lending đai cựa)
- **Tác Dụng cựa (Purpose)**: Bộ Test bọc Validate ngỏ JWT ngỏ đai cựa và mạc Lọc bọc ngõ Tách mảng bộ nảy Role rã (Vai ngỏ) bục nạp cho mảng nhạp Lending nảy Service cựa
- **Khối Cựa Lệnh bọc Phận rã (Responsibilities)**: Móc Gọi bục Token đai Nhả verification ngõ báo, Nạp Giới tuyến mỏ Route ngỏ protection dải ngỏ chặn cự bọc băng role mác đai dựa trên role-based rã, Túi Nạp thêm Enrichment ngỏ Request context rã nhả ngỏ 

---

## Ổ Bọc Nhóm Concerns đai Rã Phễu Bọc Rã Dùng ngỏ Chung rã (Shared Concerns cựa đứt đâm đai nảy)

### Trích cự Khâu Cross-Cutting dải: Mảng Structured Logic Logging đai xả ngỏ
- **Trút Nhỏ Mục báo Đích (Purpose)**: Làm mỏ bộ trút Config rẽ Tập đai ngõ Trung nhả Logging mảng báo cựa cho Cả ngỏ 2 Ổ nảy services dải đo nạp cự trích bọc 
- **Vai nảy Trò đai Nhiệm ngỏ bục**: Bọc Gắn Logger rã Request/Response ngỏ đai với mảng nảy Correlation cự IDs, Túi Trích Lọc Đai Cắt PII nảy trút, Đai nạp Rã Bọc Sinh ngõ JSON bục xả có đai cấu trúc. 

### Mảng Cựa Cross-Cutting: Lệnh Error Handling ngỏ bục 
- **Ngỏ Lý dải Do nạp cựa**: Khâu bọc cựa Gọi mạc dải Middleware cự Xử Nhả Đai Errors bọc Toàn Cục cựa (Global mỏ đai mảng)
- **Dải Công ngỏ Lệnh nảy bọc**: Câu chóp Cựa Lỗi đai unhandled nảy exceptions ngõ đai rẽ bọc, Móc Trích ngỏ Phản rã nạp mảng Hồi Responses đai ngỏ Standardized rã chuẩn lỗi, Bọc Lưu ngỏ báo Logs đai cựa mã lỗi mảng bọc Không dải đai Bị nạp Phô rã phễu Internals nảy đai xả 

### Móc Kênh bọc Cross-Cutting ngỏ phễu: Bọc Bao bì dải nạp báo Response xả đai nhả Envelope rã nảy đo chóp 
- **Bộ cựa Đai Thiết Kế nảy Mục Đích**: Mảng Cựa Format đai Phễu rẽ Mảng response API nảy báo Consistent báo ngỏ nhả
- **Túi Nạp đai Trách ngỏ rã Nhiệm bọc**: Bụng Ốp cựa Trút nhả Dải Bọc bộ Wrap ngỏ Trích cả nạp Responses nạp vô chóp báo nạp Định báo Dạng: `{"status": "ok/error", "data/error": ...}` nảy
