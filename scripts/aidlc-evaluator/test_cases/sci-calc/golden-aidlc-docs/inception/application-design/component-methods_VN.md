# Các Phương Thức Của Các Thành Phần (Component Methods)

## 1. Lớp Engine — `math_engine.py`

### Số Học (Arithmetic)
- `add(a: float, b: float) -> float`
- `subtract(a: float, b: float) -> float`
- `multiply(a: float, b: float) -> float`
- `divide(a: float, b: float) -> float` — ném ra lỗi `DivisionByZeroError`
- `modulo(a: float, b: float) -> float` — ném ra lỗi `DivisionByZeroError`
- `absolute(a: float) -> float`
- `negate(a: float) -> float`

### Lũy Thừa (Powers)
- `power(base: float, exponent: float) -> float` — ném ra lỗi tràn dữ liệu `OverflowError`
- `sqrt(a: float) -> float` — ném ra ngoại lệ `DomainError` nếu a < 0
- `cbrt(a: float) -> float`
- `square(a: float) -> float`
- `nth_root(a: float, n: int) -> float` — ném ra `DomainError` nếu a < 0 và n là số chẵn

### Lượng Giác (Trigonometry)
- `sin(a: float, angle_unit: str) -> float`
- `cos(a: float, angle_unit: str) -> float`
- `tan(a: float, angle_unit: str) -> float`
- `asin(a: float, angle_unit: str) -> float` — ném ra `DomainError` nếu |a| > 1
- `acos(a: float, angle_unit: str) -> float` — ném ra `DomainError` nếu |a| > 1
- `atan(a: float, angle_unit: str) -> float`
- `atan2(y: float, x: float, angle_unit: str) -> float`
- `sinh(a: float) -> float`
- `cosh(a: float) -> float`
- `tanh(a: float) -> float`
- `asinh(a: float) -> float`
- `acosh(a: float) -> float` — ném ra `DomainError` nếu a < 1
- `atanh(a: float) -> float` — ném ra `DomainError` nếu |a| >= 1

### Logarit (Logarithmic)
- `ln(a: float) -> float` — ném ra `DomainError` nếu a <= 0
- `log10(a: float) -> float` — ném ra `DomainError` nếu a <= 0
- `log2(a: float) -> float` — ném ra `DomainError` nếu a <= 0
- `log(a: float, base: float) -> float` — ném ra `DomainError` khi vi phạm miền đo đạc giá trị
- `exp(a: float) -> float` — ném ra cờ lỗi `OverflowError`

### Thống Kê (Statistics)
- `mean(values: list[float]) -> float`
- `median(values: list[float]) -> float`
- `mode(values: list[float]) -> float` — trả về giá trị nhỏ nhất nếu có trường hợp các số liệu đồng hạng (ties)
- `stdev(values: list[float]) -> float` — yêu cầu độ dài len >= 2
- `variance(values: list[float]) -> float` — yêu cầu độ dài len >= 2
- `pstdev(values: list[float]) -> float`
- `pvariance(values: list[float]) -> float`
- `min_val(values: list[float]) -> float`
- `max_val(values: list[float]) -> float`
- `sum_val(values: list[float]) -> float`
- `count(values: list[float]) -> int`

### Hằng Số (Constants)
- `get_constant(name: str) -> float`
- `get_all_constants() -> dict[str, float]`

### Quy Đổi (Conversions)
- `convert_angle(value: float, from_unit: str, to_unit: str) -> float`
- `convert_temperature(value: float, from_unit: str, to_unit: str) -> float`
- `convert_length(value: float, from_unit: str, to_unit: str) -> float`
- `convert_weight(value: float, from_unit: str, to_unit: str) -> float`

## 2. Models Đối Tượng — `requests.py`

### Các Lớp Chứa Yêu Cầu (Request Models)
- `BinaryOperationRequest(a: float, b: float)` — kèm theo chức năng chặn kiểm tra số nguyên báo hiệu lỗi NaN validator
- `UnaryOperationRequest(a: float)` — đi kèm khối thiết đặt đo lường NaN validator
- `PowerRequest(base: float, exponent: float)` — đi kèm NaN validator
- `NthRootRequest(a: float, n: int)` — cài định mã xác nhận ngắt loại NaN validator
- `TrigRequest(a: float, angle_unit: str = "radians")` — gắn kèm trạm nhấp đo lọc rớt NaN validator
- `Atan2Request(y: float, x: float, angle_unit: str = "radians")` — thiết lập hệ thống rào lọc rớt NaN validator
- `LogRequest(a: float, base: float)` — được rào với bộ móc kiểm chứng lọc cặn rỗng lỗi NaN validator
- `StatisticsRequest(values: list[float])` — dán cấu hàm dò quét NaN validator
- `ConversionRequest(value: float, from_unit: str, to_unit: str)` — nối thêm phần kẹp bắt mã ngắt lỗi trống ảo NaN validator

## 3. Models Đối Tượng — `responses.py`

### Các Lớp Chứa Phản Hồi (Response Models)
- `SuccessResponse(status: str, operation: str, inputs: dict, result: Any)`
- `ErrorDetail(code: str, message: str)`
- `ErrorResponse(status: str, operation: str, inputs: dict, error: ErrorDetail)`

## 4. Bảng Định Tuyến (Routes) — Trên mỗi mô-đun định tuyến
- Cung cấp một bộ cổng `APIRouter` cho từng mô-đun gắn với một tiền tố đường dẫn URL tương ứng
- Các chức năng tuyến đường (Route functions): tiến hành phân tích nhận các phân khúc request → gọi dội vào bộ máy lỏi engine → trả gói ra khối đóng bộ phản hồi SuccessResponse hoặc là ErrorResponse
- Các trình xử lý ngoại lệ bắt các cảnh báo lỗi hệ lệnh trỏ móc từ engine và chuyển ánh xạ lại thành các mã lỗi (error codes)
