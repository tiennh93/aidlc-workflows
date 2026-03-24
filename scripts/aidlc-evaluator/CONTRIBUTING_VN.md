# Đóng góp cho Framework Đánh giá AI-DLC

Cảm ơn bạn đã đóng góp cho framework đánh giá và báo cáo quy trình làm việc AI-DLC!

## Bắt đầu

### Điều kiện Tiên quyết

- Python 3.13+
- Trình quản lý gói [uv](https://github.com/astral-sh/uv)
- Git

### Thiết lập

```bash
# Clone kho lưu trữ
git clone <repository-url>
cd aidlc-evaluation-framework

# Cài đặt các phần phụ thuộc
uv sync

# Chạy các bài kiểm thử để xác minh thiết lập
uv run pytest
```

## Quy trình làm việc Phát triển

### 1. Tạo một Nhánh

```bash
git checkout -b feature/your-feature-name
```

### 2. Thực hiện Thay đổi

Làm việc trong gói (package) phù hợp:
- `aidlc-runner/` - Framework Thực thi (trình chạy quy trình làm việc AI-DLC hai agent)
- `packages/qualitative/` - Đánh giá Định tính (chấm điểm sự tương đồng về chức năng cốt lõi (intent) & thiết kế)
- `packages/quantitative/` - Đánh giá Mã nguồn (lint, bảo mật, tổ chức mã)
- `packages/nonfunctional/` - Đánh giá NFR (số liệu token, thời gian, tính nhất quán)
- `packages/reporting/` - Trình tạo báo cáo
- `packages/shared/` - Các tiện ích dùng chung

Hoặc đóng góp vào các luồng công việc khác:
- `test_cases/` - Các Trình mô phỏng Test case Vàng (đầu vào cơ sở)
- `writing-inputs/` - Hướng dẫn biểu diễn tầm nhìn (vision) và môi trường công nghệ (tech-env) 
- `.github/workflows/` - Quản lý & Tích hợp liên tục thông qua CI/CD của GitHub

### 3. Chạy Kiểm Thử

```bash
# Chạy tất cả kiểm thử
uv run pytest

# Chạy kiểm thử của một gói cụ thể
uv run pytest tests/test_qualitative.py

# Chạy kiểm thử cùng với tỷ lệ bao phủ code (coverage)
uv run pytest --cov
```

### 4. Kiểm tra (Lint) Mã Nguồn của bạn

```bash
# Kiểm tra văn phong lập trình
uv run ruff check .

# Tự động sửa lỗi
uv run ruff check --fix .

# Định dạng code
uv run ruff format .
```

### 5. Cam kết (Commit) Sự Thay đổi

Viết các thông báo commit rõ ràng, mang tính mô tả:

```bash
git add .
git commit -m "Add token tracking to nonfunctional package"
```

### 6. Gửi Yêu cầu Cập nhật Mã Nguồn (Pull Request)

- Đẩy (push) nhánh của bạn lên kho lưu trữ
- Mở một pull request (PR) với mô tả rõ ràng về các thay đổi
- Cung cấp liên kết đính kèm các số liệu theo dõi Issue có liên quan
- Chờ các quy trình kiểm thử tự động chạy qua và phản ánh xanh (pass)
- Xử lý các phản hồi đánh giá (review) từ hệ thống và nhóm

## Luồng công việc (Work Streams)

Dự án được cấu trúc xoay quanh sáu nền tảng trọng tâm (big rocks). Ở mỗi đề xuất mà bạn muốn thay đổi lên chúng tôi thường nằm trong số những nền tảng này:

| Luồng công việc | Tóm tắt | Gói / Phạm vi |
|---|---|---|
| **Golden Test Case** | Điểm thử cơ sở đã được giám sát | `test_cases/` |
| **Execution Framework** |  Hệ Trình biên tập Chạy workflow AI-DLC (Chủ phân hệ: Jeff)| `aidlc-runner/` |
| **Semantic Evaluation** | Trình đánh giá sự tương quan đồng nghĩa của tính năng thiết kế và mục đích (ý đồ)| `packages/qualitative/` |
| **Code Evaluation** | Điểm số linting, quét mã hóa bảo an, bố trí code | `packages/quantitative/` |
| **NFR Evaluation** | Tiêu hao tokens số, hiệu ứng đóng khung thời gian tính, biên độ mức tính ổn định dữ liệu  | `packages/nonfunctional/` |
| **GitHub CI/CD** | Mạng kết phân dòng tích hợp và vận hành quy củ | `.github/workflows/` |

## Tiêu chuẩn Mã Nguồn (Code Standards)

### Trình bày Python

- Bám theo chuỗi PEP 8 (Được xác minh kỹ bởi Ruff)
- Vận dụng linh hoạt biểu thị tính kiểu dòng (type hints)
- Dài tối thiểu trên dòng là: 100 character
- Quy phạn chuẩn comment qua (docstrings) dùng chung ở dạng Class hoặc Hàm dùng hệ cộng đồng.

### Chuẩn hóa kiểm thử

- Làm Test case chuẩn cho các nhóm luồng quy tính mới thiết trí
- Bảo hộ và củng cố độ che phủ bộ mã test
- Viết định hướng test miêu tả tên mở: `test_<what>_<condition>_<expected>`

### Về thông tin Tài Liệu

- Làm mới tệp thông hành chuẩn `README.md` lúc cần giải nạp thêm 1 bản feature(tính năng) vào.
- Làm mới lại câu lệnh giải chuẩn `docstrings` để có file dẫn cho bộ models/function bổ sung
- Update lưu ý các ghi chú tài liệu tương hợp quanh thư mục `docs/`

## Ràng buộc về các Gói (Package Dependencies)

Sự việc thay hoặc bổ xung cài thêm mới về Dependence trên gói:

1. Đặt thông trên bản dạng config Python ở `pyproject.toml` định lưu trong khu hệ `packages/<package>/` hay là tại nhánh mã mẹ `aidlc-runner/`
2. Kéo luồng chạy lệnh `uv sync` để tiến hành đổi làm mới khoá tập tin cài lock file
3. Ghi tài liệu lại giải phẫu vì sao đòi thiết phải cần dependency loại này trong cái file ghi nội báo cáo kéo Push code ở dòng PR của bạn.

## Khai báo Vấn đề (Issues)

Bạn hãy thông qua cách tiếp sau lúc tìm thấy báo cáo dòng gây sập lân cận hay mở ngỏ một yêu cầu tính năng mở:

- Có sử dụng GitHub Issues
- Chỉ dẫn bước làm rõ nét cho cách để mô phỏng tái tạo phát hiện điểm sai.
- Đóng thông lại cho các nhánh lỗi dính lân trong file giải dạng log hoặc mã câu cảnh báo trên dòng thông.
- Khoan định vùng cự ly sai ở module (package) chạy mã mà đã được cấu trong phạm.

## Câu hỏi? (Questions)

- Coi ngó tại tài liệu hỏi đáp nhanh [FAQ.md](./FAQ_VN.md) lúc bí
- Tham khảo bảng hướng tiêu chuẩn định phận quản theo kiểu giải pháp chung ở bản: [OPERATING_PRINCIPLES.md](./OPERATING_PRINCIPLES.md) (nếu có bản _VN hãy xem bản _VN) 
- Bạn được hoan hỉ chào mời điền hỏi tư vấn trên phần bình phẩm ở PR review hay có mở thông bài chia sẻ mới.

## Quy tắc Ứng xử (Code of Conduct)

- Xin có ý kiêng nể tương hữu và gây xây dựng.
- Có ý chung với tập luồng vào phân khu mã code, khoan soi trích người thực thi tạo.
- Hoan hiệp và mở chào tất cả dải tầng phân góc khác với định.
- Biết chia san để hỗ và bổ tiến những người bạn ngoài hệ vòng cùng nhau thăng.

Chân thành Cảm ơn mọi người trong cuộc hoàn trả tốt hơn một AI-DLC theo thông mác giải mã framework!
