# Đơn Vị Công Việc (Unit of Work) — Bản Đồ Rã Khâu Câu Chuyện (Story Map)

## Cụm Unit 1: Catalog Service

| Đai ID Nháp Story | Tựa cựa Bộ Tên Story mảng bọc (Story Name) | cựa Hệ Ưu báo Tiên bục (Priority) |
|----------|-----------|----------|
| US-CAT-001 | Nhả Tạo mỏ Add nạp 1 ngỏ bọc Book | Phải đứt Có Mảng (Must Have cựa) |
| bục US-CAT-002 ngỏ | Đai Cập nảy Nhật phễu mảng 1 bọc Book | Phải Có (bọc Must mạc Have) |
| US-CAT-003 | cựa Kéo Get 1 cựa mạc Book | Phải Có (Must phễu Have) |
| US-CAT-004 | Bọc Quét phễu list nảy đứt Toàn rã bộ Books ngỏ | Phải rã đai Có (nhả Must Have) |
| US-CAT-005 | Mác Xóa nảy Delete cựa rã 1 mảng Book bọc | Phải Nháp Có ngỏ (Móc Must Have đứt) |
| US-CAT-006 | Tìm cựa Quét ngỏ Search Books đâm | dải Phải ngỏ Có nảy (Must Have ngỏ bọc) |
| US-SYS-001 | Báo Test rã Catalog ngỏ Bộ chóp Health Check | Phải đứt mạng Có bục (Must cựa Have) |

**Ngỏ Tổng Cộng cựa (Total)**: Rã 7 cựa stories trích đai 

---

## Cụm Phễu mỏ Unit 2 bọc: Lending Service

| Mã chóp ID nảy Story bục | Mác Tựa rã Tên nhả ngỏ Story bọc | Mức cựa Ưu Nháp Tiên đai (Priority) |
|----------|-----------|----------|
| US-AUTH-001 | Đăng nạp Nhả Ký ngõ Mới nảy Member xả | Phải nảy mảng Có (báo Must Have bọc) |
| US-AUTH-002 | cựa Đăng mác Nhập nảy (Login rã) | cựa Phải đâm Có ngỏ |
| US-AUTH-003 | Rã Xem chóp Test mác Profile Của Nhả Mình mảng | Phải trút Có đai  |
| US-AUTH-004 | Nhả Update gán cựa Profile Của bọc ngỏ Mình | Phải mạc phễu Có đai bục |
| US-AUTH-005 | Soi cựa Nhả Ngỏ Cựa Profile Người Khác đai | cựa Phải Có bọc  |
| US-AUTH-006 | Văng gán Deactivate mảng Tài rã xả khoản cháp Member ngỏ | mảng Phải Có đai nảy |
| US-LND-001 | Mượn bọc ngõ Checkout chóp Sách nảy bục | Phải Có mảng bọc |
| US-LND-002 | Trả mảng ngỏ Return cựa nháp Sách bục | Phải Có rã  |
| US-LND-003 | Nơi bọc đai Nới phễu Hạn Renew mảng đai Checkouts phễu | Cựa Phải Có báo bục |
| US-LND-004 | cựa Xem bục Các gõ lượt Checkouts nhả cựa đang rẽ mác giữ dải đâm | bục Phải cựa Có đâm |
| US-HLD-001 | Nháp Đai Đặt nảy Gạch móc Hold nhả | gán Phải bục xả Có đai |
| US-HLD-002 | đai Hủy nảy bọc Cancel Hold ngỏ đứt | mảng Phải Có nạp đai ngỏ |
| US-HLD-003 | View mác Bộ trích Hàng cựa ngỏ đợi đai Hold Queue đai bục | xả Phải nảy móc Có đai |
| US-HLD-004 | Gán View xem đứt mạc đám mạc Hold của nảy Mình | bục Phải Có bọc  |
| US-FEE-001 | Xem đâm nháp Phí cự đai rã nảy Phạt cựa nạp Mác của Mình bục báo | trích Phải rã Có ngỏ cựa |
| US-FEE-002 | Nhả Xem bục rẽ Phạt phễu nảy cựa Member Khác đai mảng | Nhả Phải Có bục  |
| US-FEE-003 | Thanh bọc phễu Tính đâm rã Đóng nháp Fee Payment bọc đai | rã Phải Có phễu  |
| US-RPT-001 | Phễu Đo Report mảng Quá chóp Hạn (trút báo Overdue) | Phải xả Có ngỏ rã  |
| US-RPT-002 | nảy Ngỏ Report mác Tổng Ngỏ Bộ Collection Summary | rã Phải móc đai Có nháp |
| US-SYS-002 | Kiểm cựa test rẽ Lending bục phễu Health Check dải bọc | ngỏ Phải đứt Có rã bọc |

**Tổng Nháp Cộng bục (Total)**: Ngỏ bọc 20 nhả nạp stories rẽ đứt 

---

## Bảng đai Tóm tắt móc Summary ngỏ bọc

| Đo Đơn vị bục (Unit) | Cửa Stories nảy trút | Lệnh Trật Tự đứt Build (Order cựa nảy) |
|------|---------|-------------|
| Bộ Catalog Service | nạp 7 | dải Mảng Build nháp Số 1 (1st) — nhả rã trút bọc móc Không dépendencie phễu nạp bọc ngõ |
| Thiết rã bọc Lending Service | 20 bục đai | Build mỏ Số 2 đai rẽ (2nd) — dải ngõ Vì phụ thuộc vào Catalog bọc nảy rã |
| **Trút gán Tất Cả (Total nạp)** | **27 bọc** | |
