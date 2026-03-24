# Yêu cầu Phi chức năng (NFR Requirements) — Dành cho Cả Hai Dịch Vụ 

## Yêu cầu về Hiệu năng mảng (Performance Requirements)

| Quy nạp Yêu cầu (Requirement) | Chỉ Điểm Target | Đóng Cựa Áp phễu cho Bọc (Applies To) |
|---|---|---|
| Độ Mảng Đáp Cửa Giờ API (p95) | Dưới trích `< 100ms` | Cả 2 nảy dịch rẽ vụ |
| Trễ Báo rẽ Tìm Full-text | Mác Báo `< 200ms` | Nháp cựa bên Catalog Service |
| Ổ cựa Người Cùng nạp dùng Concurrent | Điểm 100 đai Nhịp simultaneous (cùng móc nảy rã lúc) | Ngỏ hai cựa 2 đai Services |
| Mức cháp Trễ phễu gán Chờ đai Cold start rã | Rẽ Cựa `< 3 seconds (3 giây rã)` | Cho cựa bục both 2 services nạp |

## Đo Yêu Phễu Tính Sẵn Sàng Sống Bền đai (Availability Requirements)

| Túi Yêu Cầu (Requirement) | Hệ Điểm SLA Target |
|---|---|
| Độ Xả Chóp Uptime nảy của API | 99.9% rã đo |

## Mảng Nhả Tính Bảo bọc Mật An Toàn (Security Requirements)

| Đầu rã Yêu cầu | Cấu Chi Rẽ Tiết mảng Detail |
|---|---|
| Báo Nhả Xác Cựa Thực (Auth) | JWT trút nạp mức tầng app Application-level (Qua bộ: PyJWT + đo dải bcrypt) |
| Kiểm đai Phân bọc Quyền (Authorization) | Đi lệnh RBAC với ngỏ Admin rã, mảng Librarian, túi Role rẽ Member cựa |
| Chóp Giữ bọc Mật ngỏ Khẩu Password báo | Ngõ Adaptive hashing nảy dùng đai bcrypt cựa ngỏ |
| Điểm Hạn cựa Token expiry rã | Phễu Gán 24 hours, xả nảy cấm ngỏ không đâm sinh refresh tokens rẽ (ở mốc MVP cựa nảy) |
| Mã Cựa Mảng Hóa Khi nảy Lưu data rest | Bắt ngỏ bục buộc rã (Required) với cựa trích All ổ data cựa stores nảy |
| Mảng Cựa Hóa Lúc rã Mạng In-Transit | Mỏ Chuẩn cựa TLS 1.2+ rã đai |
| Khóa Rã Cựa Hồ PII bục nạp | Email / name (tên cựa) của Member đai cấm Không vách rã lọt (must not) lên chóp mảng Logs báo đai ngỏ |
| Vạch Validation xả Input đầu đai | Bọc Pydantic rã models đo kẹp chặt đai mảng length constraints trích ở trút gán All endpoints cựa |
| Phễu rã CORS bọc ngỏ | Rào Restricted origins ngỏ (chỉnh bọc được đai ngỏ configurable, Chặn tuyệt ngỏ cái dấu `*` rẽ wildcard đai với các endpoints trích ngõ có xác Auth) |

## Thiết Cài Các Yêu cầu Khâu Kiểm (Testing Requirements)

| Loại Nhả Test (Type) | Mốc đai Đích Target nảy |
|---|---|
| Test Unit mỏ Độ Phủ nảy Coverage | Lớn `>= 90%` Code phễu nhả đai Line trích cho rã từng service đo |
| Đo Bục Integration Tests trích | Gõ Test đo trút cho Tất cựa All ngỏ nảy API Endpoints rã |
| Test Bọc Hệ xả Quyền Auth | Đai Nhả Test cả chóp gán trường hợp có quyền rẽ Authorized nảy VÀ Không quyền Unauthorized cựa cho 1 endpoint đai |

## Cấu Dải Mở Tải Khả Scale (Scalability)

- Mỗi Mảng Bộ Service cựa Nó tự bọc đai rã Scale vách Independent độc đai cựa. 
- Ngõ Catalog: Nhả Thiết cựa ngõ đánh Trụy gánh túi Bọc Nặng về Đọc Read-heavy (Ngõ xả Mảng tìm kiếm Search/Browse bọc)
- Ổ mảng Lending: Trút Bọc mỏ Gánh Trọng đo Móc hệ Ghi Write-heavy (Khâu nảy Checkout/Returns mác cựa rẽ) 
- Mô mỏ đai mảng Hình Gán Chóp triển Nhỏ Deploy đai (Single-library) cho dải vách MVP
- Hệ Mục Cựa Tiêu Target: Chứa rã ngỏ 10,000 cuốn books, với rẽ nạp 2,000 nhả Members đứt rã. 

## Tính Chọn Lựa Đo Bộ Hệ Database Decision (Quyết định DB)

**Quyết nhả Chóp: Ốp In-memory ngỏ Repos đai kho lưu cùng trút bọc cài Abstract phễu Base mảng Class (Cho rẽ đai Bọc Mốc test đai mạc MVP).**

**Móc Cựa Lý Giải đai (Rationale):**
- Ngõ đai Tech-env trích đã đứt nhóp cho Lùi bọc (Deferred rã) việc đai chọn DB gán xả cài Database nảy mạc (giữa DynamoDB rã chóp vs PostgreSQL mỏ bục) qua tận cái ngỏ NFR stages cựa đo rẽ 
- Hạ tầng CDK trút rã bị gác đai Mạc Deferred — Thành nảy rã Sẽ chóp nảy chưa Có cái Cloud DB cựa xả nào bị mạc rã Dựng Provisioned lên đai
- Dành ngỏ cho test với mổ Dev chạy cục bộ đai rã, Bộ in-memory Repos nạp cung cự ngỏ dải sự Độc lập (Zero-dependency chạy mỏ test). 
- Khuôn bọc cựa Móc Abstract nảy mạc repository ngỏ đai cho bọc nháp phép cựa Tráo bọc Mảng Đổi Swapping qua ổ DynamoDB hoặc là mỏ PostgreSQL sau bục qua trích báo ngõ cài Concrete nảy khâu implementation. 
- Bộ Cách cự In-memory cựa bọc là mỏ đai mảng Nhất ngỏ Quán (Consistent) cựa với Thiết Scope mảng của "Application Code ONLY móc đai cựa" rẽ ở bản MVP. 
- Đai mỏ Toàn Bảng Business Logic cài với mạc ngỏ Behavior phễu API cựa rẽ Fully Testable (Tự test ok) không đâm chóp kẹt Dependencies bó buộc ngỏ nạp móc External nào. 

**Bộ Mảng Design Cựa Repository Pattern Pattern:**
- Lớp `BaseRepository` (hệ abstract) định đo mảng cài cái giao trích diện (Cấu interface đai mỏ móc: create nảy, get báo, phễu list đai, update ngỏ, nhả delete cựa, bục mác search)
- Bọc dải `InMemoryRepository` trút mảng Implementation nhả bằng dải Python dicts đứt đai — Dành ngỏ cựa xài test bọc cài + Dev
- Tương cựa Lai: Ngõ `DynamoDBRepository` móc hoặc `PostgresRepository` bọc rẽ ngỏ có thể rã Cắm Tráo (đai drop-in) nạp thẳng mảng bọc replacement được nảy vách lấy cài đai phễu sau cựa 

## Khâu Mảng Chắn Chọn Lệnh Hóa Quyền Xác Định Định (Auth Decision)

**Chốt Decision: Mảng Hệ Ngõ JWT bọc Ở Application-level đai với rẽ PyJWT nạp + passlib[bcrypt] đai cựa**

**Rationale Giải Lẽ Lý Đo:**
- Gọn nhẹ chóp đo hơn xài nảy Cognito cho đai vòng MVP rẽ đo 
- Khử cựa Đứt Không lệ thuộc bục ngoại nhả bọc gán External (Chả cựa bọc Phải đòi báo AWS Account đai rã mới dải đâm gọi Run rẽ Test ngỏ bọc)
- Trích Cả Dải 2 Services đai tự Auth bọc nảy Validates đai JWT gán dải báo Độc lập móc qua móc đai Xài Cùng bọc ngõ 1 Ổ Shared chóp nảy Trích Secret
- Cái mã ngỏ JWT dải Secret đai được Lưu cài trút Ổ Cấu phễu Config (Mác cựa biến bọc Environment cho gán cựa Local dev gán, Chứa trích Bọc Secrets bọc mảng Manager cựa cho mác Prod xả đai)

## Thông Bộ Gán Nảy Đi Khóa Bộ Lưu Log (Structured Logging JSON)

- Bục Module Của đai Cựa Python `logging` với ngỏ Khóa JSON báo formatter đai
- Các bọc rẽ Trường Field gán: timestamp nhả, túi rã correlation_id bục đai (Request ID đo rẽ đai), cựa level ngỏ mác, bọc message, mảng chóp service_name xả đai 
- Bộ Lọc Nảy Lọc ngõ cựa PII rã: Gạt Bỏ Xóa nảy mác (Strip mảng cựa) Tên Member name rã cùng mảng Member bọc email ngỏ đai khỏi mảng Móc rã Log output 
- Tuồn log rã Đẩy Báo Về Stdout stdout (Cái đai nhả này Hợp rẽ Hệ ngỏ CloudWatch bọc đai gán phễu Lúc deploy cựa bục lên xả test AWS đai)
