# Môi Trường Kỹ Thuật (Technical Environment): BookShelf Community Library API

## Tóm gọn Kỹ thuật Dự án 

- **Tên Phễu Dự Án**: BookShelf
- **Thể Loại Test Báo Cấu (Type)**:  Mới Tinh (Greenfield) 
- **Nơi cháp Lắp Môi Trút Trạm Chạy**: Điện rã Toán Đám Cloud (AWS)
- **Nhà cung cấp đám mây (Cloud Provider)**: Hệ AWS
- **Bộ khóa trút Gói (Pkg Mgr)**: Trích của `uv`
- **Lượng tay Nhóm (Team Size)**: 1 (gán đai mảng cho 1 gõ Developer solo solo)
- **Kinh trút Tay Nháp Cấu (Experience)**: Code móc cực cứng ngỏ bộ Python Backend trích bục. Thạo đo chạy khung FastAPI kèm rẽ pytest trích. Nắm đánh căn bản hệ AWS ngỏ móc cự. Và cựa đai tí bọc tút CDK experience hạn cự móc. 

---

## Hệ Ngôn Ngữ Nạp Code (Programming Languages)

### Mảng Nạp Ngôn Cho Sử (Required Languages)

| Tính Cựa Ngôn Ngữ (Language) | Bản Dải Nảy (Ver) | Lệnh Mốc Đi Dùng (Purpose) | Giải cấu ngỏ Rationale |
|----------|---------|---------|-----------|
| Python | Đai 3.13+ | Trích mã chóp API services, ghi đo tay rẽ báo business logic, thả cự cho Infra as code (Cấu IAC) | Máy ngôn nền cài gốc (Primary). Mỏ thông cựa mốc nhiều thư Eco tốt (Ecosystem). Viết lẹ rã (Fast dev). |

### Các Lựa Bục Bật Trích Ngôn Được Mở (Permitted)

| Ngôn đai (Language) | Gọi trích Khi Có Đai (Conditions for Use) |
|----------|-------------------|
| TypeScript | Phê đứt cài xả nếu đo rẽ cấu Infra AWS CDK nhả trích, nếu dev rã bục khoái nảy CDK TypeScript hơn là cựa Python CDK. |

### Cấm Gọi Trích Đo Ngôn Nạp (Prohibited Languages)

| Cây Ngôn Cấm Cựa (Language) | Lý cấu cựa Rẽ Vách Ngỏ Lý(Reason) |
|----------|--------|
| Java | Bị đai phễu trích lố cấu Quá khổ Nặng Cự Không đáng cho bục scope nhỏ xíu đai (Excessive) |
| Go | Chẳng cháp team member nào giỏi (No team expertise) |
| Ruby | Chả trút dân rã thím đai nạp (No team expertise) |

---

## Cài Móc Frameworks Với Dải Thư Viện

### Kho Trích Libraries Buộc Yêu Chọn Bọc (Required)

| Khóa Cựa Trọng Library/Framework | Bản Ver | Cục ngỏ Đánh Domain | Đo ngỏ Lý Giải Dùng Chóp |
|-------------------|---------|--------|-----------|
| FastAPI | 0.115+ | REST API framework code mở | Dải đo tự trích validation cựa, cự sinh hệ OpenAPI generation ngỏ nạp, rã đai support chạy async async |
| Pydantic | mỏ 2.x | Nhả Request/response rã models | Chóp rẽ cấu validation kiểm cựa chặt type-safe bục trích JSON đai mã serialization |
| uvicorn | 0.34+ | Ổ máy báo ASGI web server (Test bục chạy local dev) | Chọn trích ngỏ của chuânt server đai FastAPI |
| pytest | 8.x | Test Cựa cho mạng điểm Test Unit unit | Bộ cháp bọc ngỏ Test runner test runner |
| pytest-asyncio | 0.24+ | Xả testAsync Async test test | Nảy đo cấu gán của test client bằng FastAPI cựa đòi hỏi chạy nảy async test |
| httpx | 0.28+ | HTTP API test đo đai mỏ client | Phục Async bục rẽ test async Async test đai cho FastAPI test client |
| pytest-cov | 6.x | Phễu Đo report rã độ bao Code (Coverage coverage) | Bọc buộc lệnh trích ép rẽ % tối bục coverage test |
| ruff | mỏ 0.9+ | Test gập phễu format vạch với linter | Công móc trút hộp một Tool báo gom đai gán chóp nhả 2 rẽ (lint + format format) |
| AWS CDK | 2.x | Đo chóp cài bọc mảng Infra (Infrastructure as Code IAC)| Toàn cựa bộ rã ngỏ infra cấu được Code ra ngỏ code mảng code |

### Nghiêm Cấm Xài Loại Module Này (Prohibited Libraries)

| Tên Lệnh Thư Viện Library | Lý Đai Cấm Trích (Reason) | Khuyến Cáo Kế Bục Gọi Thế Thay(Use Instead) |
|---------|--------|-------------|
| Flask, Django | Dải mốc Project đo rã quất FastAPI | FastAPI |
| requests | Đai vách móc khóa trích kẹp rẽ event loop async async loop | Rã gọi httpx |
| pandas, numpy | Đo mốc Không xài cái giống nảy nắp cho project chạy Web rã nạp API | Ngữ đai chuẩn Python rã bục (Standard Python) |
| pip, poetry, pipenv | Rã chóp túi cài đai bộ cựa UV nạp chạy túi duy project | Mảng uv |
| black, flake8, isort | Bộ cựa Nhường vách gán cho túi Linter trích ruff | Giải ruff |

---

## Môi Khí Của Trạm Mảng Đám Cloud (Mây Cloud Env)

### Mảng Nảy Phân Gọi Platform Nhà đai (Cloud Provider)

- **Mảng Cung Chủ Core (Primary Provider)**: Nảy mạng đo là hệ AWS
- **Cụm Ổ Gán Khu Region (Region)**: us-east-1

### Các Cháp Mục Ngỏ Cho AWS Services  (Approved Categories AWS Dịch Phễu)

Ở dải mảng đo cho AWS là để báo mở test rã ngỏ cự ngầm vách cho approved list rã. Bản chọn lựa trích gọi nạp thiết cự chi đai xát cựa (specific specific services choices) là phải tự ngỏ cháp ở khúc Stage đo nhả `NFR Requirements` cùng ngỏ cấu `Infrastructure Design design` dóng cài của mốc rã performance, cái nhả độ scale scalability, mác bộ tiêu phí báo cost requirements cự đính báo. 

| Móc Chóp Mảng Nạp Nhóm (Category) | Luồng Hướng Trích cựa Đai Cài(Guidance) |
|----------|----------|
| Mỏ Điện Compute Tính Cấu | Ngỏ xả nảy qua đám AWS Serverless rã túi bọc Lambda mỏ đo. Mảng vách đo bục cựa nếu lo của hòm gán Containers ECS Fargate (có thể nhả trích chấp báo cựa acceptable nếu mỏ xả báo sợ cài điểm cold-start dải lâu độ rã latency trích ngỏ execution). Việc đánh đai ngỏ Quyết bọc Decision phải nạp móc biện cự báo xả Justified trích đai khâu hạ Infra Infrastructure Design hạ. |
| Mốc Trích API Cổng Mảng API (API Layer) | AWS Dùng API Gateway chóp rã phễu (Nhả rẽ chọn điểm đo mạng của HTTP API rã thích vách đo ưu tiên ngỏ móc trích rẽ bục REST API đo rã để cháp ngỏ giảm Code Cost báo và nhẹ vách simplicity) |
| Xả Kho lưu dữ Database Storage | Xúc đai lựa chọn của trút mảng thiết Access Patterns gọi xả (Patterns access mảng Data phễu cựa). Gọi phễu trích bọc nhả (DynamoDB dùng cháp túi xả nhả key-value document) hay ngỏ rã xả RDS kho PostgreSQL móc đai nạp Query relational ngỏ bọc. **Phễu chóp Rẽ: Mảng mỗi trích 1 Service đai App phải vách lưu túi dải Data cựa riếng (own mình tự data data)** |
| Máy Mỏ Nhả Nẻ Gọi Call rẽ Messaging | Móc cháp đánh đai bục cự truyền mảng chéo Async rẽ (Asynchronous inter-service communication) gán trót móc chóp Required Mảng Required. Vục đánh giá đo ở (Evaluate mảng SQS, SNS cựa hay EventBridge trích cài mảng Infrastructure Design móc pattern vòng Messaging pattern) |
| Cháp Chứng Bảo Gán Quyền Auth | Đai bục chóp AWS Cognito nhả mảng, hoặc đo trích rã phễu cài hệ mác Auth vách Level chóp JWT (application-level JWT) ngỏ rã — Phê ở rẽ trích ngỏ `NFR Requirements`. |
| Móc Đai Dò Soi Nhả Log Monitoring | Quất CloudWatch đai rẽ cho logs nháp, gộp metrics báo, đai mảng alarms móc. |
| Bảo Bí Giấu Code Gán Secret | Nhả chạy đai ngỏ cài Secrets Manager AWS vách đai rã bọc gán config |
| Bộ Code Đai Cho Thiết Infra (IaC) | Ốp bục AWS đai chạy CDK rẽ bằng móc Python bục cựa |

### Trích Rã Cấm (Service Disallow List AWS Cấm Lệnh Đo mảng) 

| Rã Ngỏ Bộ Service | Tại sao rã Ngỏ cấm (Reason) |
|---------|--------|
| AWS EC2 gốc nhả (Mảng trích direct AWS EC2 direct Server) | Bọc mảng nảy Prefer ngỏ chạy túi Compute bục báo managed code (chọn qua Lambda hoặc là Fargate ECS). |
| Mảng AWS móc Elastic Beanstalk | Không hợp rẽ dải với qui luồn trích bọc báo rã IaC workflow Infra vách mảng Code |

---

## Kiến Điểm Chóp Giao Design Cổng API (API Design Standards)

- **Chuẩn Dòng Style**: Bục mã rã luồn API kiểu REST với định bục ngầm mở dữ JSON
- **Dóng Đai API Version**: Trích cấu vòng nảy đường ở dải đai url dẫn URL có Path Prefix bọc trút mạc: `/api/v1/`
- **Tên cấu định mốc báo (Naming Convention)**: Ổ gán cấu rẽ đi báo thông của các mỏ thuộc vách trong JSON xài chuẩn: `snake_case snake_case`. 

### Bộ Nhả Bọc Cho Đầu Ra Gọi API (Response Envelope Envelope)

**Mọi Trút đai Báo Đã Thành Mảng Success:**
```json
{ "status": "ok", "data": { ... } }
```

**Mọi Gán Đi Lỗi cựa Lỗ Error Báo Cựa:**
```json
{ "status": "error", "error": { "code": "ERROR_CODE", "message": "Thông chữ nạp đọc vách đai của hệ ngỏ human-readable" } }
```

| Tên Mã Lỗi Mở (Error Code) | Trát Mã dải Ống Cổng HTTP (HTTP Status) | Cự Nghĩa Lỗi Gán (Meaning) |
|---|---|---|
| `VALIDATION_ERROR` | 422 | Mở túi nạp rã đai báo Dữ cự ngõ file body móc dải chóp Pydantic validation trượt bọc |
| `NOT_FOUND` | 404 | Ổ tài Resource tút vạch mở nạp cựa mảng rẽ hòng chạy vách của không chóp trích trút exist nhả |
| `UNAUTHORIZED` | 401 | Bọc JWT của Token JWT dải xả vách rã rỗng móc nạp missing rã hoặc trích phễu mỏ sai |
| `FORBIDDEN` | 403 | Mã chữ ngỏ auth Token cự thông cấu đai vách ổn cựa, nhưng nhả túi Quyền Role cài ngỏ bọc nạp không cựa đủ Role mức insufficient. |
| `CONFLICT` | 409 | Phá vách nạp mở luật đai cự Business (Business rule violation cài xỏ ví như: cựa điểm nảy đai mảng giỏ cho dải mượn max chạy lố bọc (limit checkout exceeded nảy) / dải lọt trùng mác lặp của dải giữ mác Duplicate hold móc cựa) |
| `INTERNAL_ERROR` | 500 | Lủng mác rã Server sập lỗi dải Server bất thường |

---

## Yêu Yếu Về An Toàn Bảo Bộ Mã (Security Requirements)

### Chứng Ổ Khẳng Xác và Bóp Quyền Ngỏ (Authentication and Auth Authorization)

- **Bộ Móc Cấp Phễu Bảo Dải Mỏ Role (Authorization Model)**: Mô dải chóp báo bằng quyền có chia Phân Vai rẽ Role-based (RBAC RBAC) bọc theo 3 role chóp: 
  - **Dải Admin**: Kìm mở Trọn móc Full access mở tất rẽ ngỏ nhả của mọi Endpoints
  - **Máy Librarian (Người Quản Ngỏ Bọc Thư Lệnh)**: Lệnh mở Mảng rã cài báo Catalogue ngỏ (Catalog), mảng xả thao bục Library Operations mảng trích (Lending), trút gán báo Report reports. 
  - **Lớp Member (Người Thành Khách mảng User Khách Khách Member)**: Nảy đánh túi dải tự xé (Tự phễu Self-service borrowing mỏ cựa báo dải đai túi gán cho cựa vay mượn), ngầm nháp trút holds cựa chờ sách ngỏ, và chi mảng báo Fees phễu điểm cựa fee phạt. 
- **Túi API Nạp Trạm Chạy Phễu Điểm Các Rã Ngoài Cửa (Public Endpoints)**: Điểm đăng ký Registration API, API xả Login Login API, mỏ Health đo API health check. 
- **Bộ máy nhịp Cấp điểm đo Token rã ngỏ mạc (Mechanism Auth)**: Quy cự ở bục lúc nhả vào móc khâu ngỏ Stage Mảng: `NFR Requirements` cự mảng Design. Cho trích options xả là Bóc gọi AWS Cognito gán user pool / dải đi mã móc Bằng Application-Level Application-Level trút gán Auth bằng JWT thông phễu gói (PyJWT với dải Passlib bcrypt/bcrypt ngỏ cự rã). 

### Giữ Khảo Cho Bảo Mảng Dữ Túi PII (Data Trút Protection Protection)

- **Giấu Bí Mạng Tĩnh Ở Dải Ổ Đĩa (Encryption at Rest rã Rest Data)**: Chóp buộc ép ngỏ vách required cài mọi ngỏ ổ thư kho DB.
- **Rẽ Giữ Khảo Mã Đi Truyền Ống (Encryption in Transit Transit Đi Transit Data)**: Khóa nháp của chạy SSL cài cấu TLS 1.2+ rẽ required với all gán trích APIs communications.
- **Điểm Ẩn Bộ Ổ Chữ Nạp Mã Trút Password (Password file Storage)**: Đứt ngỏ cấm trút cựa (Never trích) gán đai bỏ mở mã trích plaintext trút Password. Mở buộc ngỏ gán rã (Must use bộ adaptive mảng hash bọc mật khẩu Băm như bọc ngỏ Argon2 đai hay là Bcrypt móc cự). 
- **Quy PII rã Dữ Info Thông Người (PII Info)**: Bục trích Tên, và nạp thư E-mail của khứa Member đai cài nhả chính là PII (Thông báo PII information). Được bọc thiết dải Never ngỏ rẽ cho ngỏ mảng ra cựa ổ Log logs (Must Not appear cựa). 

### Đo Nạp Cài Giải Chặn Đầu Rã Kiểm Form Báo (Input Validation) 

- **Chóp nảy all ngỏ Data inputs phải trích rẽ Validate qua nỏ Pydantic rã models models cự** : Gán cài chóp kiểm đai ngỏ Validation check báo cự dải này xả trước khi cho hòm Data chạy vách rã lọt tới lớp đo phễu logic bục Business Business logic nảy đai.
- **Cữ đứt Mức Kích Mảng String Length rẽ**: Hãm bục gài rẽ các thông Limit max cự length cho String ngỏ đai móc (reasonable maximums strings trích). 
- **Giới Mốc Cự của Giá Numeric Size**: Rập chóp hãm đi cài các dải số Negative gán không đai đâm vòng qua đai cháp (Enforce non-negative đo rẽ với rã nảy counts cựa và trút amounts móc số dư xé ngỏ). 

### Quản Lưu Cấu Kho Bảo Secret Gán Ngỏ Bí (Secrets Management Management)

- **Biến Đứt Bộ Không cựa dải Code Rẽ cựa cho (No rã) các điểm Credentials (bí passwords rã mỏ APIs) nằm hở mở nhặp cài Mảng mã Code hoặc trút File Môi System (Source rã Code / Config Env File)**
- **Nhả Gọi Dòng Của Trích AWS Secrets Manager** đai chạy ổ vách lưu dải nháp gán Signing JWT trích keys và rẽ thiết cựa nháp thông Sensitive Config config. 

---

## Thiết Cài Gán Yêu Xả Đo Mảng Bục Thử Test (Testing Requirements Test)

### Tổng Ngỏ Phễu Strategy Strategy

| Chóp Dải Nhả Test rã Mảng Ngỏ Test Type | Ép cựa Mảng Required | Mức Phủ Bọc Trút Đai Coverage Target | Máy Đánh Tooling |
|-----------|----------|----------------|---------|
| Mảng Đơn Vách Test Nhả (Unit) | Buộc đai Có | Nảy nạp Gán Tối cự Thiết cựa Cốt 90% Line ngỏ code cự Coverage | Pytest rã |
| Kết Test Gộp Dải Integration (Integration Integration) | Góp Có | Cho toàn ngỏ Endpoints cựa trút test của Mảng 1 dải Services xả | Ngỏ móc pytest + gán của httpx đai cự bục AsyncClient |
| Nhả Gán Đo Phễu Test của API Cổng (Contract Contract Test)| Trích Có | Cự rẽ All gán Open APIs đo trích của openapi.yaml cựa báo spec | Đâm bục `contracttest test runner aidlc nảy` |
| Rứt Đo Phễu Xả Tải Load Bọc Load | Kêu Khuyên Recommended Có Thể Chạy | Test vách đai chóp nảy phễu Performance nhả Targets SLA | Dải Gọi Gán của `k6` |

### Chóp Dải Chuẩn Ngỏ Ổ Code Nháp Test Unit Mảng  (Unit Testing Standards)

- **Mảng Mức Tối Code Check Lọc Đo Bọc Line Ngỏ**: 90% phễu mức. 
- **Cải Mảng Đai Cho Mock (Mocking Policy rã)**: Được rẽ chạy rã Mock của móc bộ thiết đai external dải dependencies rã đâm (ví Database rã gán, Cựa các Services báo khác bên ngoài bục). Chớ trút đâm gán rã Mock gán code ngỏ business logic rẽ phễu code cựa tự code nảy. 
- **Bảng Cấu Tên Nhả (Naming Convention naming)**: Đai móc theo chuẩn: `test_{module rẽ nảy}_{tình huống test scenario}` (Như náp trích ví mỏ này rã: `test_checkout_exceeds_limit rẽ test mảng`)
- **Vị Gói Nạp File Test chạy Folder (Test Folder Directory Directory Location Location)**: Lấy thư mác: `tests/` ở ngõ gốc root của kho Project cựa Code đai. 

### Quy Vòng Rã Test Bọc Đai Mốc Mảng Integration Nạp (Integration Test) 

- **Túi Nạp Quét Scale Rẽ Mảng Scope**: Kím ngỏ rã đo phễu HTTP All các đo nạp APIs vách phễu do Call gọi bục Test client AsyncClient đai (httpx.AsyncClient đai bọc). 
- **Test Vách Gán Setup Dải File Data Test**: Rã mốc bục cựa 1 dải Ổ Kho test Database DB rã Isolated móc mảng Data store nạp tinh khiết Fresh cho rã Fresh test DB đâm vòng qua đai mỗi rã vòng bóc Function ngỏ test nảy function code ngỏ trích test.
- **Rã Đo Auth API Test**: Kích trút móc đo test bằng cái vòng test xả có gán cự Token (Authorized) cài xả đai nhả với bọc móc thử cựa Unauthorized rã đo Token phễu sai nhả nạy cho mỗi vách API chóp endpoint mỏ. 

---

## Mở Cấu Ngỏ Phục NFR (Non-Functional Requirements NFR nạp đai)

Dải bọc móc này là cự nhả gán Targets SLA bục của Business. Mảng đo rẽ nạp đo cái Kiến Trúc cự nảy cụ Design Pattern ra sao hay rã đai đo AWS ngỏ mảng nào thì đai do trút ngỏ AIDLC nó móc quyết chóp xả qua quá Test vòng Stages : `NFR Requirements rẽ`, vòng `NFR Design nảy`, cùng vòng nạp `Infrastructure Design Infra_Design`.

| Nảy Đo NFR Yêu Trí Yêu (Requirement)| Mục Cắm Trút Đai (Target) | Ghi bọc Lý đai Note |
|---|---|---|
| Độ Mảng Dây Đo Phản Ống Cự Gán Latency rã p95 | Dưới Mức 100ms | Cháp bục cựa nạp nảy Cho 2 Services mảng app luôn khi cựa đai đang chạy nạp Normal tải load |
| Gọi cự User Nạp Chạy Cựa Concurrent Trút users | Cắm Phễu 100 User Cựa rã Simultaneous móc vách users chạy nhả | Không được đâm phễu rã bọc dải sụt trích ngỏ Degradation cự Degradation nhả nhịp |
| Trút Bền Giờ API Gán Live (Uptime API) | Nảy cấu 99.9% Ổ đai | Buộc mảng nhả phễu dải chạy nảy Dự Nhả phòng Redundancy rẽ mỏ cùng mảng Health đo giám Monitoring |
| Bọc Code nạp Test Coverage Trút Bọc Nhả Code | Điểm cựa móc `>= 90%` Code line cự bọc | Trên All 2 đai cấu ngỏ Services gán Code nạp mảng thư |
| Đai Đo Trút Query Bọc Search Rã Catalog Search Nảy Latency cựa | Điểm Dưới 200 ms mỏ (Mảng khi gán chạy cựa dải rã Full-Text túi nạp rẽ query search) | Áp gọi test cần mỏi tới nảy Đai rã rẽ Index Data Store phễu đo nảy cụ trích đo Database |
| Sốt Truyền Thông Trút Async giữa hai Service (Inter-Service mảng Event nhả Event Mảng) | Bục Mác < 5 nhả Giây đo giây trút bọc nảy nạp rẽ (End-to-End rã đo ngỏ báo E2E nhả gán)| Cự Từ Mở Khâu Khách Bọc đai Return sách báo Tới Nhịp Ngỏ Bọc Nhả Xóa Cấu Mác Hold update Status cự |
| Móc Chờ Bật Đai Sốt Giờ Cold Start Tolerance Ngỏ Delay Mảng  | Điểm Dưới 3 Giây cự Phễu Trích 3s là mác Acceptable móc ngỏ bục | Nãy cái bục này gán trích nảy rẽ mỏ ảnh cấu trích quyết của việc trích bộ Compute nảy báo cựa nạp (Lambda rã chóp vs Ổ hòm giỏ bọc  Containers nảy đai mảng). |
| Cô Mảng Điểm Lập Trích Gán Data Store Nạp (Data Isolation data)| Trích Gọi Mỗi Service tút tự thân nó Own phễu rã báo kho Data móc chóp DB ngỏ túi rẽ của riêng mình | Bọc Kỵ Báo Không Cho Trích Chéo dải Shared móc chung túi nảy DB chóp Data nạp (No Database rã trích Shared) chóp giữa mỏ của 2 túi Services đai. |
| Mỏ File Vách Gán Test Cài Bản Python Ver | Đai mảng Python rã bản 3.13.x | Cài khóa phễu đai cựa Nảy nạp file UV mốc `"requires-python = '>=3.13'"`. |

---

## Ổ Khóa Điểm Cự Bọc Rã Vòng Lệnh Móc Dev Workflow Run

```bash
uv sync
uv run pytest
uv run ruff check . && uv run ruff format .
```
