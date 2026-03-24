# Yêu Cầu: API Máy Tính Khoa Học (Scientific Calculator API)

## Phân Tích Mục Tiêu
| Thuộc Tính | Giá Trị |
|---|---|
| **Yêu Cầu Người Dùng** | Xây dựng HTTP API vô trạng thái (stateless) cho các phép toán khoa học |
| **Loại Yêu Cầu** | Dự án mới (greenfield) |
| **Ước Tính Phạm Vi** | Ứng Dụng Đơn Lẻ — có nhiều thành phần (routes, models, engine) |
| **Ước Tính Độ Phức Tạp** | Trung Bình — bề mặt API xác định rõ, nhiều phép toán, xử lý lỗi toàn diện |
| **Độ Sâu Yêu Cầu** | Tiêu chuẩn |

---

## 1. Yêu Cầu Chức Năng

### FR-1: Kiểm Tra Tình Trạng Kỹ Thuật (Health Check)
- **FR-1.1**: `GET /health` trả về `{"status": "ok", "version": "0.1.0"}` với HTTP 200.

### FR-2: Phép Tính Số Học
- **FR-2.1**: `POST /api/v1/arithmetic/{operation}` hỗ trợ: `add`, `subtract`, `multiply`, `divide`, `modulo`, `abs`, `negate`.
- **FR-2.2**: Phép toán hai ngôi (`add`, `subtract`, `multiply`, `divide`, `modulo`) nhận `{"a": N, "b": N}`.
- **FR-2.3**: Phép toán một ngôi (`abs`, `negate`) nhận `{"a": N}`.
- **FR-2.4**: `divide` và `modulo` trả về lỗi `DIVISION_BY_ZERO` khi `b == 0`.

### FR-3: Lũy Thừa và Căn Bậc
- **FR-3.1**: `POST /api/v1/powers/{operation}` hỗ trợ: `power`, `sqrt`, `cbrt`, `square`, `nth_root`.
- **FR-3.2**: `power` nhận `{"base": N, "exponent": N}`.
- **FR-3.3**: `sqrt`, `cbrt`, `square` nhận `{"a": N}`.
- **FR-3.4**: `nth_root` nhận `{"a": N, "n": int}`.
- **FR-3.5**: `sqrt` trả về `DOMAIN_ERROR` khi `a < 0`.
- **FR-3.6**: `nth_root` trả về `DOMAIN_ERROR` khi `a < 0` và `n` là số chẵn.

### FR-4: Phép Tính Lượng Giác
- **FR-4.1**: `POST /api/v1/trigonometry/{operation}` hỗ trợ: `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `atan2`, `sinh`, `cosh`, `tanh`, `asinh`, `acosh`, `atanh`.
- **FR-4.2**: Hầu hết các phép toán nhận `{"a": N, "angle_unit": "radians"|"degrees"}` với giá trị mặc định là `"radians"`.
- **FR-4.3**: `atan2` nhận `{"y": N, "x": N, "angle_unit": "radians"|"degrees"}`.
- **FR-4.4**: Ràng buộc miền được thực thi: `asin`/`acos` yêu cầu `-1 <= a <= 1`, `acosh` yêu cầu `a >= 1`, `atanh` yêu cầu `-1 < a < 1`.
- **FR-4.5**: Vi phạm miền trả về `DOMAIN_ERROR`.

### FR-5: Phép Tính Logarit
- **FR-5.1**: `POST /api/v1/logarithmic/{operation}` hỗ trợ: `ln`, `log10`, `log2`, `log`, `exp`.
- **FR-5.2**: `ln`, `log10`, `log2` nhận `{"a": N}` — `DOMAIN_ERROR` nếu `a <= 0`.
- **FR-5.3**: `log` nhận `{"a": N, "base": N}` — `DOMAIN_ERROR` nếu `a <= 0`, `base <= 0`, hoặc `base == 1`.
- **FR-5.4**: `exp` nhận `{"a": N}`.

### FR-6: Phép Tính Thống Kê
- **FR-6.1**: `POST /api/v1/statistics/{operation}` hỗ trợ: `mean`, `median`, `mode`, `stdev`, `variance`, `pstdev`, `pvariance`, `min`, `max`, `sum`, `count`.
- **FR-6.2**: Tất cả các thao tác đều nhận `{"values": [N, ...]}` với yêu cầu mảng có ít nhất 1 phần tử.
- **FR-6.3**: `stdev`/`variance` yêu cầu ít nhất 2 phần tử.
- **FR-6.4**: `pstdev`/`pvariance` yêu cầu ít nhất 1 phần tử.
- **FR-6.5**: `mode` trả về một số duy nhất; trong trường hợp đồng hạng, trả về mode nhỏ nhất.

### FR-7: Hằng Số Toán Học
- **FR-7.1**: `GET /api/v1/constants/{name}` trả về một hằng số được chỉ định.
- **FR-7.2**: `GET /api/v1/constants` trả về tất cả hằng số dưới dạng map.
- **FR-7.3**: Các hằng số được hỗ trợ: `pi`, `e`, `tau`, `inf`, `nan`, `golden_ratio`, `sqrt2`, `ln2`, `ln10`.

### FR-8: Chuyển Đổi Đơn Vị
- **FR-8.1**: `POST /api/v1/conversions/{category}` nhận `{"value": N, "from_unit": "...", "to_unit": "..."}`.
- **FR-8.2**: Góc (Angle): `degrees`, `radians`, `gradians`.
- **FR-8.3**: Nhiệt độ (Temperature): `celsius`, `fahrenheit`, `kelvin`.
- **FR-8.4**: Chiều dài (Length): `meters`, `feet`, `inches`, `centimeters`, `millimeters`, `kilometers`, `miles`, `yards`.
- **FR-8.5**: Trọng lượng (Weight): `kilograms`, `pounds`, `ounces`, `grams`, `milligrams`, `tonnes`, `stones`.
- **FR-8.6**: Đơn vị không xác định trả về `INVALID_INPUT` (422).

### FR-9: Bao Đóng Phản Hồi (Response Envelope)
- **FR-9.1**: Phản hồi thành công: `{"status": "ok", "operation": "<name>", "inputs": {...}, "result": <number|object>}`.
- **FR-9.2**: Phản hồi lỗi: `{"status": "error", "operation": "<name>", "inputs": {...}, "error": {"code": "<CODE>", "message": "..."}}`.
- **FR-9.3**: Tất cả các endpoint đều nhận và trả về kiểu `application/json`.

### FR-10: Xử Lý Lỗi (Error Handling)
- **FR-10.1**: Các mã lỗi: `INVALID_INPUT` (422), `DIVISION_BY_ZERO` (400), `DOMAIN_ERROR` (400), `OVERFLOW` (400), `NOT_FOUND` (404).
- **FR-10.2**: Lỗi xác thực của Pydantic được bao gói trong cùng cấu trúc lỗi với mã code `INVALID_INPUT`.
- **FR-10.3**: Các kết quả nếu trả về thành `inf` hoặc `-inf` thì phải thông báo mã lỗi `OVERFLOW`.
- **FR-10.4**: Các đầu vào `NaN` sẽ bị khước từ với lỗi báo `INVALID_INPUT`.
- **FR-10.5**: Các endpoint không xác định trả về phản hồi `NOT_FOUND`.
- **FR-10.6**: Các ngoại lệ không dự trù trước sẽ được ghi log ở mức ERROR và trả về phản hồi `INTERNAL_ERROR` chung; tuyệt đối không bao giờ trả về HTTP 500 thuần túy mà không bọc khung lỗi.

---

## 2. Yêu Cầu Phi Chức Năng (Non-Functional Requirements)

### NFR-1: Hiệu Suất
- **NFR-1.1**: Thời gian khởi động < 2 giây.
- **NFR-1.2**: Độ trễ phản hồi (p95) < 50ms cho bất kỳ phép tính đơn lẻ nào.

### NFR-2: Tính Chính Xác
- **NFR-2.1**: Kết quả phải khớp với module `math` chuẩn (stdlib của Python) với sai số ≤ 1 ULP đối với các phép toán tiêu chuẩn.

### NFR-3: Kiểm Thử
- **NFR-3.1**: Tất cả các bài test phải pass với độ phủ code theo dòng (line coverage) ≥ 90% (điều kiện bắt buộc — việc chạy test bị đánh fail nếu tỷ lệ dưới 90%).
- **NFR-3.2**: Các bài unit tests chạy kiểm thử tệp `math_engine.py` một cách trực tiếp thông qua dùng bảng dữ liệu đã biết trước (known-value tables).
- **NFR-3.3**: Các bài tests tích hợp (Integration tests) dùng `httpx.AsyncClient` kết hợp với TestClient của FastAPI.
- **NFR-3.4**: Boundary tests (test cho các vùng biên) phải xác minh mọi tình huống vi phạm miền giá trị đều cho ra kết quả mã lỗi chính xác.

### NFR-4: Bảo Mật
- **NFR-4.1**: Kích cỡ phần body tối đa của request được giới hạn ở giới mức 1 MB.
- **NFR-4.2**: Không áp dụng cơ chế xác thực (authentication), không giới hạn tần suất (rate-limiting), hoặc cơ chế bảo mật bổ sung cho môi trường production hardening trong giai đoạn MVP.

### NFR-5: Khả Năng Bảo Trì
- **NFR-5.1**: Mã lệnh (code) được kiểm tra cú pháp và rập khuôn định dạng trích lọc bằng `ruff` (chiều dài dòng mặc định 100, nhắm mục tiêu target py313).
- **NFR-5.2**: Có sự phân tách chức năng rõ ràng (separation of concerns): routes, models, engine.

---

## 3. Ràng Buộc Kỹ Thuật (Technical Constraints)

| Ràng Buộc | Cấu Hình Giá Trị (Value) |
|---|---|
| Ngôn Ngữ | Python 3.13 |
| Trình Quản Lý Gói | uv (không dùng pip, poetry, conda) |
| Framework | FastAPI + Pydantic v2 |
| Máy Chủ ASGI | uvicorn |
| Build Backend | hatchling |
| Framework Chạy Test | pytest + pytest-asyncio + httpx + pytest-cov |
| Linter/Formatter | ruff |
| Không Được Phép (Prohibited) | Flask, Django, requests, sympy, pandas, numpy, pip, poetry, pipenv, black, flake8, isort |

---

## 4. Định Cấu Phiên Bản API (API Versioning)
- Tiền tố URL (URL prefix): `/api/v1/...`
- Bản phát hành sơ khởi: v0.1.0
- Áp dụng cấu chuẩn Semver.

---

## 5. Nằm Ngoài Phạm Vi Cho Bản Phát Hành MVP (Out of Scope)
- Hệ cơ quản lý lưu trữ liên tục lâu dài (Persistent storage) hoặc tạo lập theo dõi tài khoản người dùng
- Các giao diện đồ họa hoặc giao diện dạng terminal (UI)
- Các công năng ký hiệu tính toán dựa trên đại số (Symbolic / CAS capabilities)
- Độ chính xác điểm đo tùy ý (Arbitrary-precision) nếu vượt xa giới hạn của module Python `decimal`
- Đánh giá biểu thức từ lệnh của đầu vào chuỗi văn bản (Expression evaluation from string input)
