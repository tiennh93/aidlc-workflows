# Kế Hoạch Tạo Mã Nguồn — sci-calc

## Ngữ Cảnh
- **Đơn vị**: sci-calc (đơn vị duy nhất — toàn bộ ứng dụng)
- **Loại dự án**: Greenfield, Python 3.13, FastAPI
- **Thư mục gốc**: workspace/
- **Vị trí mã nguồn**: workspace/ (cấu trúc tech-env: pyproject.toml, src/sci_calc/, tests/)

## Trình Tự Các Bước

### Bước 1: Thiết Lập Cấu Trúc Dự Án
- [ ] Tạo `workspace/pyproject.toml`
- [ ] Tạo `workspace/src/sci_calc/__init__.py`
- [ ] Tạo `workspace/src/sci_calc/routes/__init__.py`
- [ ] Tạo `workspace/src/sci_calc/models/__init__.py`
- [ ] Tạo `workspace/src/sci_calc/engine/__init__.py`
- [ ] Tạo `workspace/tests/__init__.py`
- [ ] Tạo `workspace/tests/conftest.py` với fixture test client async

### Bước 2: Engine — Các Ngoại Lệ Tùy Chỉnh
- [ ] Tạo `workspace/src/sci_calc/engine/math_engine.py` — định nghĩa custom exceptions: `MathDomainError`, `MathDivisionByZeroError`, `MathOverflowError`

### Bước 3: Engine — Số Học
- [ ] Thêm các hàm vào `math_engine.py`: `add`, `subtract`, `multiply`, `divide`, `modulo`, `absolute`, `negate`
- [ ] Triển khai khả năng phát hiện tràn (kết quả là inf/-inf → ném ra OverflowError)

### Bước 4: Engine — Lũy Thừa và Căn
- [ ] Thêm các hàm lũy thừa vào `math_engine.py`: `power`, `sqrt_op`, `cbrt`, `square`, `nth_root`
- [ ] Triển khai xác thực miền giá trị (căn bậc 2 của số âm, giới hạn nth_root)

### Bước 5: Engine — Lượng Giác
- [ ] Thêm các hàm lượng giác vào `math_engine.py`: `sin_op`, `cos_op`, `tan_op`, `asin_op`, `acos_op`, `atan_op`, `atan2_op`, `sinh_op`, `cosh_op`, `tanh_op`, `asinh_op`, `acosh_op`, `atanh_op`
- [ ] Triển khai chuyển đổi đơn vị góc (độ ↔ radian)
- [ ] Triển khai xác thực miền cho các hàm lượng giác nghịch đảo

### Bước 6: Engine — Logarit
- [ ] Thêm các hàm logarit vào `math_engine.py`: `ln`, `log10_op`, `log2_op`, `log_op`, `exp_op`
- [ ] Triển khai xác thực miền (a <= 0, giới hạn cơ số)

### Bước 7: Engine — Thống Kê
- [ ] Thêm các hàm thống kê vào `math_engine.py`: `mean_op`, `median_op`, `mode_op`, `stdev_op`, `variance_op`, `pstdev_op`, `pvariance_op`, `min_op`, `max_op`, `sum_op`, `count_op`
- [ ] Triển khai xác thực số lượng phần tử tối thiểu

### Bước 8: Engine — Hằng Số
- [ ] Thêm các hàm hằng số vào `math_engine.py`: `get_constant`, `get_all_constants`
- [ ] Định nghĩa bản đồ hằng số: pi, e, tau, inf, nan, golden_ratio, sqrt2, ln2, ln10

### Bước 9: Engine — Chuyển Đổi Đơn Vị
- [ ] Thêm các hàm chuyển đổi vào `math_engine.py`: `convert_angle`, `convert_temperature`, `convert_length`, `convert_weight`
- [ ] Định nghĩa bảng hệ số chuyển đổi cho tất cả các đơn vị được hỗ trợ

### Bước 10: Models — Request Models
- [ ] Tạo `workspace/src/sci_calc/models/requests.py` — tất cả request models của Pydantic v2 kèm xác thực NaN

### Bước 11: Models — Response Models
- [ ] Tạo `workspace/src/sci_calc/models/responses.py` — các mô hình SuccessResponse, ErrorDetail, ErrorResponse

### Bước 12: Routes — Số Học
- [ ] Tạo `workspace/src/sci_calc/routes/arithmetic.py` — APIRouter với các điểm cuối POST cho mọi phép toán số học

### Bước 13: Routes — Lũy Thừa
- [ ] Tạo `workspace/src/sci_calc/routes/powers.py` — APIRouter với các điểm cuối POST cho mọi phép toán lũy thừa/căn

### Bước 14: Routes — Lượng Giác
- [ ] Tạo `workspace/src/sci_calc/routes/trigonometry.py` — APIRouter với các điểm cuối POST cho mọi phép toán lượng giác

### Bước 15: Routes — Logarit
- [ ] Tạo `workspace/src/sci_calc/routes/logarithmic.py` — APIRouter với các điểm cuối POST cho mọi phép logarit

### Bước 16: Routes — Thống Kê
- [ ] Tạo `workspace/src/sci_calc/routes/statistics.py` — APIRouter với các điểm cuối POST cho phép toán thống kê

### Bước 17: Routes — Hằng Số
- [ ] Tạo `workspace/src/sci_calc/routes/constants.py` — APIRouter với các GET endpoint cho hằng số

### Bước 18: Routes — Chuyển Đổi
- [ ] Tạo `workspace/src/sci_calc/routes/conversions.py` — APIRouter với các POST endpoint cho quy đổi

### Bước 19: Điểm Bắt Đầu Ứng Dụng
- [ ] Tạo `workspace/src/sci_calc/app.py` — app FastAPI, đăng ký routers, xử lý lỗi tùy chỉnh, endpoint health check

### Bước 20: Tests — Engine Unit Tests
- [ ] Tạo `workspace/tests/test_arithmetic.py`
- [ ] Tạo `workspace/tests/test_powers.py`
- [ ] Tạo `workspace/tests/test_trigonometry.py`
- [ ] Tạo `workspace/tests/test_logarithmic.py`
- [ ] Tạo `workspace/tests/test_statistics.py`
- [ ] Tạo `workspace/tests/test_constants.py`
- [ ] Tạo `workspace/tests/test_conversions.py`

### Bước 21: Tests — API Integration Tests
- [ ] Thêm integration tests bằng httpx.AsyncClient vào file test
- [ ] Test success responses
- [ ] Test error responses
- [ ] Test 404
- [ ] Test custom 422 handler

## Tổng: 21 bước, ~20 tệp mã nguồn, ~7 tệp kiểm thử
