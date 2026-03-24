# API Máy Tính Bỏ Túi Khoa Học

## Tóm Tắt Khái Quát

Một HTTP API phi trạng thái thực hiện các phép toán khoa học — số học, lượng giác, logarit, lũy thừa, thống kê và chuyển đổi đơn vị. Bất kỳ HTTP client nào cũng có thể sử dụng nó mà không cần cài đặt thư viện toán học. Máy tính ưu tiên tính chính xác, độ chuẩn xác và báo cáo lỗi rõ ràng hơn là thông lượng thô. Nó đóng vai trò như một ứng dụng minh chứng (golden test-case): đủ nhỏ để hiểu tường tận, nhưng đủ phong phú để kiểm tra công cụ sinh mã tự động qua nhiều khía cạnh.

## Tính Năng Trong Phạm Vi (MVP)

- Số học: cộng, trừ, nhân, chia, chia lấy dư (modulo), giá trị tuyệt đối (abs), phủ định (negate)
- Lũy thừa và căn: lũy thừa, căn bậc hai (sqrt), căn bậc ba (cbrt), căn bậc n (nth_root), bình phương (square)
- Lượng giác: sin, cos, tan, asin, acos, atan, atan2, sinh, cosh, tanh, asinh, acosh, atanh (chế độ độ và radian)
- Logarit: ln, log10, log2, log (cơ số tùy ý), exp
- Thống kê: trung bình (mean), trung vị (median), yếu vị (mode), độ lệch chuẩn mẫu (stdev), phương sai mẫu (variance), độ lệch chuẩn tổng thể (pstdev), phương sai tổng thể (pvariance), nhỏ nhất, lớn nhất, tổng, số lượng
- Hằng số: pi, e, tau, inf, nan, tỷ lệ vàng, sqrt2, ln2, ln10
- Chuyển đổi đơn vị: góc, nhiệt độ, chiều dài, khối lượng
- Endpoint kiểm tra sức khỏe (health-check)
- Phản hồi lỗi có cấu trúc cho mọi trường hợp thất bại
- Unit test và integration test

## Tính Năng Bị Loại Trừ Rõ Ràng (MVP)

- Lưu trữ liên tục hoặc tài khoản người dùng
- Giao diện người dùng đồ họa hoặc terminal
- Khả năng xử lý biểu tượng / đại số máy tính (CAS)
- Các thư viện số lớn hoặc độ chính xác tùy ý ngoài module `decimal` chuẩn của Python
- Xác thực, giới hạn tỷ lệ (rate-limiting), hoặc củng cố cho production
- Tính toán biểu thức từ đầu vào dạng chuỗi

## Đặc Tả API

Tất cả các endpoint đều chấp nhận và trả về `application/json`.

### Cấu Trúc Phản Hồi (Envelopes)

**Thành công:**

```json
{ "status": "ok", "operation": "<tên>", "inputs": { ... }, "result": <số | object> }
```

**Lỗi:**

```json
{ "status": "error", "operation": "<tên>", "inputs": { ... }, "error": { "code": "<MÃ>", "message": "..." } }
```

| Mã Lỗi | HTTP Status | Ý Nghĩa |
|---|---|---|
| `INVALID_INPUT` | 422 | Request body không qua được xác thực |
| `DIVISION_BY_ZERO` | 400 | Chia hoặc chia lấy dư cho không |
| `DOMAIN_ERROR` | 400 | Đầu vào nằm ngoài miền toán học (VD: sqrt(-1), log(0)) |
| `OVERFLOW` | 400 | Kết quả vượt quá phạm vi biểu diễn |
| `NOT_FOUND` | 404 | Không tìm thấy endpoint |

### Các Endpoint

**`GET /health`** — Trả về `{"status": "ok", "version": "0.1.0"}`.

**`POST /api/v1/arithmetic/{operation}`** — `add`, `subtract`, `multiply`, `divide`, `modulo` nhận `{"a": N, "b": N}`. `abs`, `negate` nhận `{"a": N}`.

**`POST /api/v1/powers/{operation}`** — `power` nhận `{"base": N, "exponent": N}`. `sqrt`, `cbrt`, `square` nhận `{"a": N}`. `nth_root` nhận `{"a": N, "n": int}`. Lỗi miền (domain error) nếu `a < 0` cho sqrt; lỗi miền nếu `a < 0` và `n` là số chẵn cho nth_root.

**`POST /api/v1/trigonometry/{operation}`** — Hầu hết nhận `{"a": N, "angle_unit": "radians"|"degrees"}` (mặc định là radians). `atan2` nhận `{"y": N, "x": N, "angle_unit": ...}`. Ràng buộc miền: asin/acos yêu cầu -1 <= a <= 1, acosh yêu cầu a >= 1, atanh yêu cầu -1 < a < 1.

**`POST /api/v1/logarithmic/{operation}`** — `ln`, `log10`, `log2` nhận `{"a": N}` (lỗi miền nếu a <= 0). `log` nhận `{"a": N, "base": N}` (lỗi miền nếu a <= 0, base <= 0, hoặc base = 1). `exp` nhận `{"a": N}`.

**`POST /api/v1/statistics/{operation}`** — Tất cả nhận `{"values": [N, ...]}`. Yêu cầu ít nhất 1 phần tử. `stdev`/`variance` yêu cầu ít nhất 2 phần tử. `pstdev`/`pvariance` yêu cầu ít nhất 1 phần tử. `mode` trả về mode nhỏ nhất nếu có trùng lặp.

**`GET /api/v1/constants/{name}`** — Trả về hằng số được chỉ định. `GET /api/v1/constants` trả về tất cả dưới dạng bản đồ (map).

**`POST /api/v1/conversions/{category}`** — Nhận `{"value": N, "from_unit": "...", "to_unit": "..."}`. Danh mục: góc (degrees/radians/gradians), nhiệt độ (celsius/fahrenheit/kelvin), chiều dài (meters/feet/inches/centimeters/millimeters/kilometers/miles/yards), khối lượng (kilograms/pounds/ounces/grams/milligrams/tonnes/stones).

## Nguyên Tắc Xử Lý Lỗi

1. Không bao giờ trả về lỗi 500 thuần túy. Bắt các lỗi miền toán học và lỗi tràn số (overflow) và chuyển đổi chúng thành cấu trúc phản hồi lỗi.
2. Để FastAPI/Pydantic xử lý các lỗi xác thực schema; ghi đè handler 422 mặc định để tuân thủ cấu trúc lỗi chung.
3. Ghi log các ngoại lệ không mong muốn ở cấp độ ERROR và trả về phản hồi `INTERNAL_ERROR` chung.

## Số Liệu Thành Công

- Tất cả các bài kiểm tra (test) pass (đạt) với độ bao phủ dòng >= 90%
- Các kết quả khớp với thư viện `math` chuẩn của Python tới <= 1 ULP cho các phép toán tiêu chuẩn
- Độ trễ phản hồi p95 < 50ms cho bất kỳ phép tính đơn lẻ nào

## Định Phiên Bản

API được định phiên bản qua tiền tố URL (`/api/v1/...`). Bản phát hành ban đầu là v0.1.0. Áp dụng Semver.
