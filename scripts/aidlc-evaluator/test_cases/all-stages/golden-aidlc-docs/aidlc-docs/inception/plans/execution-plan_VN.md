# Kế Bản Hoạch Thực Thi (Execution Plan)

## Tóm gọn mảng Phân phễu Tích đai Chi mảng Tiết bục (Detailed Analysis Summary)

### Bảng Đánh Giá Mức độ Tác Động nảy cựa (Change Impact Assessment)
- **Rã Thay đổi hướng bục người đai dùng nảy (User-facing changes)**: Có rẽ — toàn bọc bộ 27 đai tuyến API endpoints đều cựa phục bục vụ đai cho bọc 3 nhóm user roles
- **Nhả Đổi Thay ngỏ Cấu trúc mảng (Structural changes)**: Có nảy — hệ rã thống có 2 microservices độc đai lập rẽ giao nảy tiếp với nạp nhau chóp
- **Thay báo đổi cựa Khung dải Mô nảy Hình Data (Data model changes)**: Nháp Có — bổ ngỏ sung các models đứt mới trút cho bộ books, bục members, cựa checkouts đo, phễu holds, phí rã fees bọc
- **Rã Các Nháp Thay đổi cựa Về phễu API (API changes)**: Có rẽ — dải Thiết đo kế trút bộ Cổng REST API rã hoàn ngỏ thiện xả cho cả 2 nhánh services cựa
- **Xả Mảng Tác ngỏ động Nháp vào NFR (NFR impact)**: Có bọc — bộ mốc hiệu rã năng performance đứt targets, dải cơ chế Security rẽ (RBAC, vòng JWT bục), kèm đai bộ đòi bọc hỏi Khả năng nảy Mở rộng nảy Scalability cựa

### Bảng Đánh báo Giá Mảng Rủi cựa Ro (Risk Assessment cháp)
- **Mảng Mức nảy Rủi ngỏ Ro (Risk Level)**: Mức Medium (Trung bình) — Luật bục kinh nảy doanh Business rules rẽ khá phức nháp tạp ngỏ nhưng nảy Yêu mảng Cầu Requirements phễu lại Được bục Định ngõ Mảng nghĩa rất cựa rõ ràng ngỏ.
- **Rã Độ Phức Tạp bục Khâu ngỏ Rollback (Rollback Complexity)**: Cựa Mảng Dễ (Easy) — (Đây là dự cựa án Greenfield đứt, không cựa có mốc Hệ thống gốc đứt nào để đo mà nảy lo Bể Mảng Break). 
- **Móc Độ bục Phức nhả Tạp trút Khâu báo Testing (Testing Complexity)**: Phức rã Tập (Complex) — Bộ 2 rã cựa services ngỏ nối nhau, Bọc biên rã giới quyền RBAC đai, nảy các Cựa bọc góc rã nhả nảy cạnh edge ngỏ cases nạp đo của sách nảy mượn Lending cháp.

## Bọc Sơ ngỏ Đồ rã Luồng rã Chạy (Workflow Visualization trút)

```text
Pha cháp 1: INCEPTION
  - Nhịp 1: Bọc Quét Workspace đai Detection (ĐÃ XONG COMPLETED)
  - Bước 2: Kĩ cựa Thuật ngõ Ngược Reverse bục Engineering (BỎ QUA SKIPPED - do là greenfield code mới)
  - Khâu 3: Bộ Phân Hệ Tích ngỏ Requirements rã Analysis (XONG COMPLETED)
  - Nảy 4: Các User chóp Stories (XONG COMPLETED)
  - Nhịp dải 5: Đo Lên bục Workflow mạc Planning (ĐANG CHẠY IN PROGRESS)
  - Đo 6: Mác Thiết rẽ Kế nhả Application Design (CHỜ LỆNH EXECUTE)
  - Nảy 7: Chia Khối Units Generation (CHỜ THỰC THI EXECUTE)

Bục Pha 2: CONSTRUCTION (XÂY DỰNG)
  - Khâu dải 8: Mảng Thiết đo Kế Functional Design (EXECUTE rã)
  - Dải 9: Lên Yêu đứt Cầu NFR ngỏ Requirements (EXECUTE đo)
  - Nháp 10: Xả Thiết trút Kế NFR cựa Design (BỎ QUA SKIP)
  - Nháp 11: Mốc Thiết cựa Kế bọc Hạ tầng Infrastructure Design (SKIP rẽ)
  - Nhịp phễu 12: Đục Code Gen rã Code Generation (EXECUTE cựa)
  - Mảng 13: nảy Build phễu thử bọc Test bục (EXECUTE bọc)

Trút Pha 3: OPERATIONS (VẬN HÀNH)
  - Đai Mảng Operations (Trống để nhả đó PLACEHOLDER bục)
```

## Các Bước đai Cần Nhả Thực bọc Thi cự (Phases to Execute)

### MẢNG CỰA INCEPTION PHASE
- [x] Workspace Detection (COMPLETED nảy)
- [x] Reverse Engineering (SKIPPED — Dự xả án móc greenfield project mới cứng đai)
- [x] Requirements Analysis (COMPLETED bọc)
- [x] Hệ User cựa Stories bục (COMPLETED mảng)
- [x] Workflow Planning (ĐANG rã CHẠY nảy IN PROGRESS)
- [ ] Application Design — Nảy EXECUTE rã
  - **Lập luận (Rationale)**: Hệ bục 2 services nảy Rất cần cựa phễu khâu đai Xác định dải phân cháp thành dải phần trích component đai, biên cựa giới Service bọc, và bọc bộ sơ ngỏ đồ cựa Map móc độ lệ mảng thuộc phễu phụ rẽ dependency ngỏ. Điều này Cực mảng Kỳ bọc Quan Trọng (Critical trút cho) việc rã chốt nhả cơ mỏ chế 2 rã mạng Catalog + mảng Lending giao rẽ tiếp ngỏ mạc interact với bục nhau. 
- [ ] Mảng Units đứt Generation mạc — THỰC THI EXECUTE
  - **Giải thích Rationale**: Hệ cự Này cháp sẽ bóc rẽ tách decompose đai thành 2 cụm nháp services bọc lớn (Catalog đứt + nảy Lending). Units sẽ nảy Quyết Định chóp trật phễu tự Build order bục và nạp mác thứ tự ngỏ Dependency bọc chuỗi sequence ngỏ.

### GIAI bọc ĐOẠN nảy CONSTRUCTION PHASE đai
- [ ] Bộ dải Functional ngỏ Design — LỆNH EXECUTE
  - **Lập cựa Nạp Luận Rationale**: bọc Business cháp rules cựa nảy bọc ngõ Khá phức nạp tạp (ví dụ ngỏ bọc checkout bục limits, rẽ hold đai queues, phí phạt fee ngõ calculation, móc luật nảy renewals bọc) rã nên nảy Cần phải ngỏ có mác bản spec bọc đặc rẽ thù cho nhả entity móc và domain bục model thật nhả Chi đứt Tiết bọc trước đai khi thả Code xả Gen.
- [ ] Vòng ngỏ NFR nảy Requirements — THỰC rã THI EXECUTE cựa
  - **Cựa Rationale ngỏ**: Túi Chọn cựa Chọn bục Loại Database nào (DynamoDB vs đo bục PostgreSQL đứt), giải chóp pháp rã Auth Authentication (PyJWT ngỏ + băm cựa bcrypt móc), Trút Và Nảy các hệ test hiệu cựa năng rã nảy đo. Khâu Tech-env rã Có nhả In rẽ list trích ngõ ra tùy ngõ chọn nháp bọc nhưng ngỏ nó bọc lại nảy Gác (defers) lại khâu đo quyết nhả nảy chóp định. 
- [ ] Khâu NFR mạc Design — BỎ rẽ SKIP rã
  - **Lý đo Do nhả Lập Luận phễu**: Những chóp NFR phễu patterns nhả sẽ mạc được dải ghim nháp Thu đo Nạp cháp Trọn đứt Vẹn (sufficiently) ngay rã bên trong nảy NFR chóp Requirements. Với ngỏ bọc phạm xả vi scope bản đo MVP kiểu "application-code-only đai" (Không AWS CDK bục rã), Thì dải Việc bọc Sinh nảy ra 1 đứt khâu riền "NFR bục Design" bọc nhả chóp stage đứt Sẽ ngõ Chỉ phễu rẽ Trút Sinh nạp Thêm bục nạp Value bục Tí rã tẹo bứt Minimal đai. 
- [ ] Thiết Kế Infrastructure rã Design — SKIP bọc QUA
  - **Rationale phễu Nhả Nảy**: Ổ Infrastructure đứt Bằng CDK bọc đã cựa Bị Gác đai nảy Deferred nháp theo rã chỉ bọc thị Khách dải Hàng bọc cựa stakeholder nạp. Code nhả Ứng Dụng phễu tập chóp trung móc bọc đánh mỏ In-memory bục / ngỏ rã dải SQLite nhả cho rã Dev cựa local ốp đai với đai cháp bọc Abstract mỏ đứt. Nên cháp đâu cựa có nháp Cloud nhả infrastructure đai chóp nào rẽ để cựa mà rã Design bọc.
- [ ] Dải Code rẽ Generation đứt (Gen nháp Mã Code) — THỰC cự THI EXECUTE (Lệnh mảng LUÔN bục CHẠY)
  - **Rationale bọc**: Nhả Ép cựa Gen bọc Code ngỏ nhả hoàn mạc thiện cựa cho trút bộ nhả Cả rã 2 chóp services bọc.
- [ ] Test đo Test & Build rẽ Build — CHẠY phễu EXECUTE (LUÔN)
  - **Lý nảy nhả Luận cựa**: Lắp Install ngỏ dải phụ nạp thuộc rã, xả chạy rã Test, nhả verify bục chóp % độ móc phủ Code rã.

### KỲ OPERATIONS PHASE
- [ ] Khâu Vận Hành Operations — TRỐNG nháp Để mảng Chỗ bọc

## Tiêu chí Thành bọc Ngõ Công ngỏ cựa (Success Criteria phễu)
- **Mục tiêu xả Chóp Chính nảy (Primary Goal bục)**: Bọc Build đai Dựng mảng được trút 2 cựa nhánh chóp FastAPI rã services ngỏ Chạy bọc rã gán Hoạt nháp Động cựa Độc đứt lập rã nảy (Catalog + Lending) đi ngỏ kèm bản cựa code cự Business nạp Logic móc Hoàn chỉnh nảy  
- **Cựa Điểm nảy Nhả Đích Sản Phẩm (Key Deliverables mỏ)**:
  - Catalog đo Service bục mảng gồm rã bộ nảy Sách book bọc CRUD đứt, Tìm nảy search ngỏ, móc đai Dò rã bục nảy độ cháp availability 
  - Đai Lending Service ngỏ nhả kèm Khâu xả Đăng đứt Ký auth chóp đai, dải nhả Checkouts rã phễu, Dải return đo cựa, nạp Khúc mảng renew xả đứt, cựa Khâu cháp hold ngõ đai, Tính xả fee bục, và cựa reports ngỏ
  - Bộ nhả Unit gõ Test và ngỏ integration cựa rã integration dải với phễu `Coverage rã bục >= đai rã 90%` chóp
  - Mảng Bộ bọc Chuẩn nảy mạc Tài Liệu ngỏ OpenAPI đứt 
- **Chốt dải Chạm gán Đo Chất bục Lượng bọc (Quality Gates đứt)**:
  - Bục Pass hết trích ngỏ toàn nảy bộ mảng test nhả
  - `>= 90%` tỉ cựa lệ bọc Line móc rã coverage cháp ngỏ bục 
  - Khâu cự RBAC trút được bọc rẽ Ép Enforced rã bọc trên trích mọi chóp Tuyến nảy đâm endpoints đo cựa ngỏ 
  - Nhóm mác cựa Business cựa rules rã (Luật dải doanh nảy nghiệp bọc) Được móc rã Xác dải phễu nhận Verified bục bọc đúng rẽ rã  
  - Test nạp nhả Tuân rã thủ bọc Các ngỏ Ext cựa Rẽ móc nảy Security mảng bọc (Từ SECURITY-01 cựa nảy đâm qua cựa Tới đứt SECURITY-15) đai Được mảng Chấm Pass bọc rã
