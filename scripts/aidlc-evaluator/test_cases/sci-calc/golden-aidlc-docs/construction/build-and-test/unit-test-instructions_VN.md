# Thực Thi Kiểm Thử Đơn Vị (Unit Test Execution)

## Chạy Các Bài Kiểm Thử Đơn Vị (Run Unit Tests)

### 1. Thực Thi Tất Cả Các Bài Kiểm Thử (môi trường tiêu chuẩn)
```bash
cd workspace
uv run pytest tests/ -v --cov=sci_calc --cov-report=term-missing --cov-fail-under=90
```

### 2. Thực Thi Tất Cả Các Bài Kiểm Thử (môi trường Windows bị lỗi asyncio)
```bash
cd workspace
set PYTHONPATH=src
python -m pytest tests/ -v -p no:anyio -p no:asyncio
```

### 3. Xem Xét Kết Quả Kiểm Thử (Review Test Results)

#### Kết quả mong đợi: 192 bài kiểm thử vượt qua (pass), 0 bài thất bại (failures)

| Mô-đun Kiểm Thử (Test Module) | Kiểm Thử Engine (Engine Tests) | Kiểm Thử API (API Tests) | Tổng Số (Total) |
|---------------------|-------------|-----------|-------|
| test_arithmetic.py  | 20          | 12        | 32    |
| test_constants.py   | 12          | 4         | 16    |
| test_conversions.py | 21          | 7         | 28    |
| test_logarithmic.py | 17          | 9         | 26    |
| test_powers.py      | 16          | 9         | 25    |
| test_statistics.py  | 18          | 14        | 32    |
| test_trigonometry.py| 25          | 8         | 33    |
| **TỔNG CỘNG**       | **129**     | **63**    | **192** |

- **Mục Tiêu Độ Bao Phủ (Test Coverage Target)**: ≥90%
- **Vị Trí Báo Cáo Kiểm Thử (Test Report Location)**: stdout (thông qua `--cov-report=term-missing`)

### 4. Sửa Các Bài Kiểm Thử Bị Lỗi (Fix Failing Tests)
Nếu các bài kiểm thử gặp lỗi:
1. Xem đầu ra chi tiết để biết bài kiểm thử nào đã thất bại và lý do
2. Kiểm tra dấu vết lỗi (error traceback)
3. Khắc phục các vấn đề mã nguồn trong `src/sci_calc/` hoặc trong `tests/`
4. Chạy lại hoàn toàn các bài kiểm thử cho đến khi tất cả 192 bài đều đạt (pass)

## Kiến Trúc Kiểm Thử (Test Architecture)

### Kiểm Thử Đơn Vị Lớp Engine (Engine Unit Tests) (129)
- Import trực tiếp từ `sci_calc.engine.math_engine`
- Kiểm thử mọi hàm toán học với các đầu vào hợp lệ, các trường hợp ranh giới/viền (edge-case) và các đầu vào gây lỗi
- Xác minh các ngoại lệ tùy chỉnh (`MathDomainError`, `MathDivisionByZeroError`, `MathOverflowError`)
- Không có bất kỳ sự phụ thuộc nào vào HTTP hoặc framework

### Kiểm Thử Tích Hợp API (API Integration Tests) (63)
- Sử dụng test client để gọi toàn bộ các bộ định tuyến (endpoint paths) HTTP
- Xác thực mã trạng thái (status codes), cấu trúc phản hồi, và cấu trúc cảnh báo lỗi (error envelopes)
- Kiểm thử các luồng chuẩn (happy path), lỗi vi phạm miền dữ liệu (domain errors), lỗi xác thực (validation errors) và các lỗi 404
- Thực hành đánh giá xác thực mô hình trên Pydantic (từ chối dữ liệu NaN, ép kiểu / type coercion)

### Client Kiểm Thử (Test Client)
- **Tiêu chuẩn (Standard)**: `starlette.testclient.TestClient` (yêu cầu hệ thống asyncio phải hoạt động bình thường)
- **Dự phòng (Fallback)**: Máy khách đồng bộ tùy chỉnh `SyncTestClient` được định nghĩa trong `conftest.py` hỗ trợ điều khiển các trình xử lý async (async handlers) một cách đồng bộ mà không nhập thư viện `asyncio` — được thiết kế chuyên sử dụng trong các môi trường nơi tệp DLL `_overlapped` của trình hệ điều hành Windows bị hỏng.
