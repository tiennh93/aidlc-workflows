# Phân Rã Sự Phụ Thuộc Của Các Thành Phần (Component Dependencies)

## Bảng Ma Trận Phụ Thuộc (Dependency Matrix)

| Tên Thành Phần cựa (Component) | Gọi Phụ Thuộc ngỏ (Depends On) | Cách Giao cựa Tiếp (Communication) |
|-----------|-----------|---------------|
| CatalogAPI (mảng Tuyến Routes) | cựa BookService, nảy AuthMiddleware | Mạng Direct Call bọc hàm trực tiếp |
| ngõ BookService | cựa BookRepository | Nháp Direct function nảy calls |
| BookRepository | mảng Database đai (bản abstract đai) | Báo Driver đai kết cơ sở dữ rã |
| AuthMiddleware (mảng Catalog) | Bộ JWT trích xài (PyJWT bục) | Trích Library đai call (gọi thư viện) |
| LendingAPI (nháp Tuyến Routes bọc) | Cựa Tất Cả đai nảy Lending services, bọc AuthMiddleware ngỏ | Gắn Direct đứt function bọc calls nảy |
| MemberService | Bộ MemberRepository nảy đai, bọc AuthService | Trút Định Direct hàm xả ngỏ |
| AuthService | cựa MemberRepository bọc, Mảng JWT/bcrypt trút libs | Library mạc call của đai thư viện rã |
| CheckoutService | Ngõ CheckoutRepository cựa xả, nạp HoldService, trút FeeService, ngỏ báo CatalogClient | Móc Direct calls bục + nhảy HTTP đai |
| HoldService | Nhả HoldRepository bục bọc | Trút Direct xả function calls cựa ngỏ |
| FeeService | ngỏ FeeRepository bọc nảy | Nháp Direct nạp function mạc calls bục |
| ReportService | mỏ CheckoutRepository ngỏ, trút MemberRepository, cựa mác BookService bọc (bằng cục CatalogClient rã) | Đâm Direct calls bọc + báo HTTP mạng ngỏ |
| CatalogClient | Phễu Catalog Service API đai | Nháp HTTP trích bọc (bằng xả httpx) |

## Thiết Cựa Giao Tiếp Liên Dịch Vụ Mảng Rã (Inter-Service Communication)

```text
+------------------+         HTTP đâm báo (qua httpx ngỏ)         +------------------+
|                  | -----------------------------> |                  |
|  Túi Lending Service |   Bọc GET /books/{id}/availability | Trích Catalog Service |
|                  |   Ngỏ POST /books/{id}/availability|                  |
|  (Ở Port 8001 bục)      | -----------------------------> |  (Ở Port 8000 báo)      |
|                  |                                |                  |
+------------------+                                +------------------+
```

### Bộ Các Mô Hình Nảy Giao Tiếp ngỏ (Communication Patterns)

1. **Khâu Mượn Sách Checkout Flow**: Từ LendingAPI bọc → Móc CheckoutService xả → Nhả CatalogClient cựa → Đo Sang Catalog Service (nhả verify + nháp trừ decrement)
2. **Luồng bục Trả Sách Return Flow**: cựa LendingAPI → nảy gán CheckoutService mỏ → Bọc sang CatalogClient (cộng bọc increment) → Nảy mác HoldService đứt (vách fulfill ngỏ) → Dải ráp FeeService (đẻ rẽ create fee mảng nếu nảy trễ)
3. **Mảng Nhả Chóp Đặt Gạch báo Hold Placement**: Bọc LendingAPI nạp → phễu HoldService trích → Báo CatalogClient ngỏ → Tới Catalog Service mỏ (để verify tút availability = cựa 0)
4. **Trút Bộ Tìm Kiếm cựa Search**: Móc CatalogAPI → nảy gán BookService → Báo tới BookRepository cựa (bục chạy Trực Tiếp Direct, xả nạp không gọi chóp sang cựa cross-service)

## Lớp Phụ Thuộc Nội Bộ Từng Dịch Vụ (Internal Layer Dependencies)

```text
+-------------------+
|   Cựa API Routes      |  (Nhả Tuyến FastAPI ngỏ route mảng handlers)
+-------------------+
         |
         v
+-------------------+
|   Nháp Services        |  (Luật Business xả logic)
+-------------------+
         |
         v
+-------------------+
|   Bọc Repositories    |  (Hệ Data trích access - trút dạng abstract)
+-------------------+
         |
         v
+-------------------+
|   Gán Database        |  (Bộ Concrete đứt mảng implementation đai)
+-------------------+
```

## Bảng Phễu Quyền Mác Sở rã Hữu (Data Ownership bọc)

| Loại Thực Thể ngỏ (Entity) | Dịch vụ Nắm Giữ cựa (Owning Service) | Mảng Lưu Storage nảy |
|-------------|---------------|---------|
| mảng Book (Sách) | Ngõ Catalog Service bục | Phễu Nháp Catalog DB ngỏ |
| nảy Member bọc (Thực) | Nhả Lending Service cựa | Ổ Lending mảng DB cựa |
| nảy Checkout xả | Trút Lending Service ngỏ | Ổ Lending DB đai báo |
| phễu Hold (Đặt trước) | Trích Lending Service bọc | Ổ chóp Lending đai DB |
| mỏ Fee (Tiền Phạt nảy) | Đai Lending Service nảy | Nháp Lending ngỏ DB bục |
| Payment nảy (Thanh Ngỏ Toán) | Bục Lending Service bọc | Rã Lending bọc DB |
