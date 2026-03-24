# Kế hoạch Sinh Mã (Code Generation Plan) — Mảng Catalog Service + Mảng Lending Service

## Đai Bối Cảnh Hệ Unit (Unit Context)
- **Thư Mục Gốc Chạy Mảng (Workspace Root)**: `workspace/`
- **Bộ Mảng Loại (Project Type)**: Greenfield xả chạy đa cấu (multi-unit microservices)
- **Cấu Rã Trúc (Structure)**: `workspace/catalog-service/` và bọc `workspace/lending-service/`

---

## Các Nhịp Đo Dành Cho mảng Catalog Service (Catalog Service Steps)

- [ ] Lệnh bọc 1: Cấu phễu Rã thiết mảng trúc Project rẽ cho Catalog Service (`pyproject.toml`, túi rã `src` layout, túi `tests` nảy)
- [ ] Lệnh Bước 2: Dựng mảng Thực thể túi Mô hình Domain (Nảy `Book`, cháp bọc `AvailabilityInfo`)
- [ ] Nhịp 3: Tạo ngỏ rã các bọc request/response models đo cho file Pydantic rã
- [ ] Khởi bọc 4: Dựng phễu các dải Modules lõi bọc (exceptions nảy, response rẽ helpers, nạp logs logging, trút config bọc đo)
- [ ] Bước dải 5: Dải mác Dựng ổ Abstract rã repository ngỏ kèm cài Trích mảng In-memory rã bản implementation
- [ ] Nhịp bọc 6: Nảy mảng Tạo dải BookService (Tính ngõ business logic rã móc)
- [ ] Lệnh ngỏ 7: Ghép auth đai middleware bọc (Tạo rẽ validation cho JWT bục)
- [ ] Bước nạp 8: Dải cựa mảng Tuyến API Routes rẽ và ráp cài app FastAPI bọc mảng
- [ ] Bước mảng 9: Làm bộ test nảy Catalog Service cựa nạp dải Unit tests
- [ ] Lệnh bục 10: Thiết xả Catalog Service rã cho integration đo tests cài phễu

## Các Nhịp Đi Lệnh Cho Lending Service (Lending Service Steps)

- [ ] Lệnh Bước 11: Tạo rã dải trúc Project rã cấu cho Lending Service (`pyproject.toml`, ngỏ bục `src` layout, gán `tests` thư mục)
- [ ] Nhịp cựa 12: Phễu Trút Lập mỏ Domain entities (Dựng `Member`, `Checkout`, `Hold`, `Fee`, `Payment` cựa đai)
- [ ] Bước cựa 13: Xây nạp Pydantic rã requests/responses models
- [ ] Nhấp phễu 14: Lên dải Tầng Phễu lõi gán các Core modules đai (exceptions ngỏ, helpers, logging nảy, chạy config)
- [ ] Trích Lệnh 15: Tạo rẽ túi Abstract ngỏ repositories trút với ngỏ mảng in-memory xả implementations đai
- [ ] Bước nảy 16: Nhả mác AuthService ngỏ (Ngỏ JWT + bọc đo bcrypt rã)
- [ ] Bước bọc 17: Xây MemberService ngỏ rã
- [ ] Nhịp nảy 18: Lên mảng CheckoutService đai
- [ ] Nhấp rã 19: Dựng cựa HoldService
- [ ] Bước ngỏ 20: Dựng mảng FeeService bọc đai
- [ ] Bước đai 21: Làm rã ReportService đai ngỏ
- [ ] Khởi nảy 22: Ghép cựa CatalogClient (Cổng HTTP client) bọc nhả 
- [ ] Trích rẽ 23: Làm túi Auth ngỏ middleware (xác thực đai JWT trút)
- [ ] Lệnh nảy 24: Nhả cựa bộ API vòng routes (cháp member, cựa checkout, mảng hold đai, bọc fee, trích ngõ report)
- [ ] Phễu đai 25: Đóng cựa mảng Cổng FastAPI rẽ app entry point móc
- [ ] Bước 26: Xây rẽ Lending Service ngõ đai Unit tests mảng
- [ ] Lệnh ngỏ 27: Nháp dải Cựa thiết đai Lending Service cho ngõ Integration tests đo

**Cổng Tổng Tính (Total)**: Nhả Phễu 27 bước nảy Cover cựa (bao xả trót đai) báo cho bọc Cả hai rẽ 2 Services dải. 
