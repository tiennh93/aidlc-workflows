# Các Script Đánh Giá AIDLC (Evaluation Scripts)

Thư mục này chứa các script chuyên biệt để chạy hệ thống khung đánh giá AIDLC (AIDLC evaluation framework).

## Tổng Quan (Overview)

Tất cả các tệp lệnh chạy (run scripts) đã được gom tập trung vào thư mục `scripts/` này nhằm mục đích tổ chức gọn gàng hơn. Điểm nghẽn cốt lõi (main entry point) để khởi chạy hiện tại là file `run.py` nằm ở thư mục gốc của kho lưu trữ (repository root). Nó sẽ làm nhiệm vụ điều phối và dẫn luồng lệnh tới các script chuyên biệt này tùy thuộc vào chế độ đánh giá (evaluation mode) được chọn.

## Các Script Hành Động

### Bộ Kịch Bản Đánh Giá Lõi (Core Evaluation Scripts)

- **`run_evaluation.py`** - Luồng đánh giá toàn diện (full evaluation pipeline) (thực thi cả luồng AIDLC + chấm điểm đầu ra).
  - Đóng vai trò nhạc trưởng điều phối toàn bộ 6 giai đoạn (stages): thực thi mã (execution), kiểm thử bộ mã sau khi chạy (post-run tests), phân tích định lượng (quantitative analysis), kiểm thử hợp đồng (contract tests), đánh giá định tính (qualitative evaluation), và báo cáo (reporting).
  - Có thể chạy ở chế độ kiểm thử nhanh bằng cờ (flag) `--test`.

- **`run_cli_evaluation.py`** - Chỉ chạy đánh giá dựa trên dạng giao diện dòng lệnh CLI.
  - Chạy đánh giá qua các trợ lý AI CLI như (kiro-cli, claude-code,...).
  - Tích hợp sử dụng các bộ chuyển đổi (adapters) lấy từ gói `packages/cli-harness`.

- **`run_ide_evaluation.py`** - Chạy đánh giá dựa trên hệ môi trường IDE (IDE-based evaluation).
  - Khởi động kiểm thử thông qua các trợ lý AI cài vào IDE (cursor, cline, kiro).
  - Sử dụng các adapters trích xuất từ nhóm gói `packages/ide-harness`.

### Chạy Kịch Bản Xử Lý Hàng Loạt (Batch Processing Scripts)

- **`run_batch_evaluation.py`** - Trình thu thập đánh giá hàng loạt dạng mẻ lớn.
  - Khởi tạo quá trình đánh giá luồng AIDLC chạy tuần tự qua nhiều loại mô hình Bedrock sinh mã khác nhau.
  - Tự tải lên tập cấu hình của các model chứa ở thư mục `config/`.
  - Có vai trò ủy quyền tiếp tục vách chạy cho `run_evaluation.py` trên từng mô hình LLM lẻ tẻ.

- **`run_comparison_report.py`** - Xuất bản báo cáo đối chiếu chéo (Cross-model comparison).
  - Dùng để thu gom và tổng hợp tất cả kết quả nhả ra từ các đợt đánh gộp mẻ batch.
  - Nảy sinh tài liệu bảng đo (comparison matrices) kết tinh dưới hai định dạng Markdown và YAML.
  - Trực tiếp đối chiếu chéo với mốc dữ liệu tham chiếu nền vàng chuẩn (golden baseline).

- **`run_extension_test.py`** - Đánh giá móc nhánh cài tiện ích mở rộng (Extension hook testing).
  - Tổ chức chạy kiểm thử mô hình AIDLC đánh giá dưới sự lựa chọn opt-in điểm cài tiện ích mở rộng khác nhau.
  - Phân vùng mảng chạy test giữa hai trường phái câu trả lời opt-in: "chấp nhận tất cả (all yes)" vs "từ chối tất cả (all no)".
  - Chốt hạ sinh ra bản tóm tắt đối chiếu hiển thị mức độ tác động (impact) do những lựa chọn extension này gây rã.
  - Bắt buộc phải móc từ nhánh mở rộng (feature branch: `feat/extension_hook_question_split`).

### Báo Cáo Xu Hướng Hoạt Động (Trend Reporting)

- **`run_trend_report.py`** - Render báo cáo xu hướng chép dịch qua trích đoạn phiên bản Release.
  - Công cụ thu trích (fetch) các gói gói (bundles) đánh giá cài từ nhịp kho Release của GitHub và của mảng Actions artifacts.
  - Trích xuất ra bảng báo biểu đồ xu hướng định dạng HTML, Markdown, và YAML có gán đối sánh chéo các chỉ số giữa các Release đo nhau.
  - Code chạy gói công cụ `packages/trend-reports` có sẵn.
  - Ở mảng Executive summary cards (thẻ tổng đồ tóm tắt) sẽ in hiển thị: Điểm Định Tính (Qualitative Score), Contract Tests, Tỉ lệ mảng pass nhả cho Unit Test (%), Số lượng lỗi bắt (Lint Findings), Thời Gian Dạo Lệnh (Execution Time), và Tổng Kéo Tokens Sinh Lọc (Total Tokens).
  - Dải biến `Execution Time` (Thời Gian) và thẻ `Total Tokens` là kiểu hệ đo "càng thấp càng ngon (lower is better)" (được lên đèn tô xanh ngầm định thông báo vì chóp này mang giá trị desirable được mong đợi đoạt).

## Hướng Dẫn Sử Dụng (Usage)

### Sử Dụng Cổng Lệnh Chung Bọc Ngoài Nhất (Khuyến Nghị - Recommended)

Cách tối ưu nhất mà nhà phát triển khuyến nghị khi muốn khởi xướng chạy là thao tác đi qua bản lệnh Master chóp của hệ thống là `run.py` ngự tại nơi gốc repo của dự án:

```bash
# Phễu Luồng Đánh Giá Toàn Trình (Full pipeline)
python run.py full --vision test_cases/sci-calc/vision.md

# Rã Test hệ CLI
python run.py cli --cli kiro-cli --scenario sci-calc

# Bộ Móc Hệ Đánh IDE 
python run.py ide --ide cursor --scenario sci-calc

# Thu Gom Điểm Đánh Batch Gộp Giữa Nắm Chóp Models
python run.py batch --models all --scenario sci-calc

# Lệnh Ép Rã Chóp Bảng Compare Report (Bảng Đối Chứng)
python run.py compare --scenario sci-calc

# Chạy Rã Tiện Móc Extension hooks (nhánh so all yes vs all no)
python run.py ext-test --scenario sci-calc

# Giải rẽ Ghi Dấu Tuyến Trend Báo Cáo Đo Cross Phát Hành Releases
python run.py trend --baseline test_cases/sci-calc/golden.yaml

# Khơi Phễu Lệnh Chạy Tập Thiết Mạng Test Tests
python run.py test
```

### Triệu Gọi Trực Tiếp Từng File Hạt (Direct Script Invocation)

Tùy vào nhu cầu cá biệt, bạn vẫn có thể nảy lệnh xé lẻ chạy riêng lẻ từng script:

```bash
# Đai Test Full nảy Trình
python scripts/run_evaluation.py --vision test_cases/sci-calc/vision.md

# Mỏ Test đai theo nảy chuẩn túi lệnh máy CLI 
python scripts/run_cli_evaluation.py --cli kiro-cli --scenario sci-calc

# Quét Gom test mẻ Batch hàng loạt 
python scripts/run_batch_evaluation.py --models all --scenario sci-calc

# Phễu test túi Extension Hook 
python scripts/run_extension_test.py --scenario sci-calc

# Trút hệ cựa Báo Móc Xu Trend Report 
python scripts/run_trend_report.py --baseline test_cases/sci-calc/golden.yaml
```

## Chống Lạc Cành Đường Dẫn (Path Resolution)

Toàn dải scripts đã tinh chỉnh cho khả năng tự trỏ (resolve) mọi ngỏ path đường dẫn về điểm chuẩn quy chiếu nằm ở cội nguồn kho git repo gốc (repository root). Thế nên vạch mỏ có thể chạy trơn tru mượt bọc cho dù bị lôi đầu bấm chạy từ bất kỳ cách xả nào:
- Phát cựa qua lệnh rẽ nhánh phễu `run.py` Master Dispatcher.
- Trực rã Móc từ gốc rã repository root.
- Trút đo trực rã bằng đường mạc ở túi mảng `scripts/` directory.

## Quản Kỷ Kiến Trúc Code Hệ (Architecture Notes)

- Điểm **REPO_ROOT**: All ngỏ các mảng script dùng dải `Path(__file__).resolve().parent.parent` để nảy dò vị rẽ gốc kho chứa Repo root.
- Cửa **Output**: Thành đo File sau chạy được bọc rẽ xả dải tới túi `runs/<scenario>/` nếu dùng mặc đai cựa.
- Thiết mảng **Config**: Khóa Rã Cài Đặt (Configuration files) được đọc phễu từ kho mảng `config/` tự bọc ngõ ở nhánh repo root đâm.
- Bộ Trút Ngỏ **Test Cases**: Bộ nhả Mác Scenario (kịch bản test cases run) ngự mác cựa rẽ ở bọc `test_cases/` ở thư nháp chóp repo root đai vách.
