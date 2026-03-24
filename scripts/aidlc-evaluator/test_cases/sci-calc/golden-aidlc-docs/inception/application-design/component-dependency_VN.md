# Phụ Thuộc Của Các Thành Phần (Component Dependencies)

## Ma Trận Phụ Thuộc (Dependency Matrix)

| Thành Phần (Component) | Phụ Thuộc Vào (Depends On) | Được Phụ Thuộc Bởi (Depended On By) |
|---|---|---|
| `app.py` | Tất cả các routes, models phản hồi | — (điểm bắt đầu / entry point) |
| `routes/arithmetic.py` | `models/requests.py`, `models/responses.py`, `engine/math_engine.py` | `app.py` |
| `routes/powers.py` | `models/requests.py`, `models/responses.py`, `engine/math_engine.py` | `app.py` |
| `routes/trigonometry.py` | `models/requests.py`, `models/responses.py`, `engine/math_engine.py` | `app.py` |
| `routes/logarithmic.py` | `models/requests.py`, `models/responses.py`, `engine/math_engine.py` | `app.py` |
| `routes/statistics.py` | `models/requests.py`, `models/responses.py`, `engine/math_engine.py` | `app.py` |
| `routes/constants.py` | `models/responses.py`, `engine/math_engine.py` | `app.py` |
| `routes/conversions.py` | `models/requests.py`, `models/responses.py`, `engine/math_engine.py` | `app.py` |
| `models/requests.py` | — (các Pydantic models độc lập) | Tất cả các routes |
| `models/responses.py` | — (các Pydantic models độc lập) | Tất cả các routes, `app.py` |
| `engine/math_engine.py` | Thư viện chuẩn `math`, `statistics` của Python | Tất cả các routes |

## Luồng Dữ Liệu (Data Flow)

```
Yêu cầu HTTP (HTTP Request)
    |
    v
app.py (FastAPI) --> trình xử lý định tuyến (xác thực thông qua Pydantic model)
    |                    |
    |                    v
    |               math_engine.py (tính toán thuần túy)
    |                    |
    |                    v
    |               kết quả hoặc ngoại lệ (result or exception)
    |                    |
    v                    v
SuccessResponse hoặc ErrorResponse (Pydantic model)
    |
    v
Phản hồi HTTP (HTTP Response) (định dạng JSON)
```

## Mẫu Giao Tiếp (Communication Pattern)
- **Các hàm gọi đồng bộ (Synchronous function calls)** — không cần lệnh gọi engine bất đồng bộ (các phép toán thao tác với CPU chạy rất nhanh)
- **Không có hàng đợi tin nhắn (message queues), sự kiện, hoặc các dịch vụ bên ngoài**
- **Không có kết nối cơ sở dữ liệu**
- **Báo lỗi dựa trên ngoại lệ (Exception-based error signaling)** từ khối engine qua các routes

## Các Quyết Định Thiết Kế Chính
1. Engine là một **mô-đun thuần túy** chứa các hàm độc lập — không khởi tạo lớp đối tượng, không lưu giữ trạng thái (state)
2. Các tập định tuyến (routes) **trực tiếp import** các hàm engine — không cần cơ chế dependency injection (tiêm phụ thuộc) cho phạm vi chức năng này
3. Các ngoại lệ tùy chỉnh (`DomainError`, `DivisionByZeroError`) được định nghĩa bên trong mô-đun lớp engine
4. Khối engine mang đặc tính **không phụ thuộc vào HTTP/framework (zero HTTP/framework dependencies)** — có thể kiểm thử hoàn toàn độc lập độc lập
