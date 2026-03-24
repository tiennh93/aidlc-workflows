# Môi Trường Kỹ Thuật: API Máy Tính Bỏ Túi Khoa Học

## Ngôn Ngữ và Trình Quản Lý Gói

- **Python 3.13**
- **uv** cho toàn bộ việc quản lý gói (không dùng pip, poetry, hay conda)
- `pyproject.toml` để cấu hình dự án và công cụ

## Web Framework

- **FastAPI** với Pydantic v2 để xác thực request/response
- **uvicorn** làm máy chủ ASGI

## Cấu Trúc Dự Án

```
sci-calc/
├── pyproject.toml
├── src/
│   └── sci_calc/
│       ├── __init__.py
│       ├── app.py
│       ├── routes/
│       │   ├── __init__.py
│       │   ├── arithmetic.py
│       │   ├── trigonometry.py
│       │   ├── logarithmic.py
│       │   ├── powers.py
│       │   ├── statistics.py
│       │   ├── constants.py
│       │   └── conversions.py
│       ├── models/
│       │   ├── __init__.py
│       │   ├── requests.py
│       │   └── responses.py
│       └── engine/
│           ├── __init__.py
│           └── math_engine.py
└── tests/
    ├── __init__.py
    ├── conftest.py
    ├── test_arithmetic.py
    ├── test_trigonometry.py
    ├── test_logarithmic.py
    ├── test_powers.py
    ├── test_statistics.py
    ├── test_constants.py
    └── test_conversions.py
```

## Kiểm Thử

- **pytest** với pytest-asyncio và httpx (test client bất đồng bộ)
- **pytest-cov** với độ bao phủ dòng tối thiểu 90%
- Unit test kiểm thử trực tiếp `math_engine.py` với các bảng giá trị đã biết
- Integration test sử dụng `httpx.AsyncClient` với FastAPI TestClient
- Boundary test xác minh mọi ràng buộc miền đều tạo ra mã lỗi chính xác
- Lệnh chạy: `uv run pytest`

## Linter và Format

- **ruff** (chiều dài dòng 100, mục tiêu py313)

## Build Backend

- **hatchling**

## KHÔNG Sử Dụng

| Bị Cấm | Lý Do | Thay Thế Bằng |
|-----------|--------|-------------|
| Flask, Django | Dự án dùng FastAPI | FastAPI |
| requests | Chặn async event loop | httpx |
| sympy | Quá nặng cho phạm vi này | Python `math` stdlib |
| pandas, numpy | Không cần thiết cho các phép tính đơn lẻ | Standard Python |
| pip, poetry, pipenv | Dự án dùng riêng uv | uv |
| black, flake8, isort | Đã bị thay thế bởi ruff | ruff |

## Yêu Cầu Phi Chức Năng

| Yêu Cầu | Mục Tiêu |
|---|---|
| Thời gian khởi động | < 2 giây |
| Độ trễ phản hồi (p95) | < 50ms cho bất kỳ phép tính đơn lẻ nào |
| Độ bao phủ kiểm thử | >= 90% dòng lệnh |
| Đồng nhất dấu phẩy động | Kết quả khớp với Python `math` stdlib <= 1 ULP |
| Kích thước thân request max | 1 MB |
| Phiên bản Python | 3.13.x (bắt buộc qua `requires-python = ">=3.13"`) |

## Quy Trình Phát Triển

```bash
uv sync
uv run uvicorn sci_calc.app:app --reload --port 8000
uv run pytest
uv run ruff check .
uv run ruff format .
```
