# Các Thành Phần (Components)

## 1. Điểm Khởi Tạo Ứng Dụng (Application Entry Point) — `sci_calc/app.py`
**Mục đích**: Class cấu hình cho ứng dụng FastAPI, middleware, các bộ xử lý lỗi (error handlers), và chức năng đăng ký router.
**Trách nhiệm**:
- Tạo một instance ứng dụng FastAPI kèm theo thông tin siêu dữ liệu (metadata) như tiêu đề, phiên bản
- Đăng ký mọi mô-đun định tuyến (route modules)
- Ghi đè (override) bộ xử lý lỗi validation mặc định để áp dụng định dạng bao gói lỗi chuẩn (standard error envelope)
- Đăng ký bộ xử lý ngoại lệ tổng quát (catch-all) để bắt các lỗi không dự tính trước
- Cung cấp endpoint cho health check (kiểm tra tình trạng sức khỏe hệ thống)

## 2. Lớp Định Tuyến (Routes Layer) — `sci_calc/routes/`

### 2.1 `arithmetic.py`
**Mục đích**: Xử lý các yêu cầu phép toán số học.
**Trách nhiệm**: Phân tích dữ liệu đầu vào, ủy quyền cho engine phân giải, bọc kết quả xuất ra dạng phản hồi success/error.

### 2.2 `powers.py`
**Mục đích**: Xử lý các yêu cầu tính lũy thừa và tính căn.
**Trách nhiệm**: Phân tích dữ liệu đầu vào, kiểm định các giới hạn miền giá trị (domain constraints), chuyển nhiệm vụ tính toán cho engine.

### 2.3 `trigonometry.py`
**Mục đích**: Xử lý các yêu cầu thuộc nhóm hàm lượng giác.
**Trách nhiệm**: Phân tích đầu vào, tiến hành chuyển đổi đơn vị góc (angle_unit), giao phó phép tính cho engine.

### 2.4 `logarithmic.py`
**Mục đích**: Xử lý những yêu cầu phép tính logarit.
**Trách nhiệm**: Xử lý đầu vào, kiểm tra giới hạn của cơ số và miền giá trị, ủy quyền tính toán cho engine.

### 2.5 `statistics.py`
**Mục đích**: Xử lý các yêu cầu tính toán về thống kê.
**Trách nhiệm**: Phân tích đầu vào, rà soát ràng buộc đối với kích thước danh sách, gọi khối engine.

### 2.6 `constants.py`
**Mục đích**: Cung cấp các hằng số toán học.
**Trách nhiệm**: Trả về dữ liệu của một hằng số chuyên biệt hoặc trọn bộ tất cả các hằng số.

### 2.7 `conversions.py`
**Mục đích**: Xử lý lệnh các phép tính quy đổi đơn vị.
**Trách nhiệm**: Tiếp nhận đầu vào, xác thực các đơn vị có tương thích không, yêu cầu chuyển chức năng tới khối engine xử lý.

## 3. Lớp Mô Hình (Models Layer) — `sci_calc/models/`

### 3.1 `requests.py`
**Mục đích**: Các mô hình yêu cầu (request models) bằng Pydantic v2 tiếp đón mọi hoạt động gọi chức năng API.
**Trách nhiệm**: Thực hiện rà soát thông qua Pydantic, khâu ép kiểu (type coercion), loại bỏ các đối số định dạng NaN.

### 3.2 `responses.py`
**Mục đích**: Các mô hình Pydantic v2 chuẩn hóa thông báo phản hồi (bao gói phản hồi thành công và lỗi - success and error envelopes).
**Trách nhiệm**: Xác định cấu trúc chuẩn cho khuôn phản hồi, liệt kê bảng mã hệ quy danh cho thông báo lỗi (error codes enum).

## 4. Lớp Lõi Tính Toán (Engine Layer) — `sci_calc/engine/`

### 4.1 `math_engine.py`
**Mục đích**: Chứa toàn bộ hệ thuật toán logic tính toán thuần túy — triệt để không phụ thuộc vào HTTP hay FastAPI.
**Trách nhiệm**:
- Phép tính số học cơ bản (add, subtract, multiply, divide, modulo, abs, negate)
- Phép tính lũy thừa và căn bậc (power, sqrt, cbrt, square, nth_root)
- Hàm toán dạng lượng giác (đầy đủ 14 phép toán có hỗ trợ định dạng đo góc)
- Hàm lô-ga-rít (ln, log10, log2, log, exp)
- Thao tác tính các số liệu thống kê (mean, median, mode, stdev, variance, v.v.)
- Thông báo hằng số
- Xử lý các quy đổi đơn vị đo lường (góc, nhiệt độ, chiều dài, trọng lượng)
- Báo hiệu ngắt vi phạm ném các ngoại lệ đặc thù theo miền dữ liệu (DomainError, DivisionByZeroError, OverflowError)
