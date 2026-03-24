# Các Đầu Mối Phụ Thuộc Của Đơn Vị Công Việc (Unit of Work Dependencies)

## Bảng Ma Trận Phụ Thuộc (Dependency Matrix)

| Đơn Vị rã (Unit) | Gọi Phụ Thuộc ngỏ (Depends On) | Cấu Giao Tiếp bục (Communication) | Mức Ưu Tiên đứt (Priority) |
|------|-----------|---------------|----------|
| Catalog Service | đứt None | — | Build bọc first (nhả đầu tiên rã) |
| Lending Service | mác Catalog Service | Mạng HTTP (cục httpx ngỏ) | Build cựa second (trút sau) |

## Giải Chi Tiết Các Mạng Phụ Thuộc bọc (Dependency Details)

### Mảng Catalog Service ngỏ → Trút Phụ thuộc Ngoại vi báo (External Dependencies)
- nảy Không phụ thuộc vào mảng service nào khác (No service dependencies)
- Bục Database: Tự giữ đai data store riêng ngỏ nạp (Để chốt ngỏ đo sau ở bộ NFR)
- Dải Libraries (Phễu Thư viện nảy): Khóa FastAPI phễu, Pydantic ngỏ cựa, PyJWT mác, nhả uvicorn

### Bọc Lending Service rẽ → Gọi Catalog Service
- **Lý Do Ngỏ Mục Đích cháp (Purpose)**: Dò test Kiểm tra sách tồn tại Existence và ngỏ Nhịp cập nhật bọc trống rã có sẵn Availability đứt 
- **Đai Các Cuộc Gọi API bọc**:
  - `GET /api/v1/books/{book_id}/availability` — Check nảy sách rã tồn tại mạc và SOI phễu cháp copies ngỏ rã 
  - `POST /api/v1/books/{book_id}/availability` — Nạp Cựa Lệnh Cộng/Trừ ngỏ rã increment/decrement điểm available_copies nảy
  - **Khâu Bắt rã Lỗi Mảng Failure Handling ngỏ**: Gắn đo Nếu rã mạng Catalog Service sập bọc unavailable cựa ngõ, Các bộ trút thao dải tác bục check sách trong Lending cự đứt buộc phải quăng ngã Fails rã đai và văng chóp bục INTERNAL_ERROR bọc 
- **Cựa Nhóm Libraries xả**: Tút FastAPI nảy đai, Pydantic, cựa bục PyJWT, Ngõ mạc passlib/bcrypt trút đai, ngỏ httpx dải nạp, uvicorn

## Các Đầu Mối Nảy Chỉ Điểm Tích Hợp Đai (Integration Points)

| Mốc bục Tích Hợp nảy (Integration) | Dải Mảng Cửa Nguồn (Source) | Đai Trích Nhả Đích (Target) | Lệnh Móc Gõ Method | Ngỏ Tần Suất đai đo (Frequency) |
|-------------|--------|--------|--------|-----------|
| Kiểm test nhả Checkout | Nạp Lending | Catalog rã bọc | bọc HTTP GET | Nhả Every checkout (Mỗi nảy rã lượt Checkout) |
| Lùi Trừ gán bọc Availability ngỏ | Mảng Lending | Trút Catalog | cựa HTTP POST | Every xả checkout (Mỗi vòng mượn) |
| Kéo Tăng rã Availability | Lending dải | Catalog nảy cựa | ngỏ HTTP POST | Every cựa return (Khi nạp bọc Trả về bục) |
| Verify cựa ngỏ chóp Lệnh Hold nảy | Cựa Lending đai | Báo Catalog bục | HTTP bọc GET mảng | Every nháp hold đai placement đo (Từng bục ngõ lượt đai ngỏ đặt hold đai) |
