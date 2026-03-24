# Hướng Dẫn Xây Dựng (Build Instructions)

## Yêu Cầu Tiên Quyết (Prerequisites)
- **Công Cụ Xây Dựng (Build Tool)**: `hatchling` (Giao diện backend phục vụ build PEP 517)
- **Môi Trường Thời Gian Chạy (Runtime)**: Python 3.13+
- **Trình Quản Lý Gói (Package Manager)**: `uv` (khuyên dùng) hoặc `pip`
- **Các Gói Phụ Thuộc (Dependencies)**:
  - `fastapi>=0.115.0`
  - `uvicorn[standard]>=0.34.0`
- **Gói Phụ Thuộc Dành Cho Phát Triển (Dev Dependencies)**:
  - `httpx>=0.28.0`
  - `pytest>=8.3.0`
  - `pytest-asyncio>=0.25.0`
  - `pytest-cov>=6.0.0`
  - `ruff>=0.9.0`

## Các Bước Xây Dựng (Build Steps)

### 1. Cài Đặt Các Gói Phụ Thuộc
```bash
# Sử dụng uv (được khuyên dùng)
uv sync --all-extras

# Hoặc sử dụng pip
pip install -e ".[dev]"
```

### 2. Cấu Hình Môi Trường
```bash
# Đặt PYTHONPATH nếu chạy từ source mà không qua cài đặt
export PYTHONPATH=src    # Dành cho Linux/macOS
set PYTHONPATH=src       # Dành cho Windows
```

### 3. Xây Dựng Gói (Build the Package)
```bash
# Xây dựng file kiểu wheel và sdist
uv build
# Hoặc: python -m build
```

### 4. Xác Minh Sự Thành Công Của Quá Trình Build
- **Kết Quả Đầu Ra Mong Đợi (Expected Output)**: Tệp `dist/sci_calc-0.1.0-py3-none-any.whl`
- **Cấu Trúc Gói**: Thư mục `src/sci_calc/` chứa các gói con engine, models, routes
- **Điểm Bắt Đầu (Entry Point)**: `sci_calc.app:app` (ứng dụng ASGI)

## Chạy Máy Chủ (Run the Server)
```bash
# Máy chủ phát triển (Development server)
uvicorn sci_calc.app:app --reload --host 0.0.0.0 --port 8000
```

## Khắc Phục Sự Cố (Troubleshooting)

### Việc Build thất bại kèm theo Lỗi phụ thuộc
- **Nguyên nhân**: Mạng không khả dụng hoặc không thể kết nối tới PyPI
- **Giải pháp**: Sử dụng cờ `--find-links` với cache gói cục bộ hoặc cài đặt trước các phụ thuộc

### Gặp Lỗi Import (site-packages bị cũ)
- **Nguyên nhân**: Phiên bản cũ của sci-calc đã được cài đặt ở cấp toàn cầu (globally)
- **Giải pháp**: Đặt `PYTHONPATH=src` hoặc sử dụng `pip install -e .` để ghi đè

### asyncio bị lỗi trên hệ điều hành Windows (WinError 10106)
- **Nguyên nhân**: Kiến trúc hỗ trợ Windows Winsock (`_overlapped` DLL) đã bị hỏng
- **Giải pháp**: Chạy `netsh winsock reset` bằng quyền admin, hoặc chuyển qua WSL2
- **Giải pháp tạm thời (Workaround)**: Sử dụng thêm các cờ pytest chèn thêm `-p no:anyio -p no:asyncio` và sử dụng máy khách kiểm thử đồng bộ (sync test client)
