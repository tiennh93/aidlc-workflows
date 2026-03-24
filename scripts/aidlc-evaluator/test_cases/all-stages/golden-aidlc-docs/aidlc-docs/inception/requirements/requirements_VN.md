# Tài Liệu Yêu Cầu API Thư Viện Cộng Đồng BookShelf

## Tóm Tắt Phân Tích Mục Tiêu (Intent Analysis Summary)

| Thuộc tính | Giá trị |
|-----------|-------|
| **Yêu cầu của người dùng** | Xây dựng nền tảng BookShelf triển khai trên đám mây với hai dịch vụ có thể triển khai độc lập (Dịch vụ Danh mục và Dịch vụ Mượn trả) để quản lý thư viện cộng đồng |
| **Loại yêu cầu** | Dự án Mới (Greenfield) |
| **Ước tính phạm vi** | Toàn hệ thống — hai microservices với xác thực chung, giao tiếp hướng sự kiện (event-driven) và các quy tắc nghiệp vụ toàn diện |
| **Ước tính độ phức tạp** | Phức tạp — nhiều dịch vụ, RBAC, sự kiện bất đồng bộ, thực thi chính sách mượn trả, giao tiếp giữa các dịch vụ |
| **Độ sâu yêu cầu** | Toàn diện |

---

## 1. Yêu Cầu Chức Năng

### 1.1 Dịch Vụ Danh Mục (Catalog Service)

#### FR-CAT-001: Các Thao Tác CRUD Sách
- Thêm sách với: tiêu đề, tác giả, ISBN (tùy chọn), danh mục, tổng số bản sao (total_copies)
- Cập nhật siêu dữ liệu của sách (tiêu đề, tác giả, ISBN, danh mục, tổng số bản sao)
- Lấy thông tin một cuốn sách theo ID
- Liệt kê tất cả sách
- Xóa sách (xóa mềm hoặc xóa cứng — chỉ dành cho thủ thư/quản trị viên)
- Tất cả các trường văn bản đều có ràng buộc độ dài tối đa

#### FR-CAT-002: Tìm Kiếm Sách
- Tìm kiếm toàn văn bản trên các trường tiêu đề và tác giả (khớp chuỗi con)
- Lọc theo danh mục
- Lọc theo tình trạng sẵn có (số bản sao có sẵn > 0)
- Hỗ trợ kết hợp tìm kiếm + lọc

#### FR-CAT-003: Theo Dõi Tình Trạng Sẵn Có của Sách
- Mỗi cuốn sách theo dõi `tổng số bản sao` (`total_copies`) và `số bản sao có sẵn` (`available_copies`)
- `available_copies` giảm xuống khi mượn (thông qua sự kiện hoặc lệnh gọi API từ Dịch vụ Mượn trả)
- `available_copies` tăng lên khi trả (thông qua sự kiện hoặc lệnh gọi API từ Dịch vụ Mượn trả)
- `available_copies` không bao giờ được nhỏ hơn 0 hoặc lớn hơn `total_copies`

#### FR-CAT-004: API Tình Trạng Sẵn Có của Sách
- API endpoint nội bộ cho Dịch vụ Mượn trả để xác minh sự tồn tại của sách và số bản sao có sẵn
- Trả về ID sách, tiêu đề, tổng số bản sao, số bản sao có sẵn

#### FR-CAT-005: Kiểm Tra Trạng Thái Hoạt Động (Health Check)
- `GET /api/v1/catalog/health` trả về trạng thái của dịch vụ và phiên bản

### 1.2 Dịch Vụ Mượn Trả (Lending Service)

#### FR-LND-001: Đăng Ký Thành Viên
- Đăng ký với: tên, email, mật khẩu
- Email phải là duy nhất
- Mật khẩu được lưu trữ bằng hàm băm thích ứng (bcrypt)
- Tự động gán vai trò "thành viên" (member) khi đăng ký
- Trả về ID thành viên và hồ sơ (không bao gồm mật khẩu)

#### FR-LND-002: Xác Thực Thành Viên
- Đăng nhập bằng email và mật khẩu
- Trả về JWT token có thời hạn 24 giờ
- JWT bao gồm: ID thành viên (member_id), email, vai trò (role)
- Cả hai dịch vụ đều xác thực JWT độc lập

#### FR-LND-003: Quản Lý Hồ Sơ Thành Viên
- Thành viên có thể xem và cập nhật hồ sơ của chính họ (tên, email)
- Quản trị viên có thể xem hồ sơ của bất kỳ thành viên nào
- Thủ thư có thể xem hồ sơ của bất kỳ thành viên nào

#### FR-LND-004: Kiểm Soát Truy Cập Dựa Trên Vai Trò (RBAC)
- Ba vai trò: Quản trị viên (Admin), Thủ thư (Librarian), Thành viên (Member)
- **Quản trị viên**: Có toàn quyền truy cập vào tất cả các endpoints trong cả hai dịch vụ
- **Thủ thư**: Quản lý danh mục (CRUD, tìm kiếm), hoạt động mượn trả (xử lý trả sách, quản lý đặt trước), xem báo cáo
- **Thành viên**: Tự phục vụ (tự mượn sách, tự đặt trước, tự xem phí, tự xem hồ sơ, tìm kiếm danh mục)
- **Công khai (Public)**: Đăng ký, đăng nhập, kiểm tra trạng thái hoạt động — không yêu cầu JWT

#### FR-LND-005: Mượn Sách (Checkout)
- Thành viên mượn sách bằng ID sách
- Các quy tắc xác nhận trước khi mượn:
  - Sách tồn tại (được xác minh qua HTTP call đến Dịch vụ Danh mục)
  - Số bản sao có sẵn > 0
  - Số sách thành viên đang mượn < 5 (mức tối đa có thể định cấu hình)
  - Phí chưa thanh toán của thành viên ≤ ngưỡng 10.00$ (có thể định cấu hình); chặn lại nếu vượt mức
- Khi mượn thành công:
  - Ghi nhận lượt mượn với: ID thành viên, ID sách, ngày mượn (checkout_date theo UTC), ngày đến hạn (due_date = ngày mượn + 14 ngày), trạng thái=đang hoạt động (active), số lần gia hạn (renewal_count)=0
  - Giảm `available_copies` trong Dịch vụ Danh mục
- Ngày đến hạn: 14 ngày kể từ ngày mượn

#### FR-LND-006: Trả Sách (Return)
- Thành viên, Thủ thư hoặc Quản trị viên trả một cuốn sách bằng ID lượt mượn (checkout ID)
- Khi trả sách:
  - Tính phí trễ hạn nếu quá hạn: 0.25$/ngày, tối đa 10.00$ cho mỗi lượt mượn
  - Nếu phí trễ hạn > 0, tạo bản ghi phí cho thành viên
  - Cập nhật trạng thái lượt mượn thành "đã trả" (returned), đặt ngày trả (return_date)
  - Tăng `available_copies` trong Dịch vụ Danh mục
  - Kiểm tra hàng đợi đặt trước đồng bộ: nếu có các đặt trước cho cuốn sách này, đáp ứng đặt trước tiếp theo (cập nhật trạng thái đặt trước thành "sẵn sàng" / ready)
- Tất cả timestamps đều ở định dạng UTC

#### FR-LND-007: Gia Hạn (Renewal)
- Thành viên gia hạn lượt mượn hiện tại bằng checkout ID
- Các quy tắc xác nhận:
  - Lượt mượn đang hoạt động (chưa trả)
  - Lượt mượn thuộc về thành viên yêu cầu
  - Số lần gia hạn < 2 (tối đa 2 lần gia hạn cho mỗi lượt mượn)
  - Không có các yêu cầu đặt trước (hold) nào đang chờ cho cuốn sách này
- Khi gia hạn: gia hạn `due_date` thêm 14 ngày kể từ `due_date` hiện tại, tăng `renewal_count` thêm 1

#### FR-LND-008: Lượt Mượn Đang Hoạt Động (Active Checkouts)
- Liệt kê các lượt mượn đang hoạt động cho thành viên yêu cầu
- Mỗi bản ghi bao gồm: ID lượt mượn, ID sách, tiêu đề sách, ngày mượn, ngày đến hạn, số lần gia hạn
- Quản trị viên / Thủ thư có thể liệt kê các lượt mượn đang hoạt động cho bất kỳ thành viên nào

#### FR-LND-009: Đặt Trước (Hold Placement)
- Thành viên đặt trước sách bằng ID sách
- Các quy tắc xác nhận:
  - Sách tồn tại (được xác minh qua Dịch vụ Danh mục)
  - Không có bản sao nào khả dụng (available_copies == 0)
  - Số lượng đặt trước đang hoạt động của thành viên < 3 (mức tối đa có thể định cấu hình)
  - Thành viên hiện chưa có yêu cầu đặt trước nào đối với cuốn sách này
- Khi đặt trước: ghi nhận đặt trước với ID thành viên, ID sách, ngày đặt trước (hold_date), trạng thái=đang chờ (waiting), vị trí trong hàng đợi (queue_position theo FIFO)

#### FR-LND-010: Hủy Đặt Trước (Hold Cancellation)
- Thành viên hủy yêu cầu đặt trước của riêng họ bằng ID đặt trước (hold ID)
- Thủ thư / Quản trị viên có thể hủy bất kỳ yêu cầu đặt trước nào
- Khi hủy: cập nhật trạng thái thành "đã hủy" (cancelled), sắp xếp lại các vị trí trong hàng đợi

#### FR-LND-011: Trạng Thái Hàng Đợi Đặt Trước (Hold Queue Status)
- Lấy danh sách hàng đợi đặt trước cho một cuốn sách cụ thể: danh sách các yêu cầu đặt trước kèm theo vị trí và trạng thái
- Thành viên có thể thấy vị trí của chính họ trong hàng đợi

#### FR-LND-012: Đáp Ứng Đặt Trước (Tối Thiểu - MVP Đồng Bộ)
- Khi một cuốn sách được trả và có yêu cầu đặt trước đang ở trạng thái "đang chờ" cho cuốn sách đó:
  - Đáp ứng yêu cầu đặt trước đầu tiên theo thứ tự FIFO
  - Cập nhật trạng thái yêu cầu đặt trước từ "đang chờ" thành "sẵn sàng"
  - Không có thời gian ân hạn — đáp ứng ngay lập tức
- Việc xử lý bất đồng bộ thực sự (SQS/EventBridge) được hoãn lại cho Giai đoạn 2

#### FR-LND-013: Theo Dõi Phí (Fee Tracking)
- Theo dõi các khoản phí chưa thanh toán của mỗi thành viên
- Bản ghi phí bao gồm: ID phí, ID thành viên, ID lượt mượn, số tiền, ngày tạo, trạng thái (chưa thanh toán/đã thanh toán/thanh toán một phần)
- Liệt kê tất cả các khoản phí cho thành viên yêu cầu
- Quản trị viên / Thủ thư có thể xem phí của bất kỳ thành viên nào

#### FR-LND-014: Thanh Toán Phí (Fee Payment)
- Ghi nhận thanh toán đối với các khoản phí chưa hoàn tất của thành viên
- Cho phép thanh toán một phần
- Quản trị viên và Thủ thư có thể xử lý thanh toán
- Bản ghi thanh toán bao gồm: ID thanh toán, ID thành viên, số tiền, ngày thanh toán

#### FR-LND-015: Báo Cáo Quá Hạn (Overdue Report)
- Liệt kê tất cả các lượt mượn đang quá hạn (due_date < now và status=đang hoạt động)
- Bao gồm: tên thành viên, email thành viên, tiêu đề sách, ngày mượn, ngày đến hạn, số ngày quá hạn
- Chỉ có thể truy cập bởi vai trò Thủ thư và Quản trị viên

#### FR-LND-016: Tóm Tắt Bộ Sưu Tập (Collection Summary)
- Tổng số sách, tổng số thành viên, số sách đã mượn, số sách có sẵn, tổng số phí chưa thanh toán
- Chỉ có thể truy cập bởi vai trò Quản trị viên

#### FR-LND-017: Kiểm Tra Trạng Thái Hoạt Động
- `GET /api/v1/lending/health` trả về trạng thái của dịch vụ và phiên bản

### 1.3 Giao Tiếp Giữa Các Dịch Vụ (Inter-Service Communication)

#### FR-ISC-001: Xác Minh Sách
- Dịch vụ Mượn trả gọi Dịch vụ Danh mục qua HTTP để xác minh sự tồn tại của cuốn sách và tính sẵn có trước khi thực hiện mượn sách / đặt trước
- Lệnh gọi HTTP trực tiếp (không dùng hệ thống phân phối sự kiện) để xác nhận đồng bộ

#### FR-ISC-002: Cập Nhật Tính Sẵn Có
- Dịch vụ Mượn trả gọi Dịch vụ Danh mục qua HTTP để giảm/tăng `available_copies` khi mượn / trả
- Hoạt động nguyên tử — nếu việc cập nhật thất bại, thao tác mượn / trả cũng phải thất bại

### 1.4 Hành Vi Hủy Kích Hoạt Tài Khoản (Account Deactivation Behavior)

#### FR-ACC-001: Hủy Kích Hoạt Tài Khoản
- Quản trị viên có thể hủy kích hoạt tài khoản của thành viên
- Khi tài khoản bị hủy kích hoạt: các yêu cầu đặt trước và lượt mượn sách hiện tại vẫn hoạt động, nhưng không được phép mượn sách, mở đặt trước hay gia hạn mới
- Việc tự động đình chỉ tài khoản nằm ngoài phạm vi MVP (được hoãn sang Giai đoạn 2)

---

## 2. Yêu Cầu Phi Chức Năng (Non-Functional Requirements)

### 2.1 Hiệu Suất (Performance)
| Tiêu chí | Mục tiêu |
|--------|--------|
| Thời gian phản hồi API (p95) | < 100ms |
| Độ trễ tìm kiếm toàn văn bản | < 200ms |
| Số lượng người dùng đồng thời | 100 người dùng cùng lúc |
| Độ trễ do gọi dịch vụ liên thông | Tăng thêm < 50ms |

### 2.2 Độ Tin Cậy (Reliability)
| Tiêu chí | Mục tiêu |
|--------|--------|
| Tỷ lệ khả dụng của API (Uptime) | 99.9% |
| Độ bền dữ liệu (Data durability) | Sao chép bảo lưu dư thừa (Multi-AZ replication) |

### 2.3 Bảo Mật (Security)
- Kiểm soát truy cập phân quyền RBAC với ba vai trò được thực thi trên mọi điểm cuối định tuyến (endpoints)
- Xác thực bằng JWT với thời gian hết hạn là 24 giờ
- Băm mật khẩu bằng thuật toán bcrypt
- Xác thực đầu vào hợp lệ (Input validation) qua lớp thư viện Pydantic đối với tất cả các endpoints
- Không đưa thông tin PII định danh cá nhân vào logs
- Mã hóa dữ liệu tĩnh lưu trữ (encryption at rest) và trên đường truyền tải mạng (encryption in transit)
- Thực thi đúng các bộ quy tắc mở rộng về bảo mật (từ SECURITY-01 đến SECURITY-15)

### 2.4 Khả Năng Mở Rộng (Scalability)
- Mỗi dịch vụ vận hành và dễ dàng quy mô hóa độc lập
- Dịch vụ Danh mục được tối ưu hóa cho khối lượng công việc thiên về mức độ đọc dữ liệu (read-heavy)
- Dịch vụ Mượn trả được tối ưu hóa cho khối lượng công việc thiên về mức độ ghi dữ liệu (write-heavy)
- Triển khai cho một thư viện độc lập tại cấu hình MVP ban đầu (hỗ trợ chế độ multi-tenant tại Giai đoạn 2)

### 2.5 Kiểm Thử (Testing)
| Loại Kiểm Thử | Mục tiêu |
|-----------|--------|
| Tỷ lệ kiểm thử dòng mã đơn vị (Unit test coverage) | ≥ 90% dòng mã |
| Các bài kiểm thử tích hợp (Integration tests) | Toàn bộ API endpoints cần được test đầy đủ |
| Kiểm thử hợp đồng giao tiếp API (Contract tests) | Khớp chặt chẽ tương thích nội dung OpenAPI spec |

### 2.6 Khả Năng Bảo Trì (Maintainability)
- Khả thi cho mô hình bảo trì đội ngũ phát triển nhóm nhỏ/khá ít người
- Ngân sách chi phí duy trì phần hạ tầng ước tính dưới 50$ hằng tháng
- Giữ được độ ranh giới tách biệt mạnh/chi tiết giữa các khối dịch vụ
- Chuẩn hóa định dạng bao phản hồi nhận tin API (API envelope) xuyên suốt cấu trúc code

---

## 3. Tiêu Chuẩn Thiết Kế API (API Design Standards)

- **Kiểu mẫu**: REST với nội dung trả về JSON
- **Phiên bản hóa**: tiền tố của API `/api/v1/`
- **Phong cách đặt tên trường (Field naming)**: snake_case
- **Cấu trúc phản hồi thành công (Success envelope)**: `{ "status": "ok", "data": { ... } }`
- **Cấu trúc phản hồi lỗi (Error envelope)**: `{ "status": "error", "error": { "code": "MÃ_LỖI", "message": "..." } }`
- **Các quy tắc mã lỗi (Error codes)**: VALIDATION_ERROR (422), NOT_FOUND (404), UNAUTHORIZED (401), FORBIDDEN (403), CONFLICT (409), INTERNAL_ERROR (500)

---

## 4. Công Nghệ và Công Cụ (Technology Stack)

- **Ngôn ngữ**: Python 3.13+
- **Framework**: FastAPI 0.115+ kết hợp mô-đun Pydantic 2.x
- **Máy chủ ứng dụng (Server)**: uvicorn 0.34+
- **Kiểm thử**: pytest 8.x, pytest-asyncio, httpx, pytest-cov
- **Công cụ rà soát tiêu chuẩn code (Linting)**: ruff 0.9+
- **Trình quản lý gói**: uv
- **Cơ sở hạ tầng dưới dạng mã (IaC)**: AWS CDK 2.x (hoãn lại — chỉ triển khai code ứng dụng cho phiên bản MVP)
- **Đám mây**: AWS us-east-1
- **Cơ sở dữ liệu**: Sẽ định đoạt cụ thể trong các chặng thiết kế NFR Yêu Cầu / Thiết kế Hạ Tầng (Infrastructure Design)

---

## 5. Các Lựa Chọn Thiết Kế Kiến Trúc (Architectural Decisions)

| Quyết định | Lựa chọn | Lý do |
|----------|--------|-----------|
| Kiến trúc dịch vụ | Hai dịch vụ độc lập phân tách nhau (Dịch vụ Danh mục + Dịch vụ Mượn khoản cấp) | Phù hợp đúng tham số yêu cầu dự án; đảm bảo scale độc lập cho từng module sau này |
| Cấp quyền định danh (Xác thực) | JWT mức ứng dụng cơ bản (PyJWT + Bcrypt); cả hai dịch vụ tự nhận biết validate JSON token nội tại chung | Đỡ ràng buộc hơn các giải pháp AWS Cognito phức tạp cho pha MVP đầu; dịch vụ mượn sách sẽ trực tiếp điều hướng data account |
| Giao tiếp gọi dịch vụ với dịch vụ qua kết nối đồng bộ | Lệnh gọi HTTP gọi trực tiếp giao tiếp qua httpx | Đơn giản, cực dễ tự test trực diện; Chiều đi từ Mượn sách tới Danh mục dùng lúc xét xác thực đầu sách |
| Quy cách xử lý chờ đặt trước (hold-queue) | Xử lý chốt kiểm soát tự nhiên trực tiếp đồng bộ sau mỗi lần gọi API Return Book | Phù hợp nhanh chống thiết lập phiên bản khởi tạo; luồng SQS nhắn tin hoãn xử lý cho đến Giai đoạn 2 |
| Định mức ngưỡng nợ phí giới hạn trước khi mượn đọc | Chặn mượn sách mới nếu dư nợ tồn đọng vượt mức giới hạn tiêu chuẩn 10.00$ | Biến số định tính cho phép tùy biến thông số ngưỡng cho nhà điều hành thư viện |
| Tối đa phí trễ hạn đóng chốt | Tối đa đạt mức 10.00$ trên mỗi đầu sách đã check out | Cố định theo như tài liệu định hình hướng tiếp cận mục tiêu đề án cấp thiết |
| Lớp mã định nghĩa đám mây Cloud CDK | Trì hoãn gác lại - mục đích hoàn tất code API trọng tâm cốt lõi MVP | Nhằm giữ độ gọn lẹ, làm xong logic ứng dụng web; một vài bộ template docs có cấp kèm sơ qua |
| Lớp dữ liệu Database | Gác tiếp cho giai đoạn xét duyệt NFR | Danh mục gợi ý trong phần công nghệ bao gốm DynamoDB so với RDS PostgreSQL; Phụ thuộc cao vào nhịp độ tương tác tần số read/write cụ thể |

---

## 6. Ranh Giới Phạm Vi MVP (MVP Scope Boundaries)

### Trong Phạm Vi (In Scope)
- Sách CRUD, thao tác tìm kiếm lọc, nhận biết tự động tình trạng khả dụng trạng thái
- Mở khóa cấu phần xác thực login account, tạo account cá nhân member, profile lưu trữ, hỗ trợ Auth JWT
- Role Permissions/RBAC (Phân tầng cho Quản trị viên, Thủ thư, Người Thường Đọc giả)
- Nghiệp vụ Checkout thư viện, Return trả, Gia hạn với cơ chế chính sách policy đi kèm
- Order Hold xếp hàng cho đầu tài liệu khi trống sách thực, cancel đặt trước, lấy vị trí báo cáo vị trí, tiếp nhận gỡ lượt đồng bộ
- Quy trình thanh toán chi trả, track báo cáo log lịch sử
- Ráp thông số list bảng người cần đòi sách và quản lý chung Collection inventory
- Thông số health status báo động server
- Gói Test bộ kiểm chức năng đơn vị code + API liên mạng nội dịch vụ (Bọc độ phủ >90%)
- Triển xuất tài liệu hệ thống định dạng API chuẩn hóa bằng tệp OpenAPI

### Ngoài Phạm Vi (Hoãn Lại - Out of Scope / Deferred)
- Hệ thống notification hòm thư (Tương lai - Giai đoạn 2)
- Hỗ trợ mô hình liên danh thiết đặt chi nhánh đa trụ sở phân lô data (Giai đoạn 2)
- Hệ thiết bị bắn mã vạch thiết bị Barcode ISBN scanner thực (Giai đoạn 2)
- Theo dõi thống kê phân tích số liệu đồ thị Advanced reporting (Giai đoạn 2)
- Ráp nối AI Gợi Ý Tự động Đề Xuất (Giai đoạn 3)
- Các cấu trúc hệ tự auto block người đọc khoá thẻ tự động trễ vi phạm (Giai đoạn 2)
- Mẫu chức năng đặt kích hoạt khôi phục mã khóa Password (Giai đoạn 2)
- Phân tách trang nhỏ giới hạn phân trang hiển thị (Giai đoạn 2)
- Toàn bộ cơ cấu IAC AWS định tuyến viết tay CDK (Chỉ tập trung lên mẫu file tài liệu docs tham khảo)

---

## 7. Các Quyết Định Mở Cho Các Giai Đoạn Sau (Open Decisions for Later Stages)

| Quyết định | Giai đoạn |
|----------|-------|
| Quyết định chốt cấu trúc cơ sở dữ liệu nền cuối (sử dụng phi quan hệ DynamoDB hay cấu trúc dọc chuẩn RDS PostgreSQL) | Khâu Định hình NFR yêu cầu thiết kết cơ sở hạ tầng mạng lưới |
| Các tính toán điện toán Server chạy trên tài nguyên môi trường nào (AWS Lambda vs ECS AWS Fargate Container) | Khâu Định hình NFR yêu cầu thiết kết cơ sở hạ tầng |
| Thiết định cấu hình chuẩn API Gateway Router đầu ngõ (Gateway configuration) | Lô Thiết Kế Architect System Cơ Sở Hạ Tầng mạng lưới |
| Lóc nền mô hình trao đổi hàng chờ nhắn tin đa ứng biến Event-Driven pha sau | Lô Thiết Kế Cơ Sở Hạ Tầng mạng lưới |
