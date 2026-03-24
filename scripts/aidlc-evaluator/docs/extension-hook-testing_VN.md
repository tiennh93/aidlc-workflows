# Kiểm thử Điểm nối Extension (Extension Hook Testing)

## Tổng quan

Khung kiểm thử điểm nối extension (extension hook) dùng để xác thực quá trình tải dần các cấu hình extension theo dạng các câu hỏi tùy chọn (opt-in questions) của quy trình làm việc AIDLC. Tính năng này cho phép kiểm thử xem các cấu hình extension khác nhau sẽ tác động như thế nào đến chất lượng và đặc tính của các kết quả được tạo ra.

## Cơ sở (Background)

Kho lưu trữ AIDLC workflows bao gồm tính năng extension hook (tại nhánh: `feat/extension_hook_question_split`) giới thiệu các câu hỏi hỏi ý kiến tùy chọn để cài cắm mở rộng (opt-in questions). Ví dụ:

- **Security Baseline (Cơ sở Bảo mật)**: `security-baseline.opt-in.md` - Extension hướng dẫn rà soát bảo mật.
- **Performance (Hiệu suất năng lực)**: Đường hướng các hướng dẫn tối ưu hiệu năng.
- **Observability (Độ quan sát theo dõi hệ)**: Các mô hình lưu vết log và giám sát mạng.

Mỗi extension có thể được tải dựa trên các câu trả lời do người dùng đồng ý thông qua các hỏi ý kiến "Có/Không", qua đó tạo ra các hướng dẫn mang tính điều hướng cụ thể của AIDLC.

## Kịch bản chạy Test (Extension Test Script)

Kịch bản `run_extension_test.py` tự động hóa việc kiểm thử các cấu hình extension khác nhau bằng cách:

1. Chạy quá trình đánh giá AIDLC nhiều lần với các kịch bản cấu hình opt-in (bật/tắt) khác nhau
2. So sánh kết quả qua các cấu hình hệ
3. Tạo các bản báo cáo cho thấy tác động của các lựa chọn extension

### Cấu hình Mặc định

Cung cấp hai thiết lập chạy cấu hình mặc định (default):

1. **all-extensions (Bật tất cả)**: Tự trả lời lệnh "YES" (Có) cho toàn bộ câu hỏi.
   - Thử nghiệm độ phủ hướng dẫn tối đa của AIDLC khi mà tất cả mở rộng tải vào luồng.
   - Kỳ vọng tạo ra đầu ra bộ mã tài liệu chi tiết và toàn diện hơn.

2. **no-extensions (Không dùng)**: Trả lời đè chữ "NO" (Không) vào hết các extension opt-in.
   - Đánh giá chạy AIDLC trên cấu mốc cơ bản (baseline) tinh giản nhất.
   - Giúp có hệ quy chiếu ban đầu để so sánh mức khác biệt.

## Sử dụng

### Lệnh Cơ bản (Basic Usage)

Chạy đối chiếu tiêu chuẩn (bật tất cả vs. tắt tất cả):

```bash
# Dùng bộ gọi hệ chính run.py (được khuyên dùng)
python run.py ext-test --scenario sci-calc

# Gọi lệnh script tĩnh
python scripts/run_extension_test.py --scenario sci-calc
```

### Lên Dải Cấu Hình Gán Sẵn (List Available Configurations)

```bash
python run.py ext-test --list-configs
```

Kết xuất ra (Output):
```
Available extension test configurations:

  all-extensions        All Extensions Enabled
                        All extension opt-ins answered YES

  no-extensions         No Extensions
                        All extension opt-ins answered NO (baseline only)
```

### Chỉ định Gọi hệ Cấu hình Tùy Chọn

```bash
python run.py ext-test --scenario sci-calc \
    --configs all-extensions,no-extensions
```

### Chèn đổi cành cài luồng luật (Override Rules Branch)

Theo mặc định, mảng kịch bản test bám chót lấy từ luồn `feat/extension_hook_question_split`. Nếu trỏ ngạch sang branch khác:

```bash
python run.py ext-test --scenario sci-calc \
    --rules-ref main
```

### Cấu hình Full chạy Dải hệ

```bash
python run.py ext-test --scenario sci-calc \
    --configs all-extensions,no-extensions \
    --rules-ref feat/extension_hook_question_split \
    --profile my-aws-profile \
    --region us-east-1 \
    --executor-model global.anthropic.claude-opus-4-6-v1 \
    --scorer-model global.anthropic.claude-opus-4-6-v1
```

## Cấu trúc File Nhả Đầu Ra

Sau khi test, một cấu trúc ổ thư file có trật tự được tạo ra:

```
runs/<scenario>/extension-test/
├── 20260309T151234-ext-all-extensions/     # Mảng Run với tất cả bật extensions
│   ├── aidlc-docs/                         # Nhả xả documents 
│   ├── workspace/                          # Đẩy ra mã code sinh thành
│   ├── run-meta.yaml                       # Phễu Metadata
│   ├── extension-test-config.yaml          # File chốt cấu hình extension đã xài cho test
│   ├── test-results.yaml                   # Thông tin luồng hậu kiểm test (post-run)
│   ├── quality-report.yaml                 # Chỉ số mảng cự cài code tĩnh (quality metrics)
│   ├── contract-test-results.yaml          # Test giao ước dạng API
│   ├── qualitative-comparison.yaml         # Lệnh chấm đo điểm ngữ nghĩa
│   └── extension-test.log                  # Ghi nhật ký Run
├── 20260309T153456-ext-no-extensions/      # Lượt test không chứa extensions
│   └── ... (y chang cấu tr trúc trên)
└── extension-comparison/
    ├── extension-test-summary.yaml         # Mảng tóm đối sánh hệ chép file YAML
    └── extension-test-report.md            # Bản rã báo thông dải cho văn báo MD
```

## Báo Cáo Chạy Kiểm Extension

Bộ đai báo dán cài có kèm:

### Tóm gọn Mảng Cấu Hình 

Mục hiển thị các cấu hình hệ đã trải để test qua:
- Tên cấu bộ phân hệ kèm rã mô tả
- Ngạch thông đậu hay trượt (Pass/fail status)
- Mức thời hành (Duration)
- Ổ trích xuất ổ đầu ra Output 

### Chi tiết cách Bóc đối Chiếu (Detailed Comparison)

Mở chỉ cho gõ cài gọi test mảng so đối chạy chéo vạch detailed giữa 2 run:

```bash
python run.py compare --runs-dir runs/<scenario>/extension-test \
    --scenario <scenario>
```

### Định Hướng Rà Lỗi Phân Tích

Mở nhắc cự nhả các ổ vùng chú tâm:
- Chỉ đo tính Qualitative score so sánh chéo.
- Biến động từ lượng artifact tạo ra.
- Tác kích sức vạch đo cho độ code tĩnh quality.
- Các mức % pass rã vạch cho luồn đậu Test Pass
- So đo lượng lố cài hao ngạch Tokens.

## Đọc Diễn Giải Sự Test

### Các Nhịp Thay Đổi Báo (Expected Differences)

Nếu đối đâm 1 nhịp có all "tất bật" vs "không bật tắt hết", sẽ tự vạch đai thấy:

1. **Khía Chất lượng Bộ Code (Code Quality)**
   - All extensions (Bật): Xả lưới kĩ mã thông an ninh bảo mật và lưới màng trích báo errors. 
   - No extensions: Ít đi bọc trích, code cài mộc tinh giản cự. 

2. **Mức rã Độ phủ (Test Coverage)**
   - Bật extensions: Văng rã được khối rẽ vạch test cases cho dày hơn.
   - Thường tắt đo: Tút hệ Test bám theo chuẩn tối cơ bản mộc baseline. 

3. **Mảng cấu Báo Documents**
   - Vạch bọc Extensions: Tỉ mỉ rẽ đánh mạc ngỏ cự về note Security và cự ngầm Performance. 
   - Tắt extensions: Ra lôi bộ docs cốt lõi tinh yếu.

4. **Lượng đo ngầm Token Usage**
   - Vạch bọc All extensions: Thổi gáy lạm trích tiêu số liệu bọc đo Tokens chóp hơn đi kèm tải nạp cho Context trích. 
   - Cho No extensions rã: Vách hao đo trút tokens tiêu nhẹ cực tốt. 

5. **Giải chất đo điểm bằng Qualitative Scores**
   - So vách nảy đồng vạch trích điểm đai Baseline Vàng. 
   - Trích extensions khả đo tự sinh gán đẩy rọi score theo chóp riêng ở mác dimension. 

## Cắm tích màng móc CI/CD

Quá luồn bọc trút Extension ngỏ để gán nảy đo vào dải mỏ liên hành CI:

```yaml
# Mỏ ví dụ rẽ GitLab CI job
extension-test:
  script:
    - python run.py ext-test --scenario sci-calc
  artifacts:
    paths:
      - runs/sci-calc/extension-test/
    expire_in: 1 week
```

## Các Lưu Chú Cài Nhúng Thêm

### Thực Trạng 

Mảng cơ chế extension opt-in opt-in đang luồn vẫn mảng vạch nảy dưới cơ chạy ngầm đục tiếp bọc Dev active development. Kịch bản test xả chứa lệnh đóng trích nhả placeholders (khoảng ngõ giả) đai điều luồng trả nhịp opt-in opt-in. Nếu dải nhả fix màng vách, kịch gọi update trích nhả đai ngỏ các chóp: 

- Gọi Biến rẽ từ môi chốp Environment variables (VD cài: `AIDLC_EXTENSION_OPT_IN=yes|no`)
- Rã đai ngõ trích file Config (VD cài: `aidlc.extension_opt_in_default`)
- Gọi lóng bằng rẽ CLI flags (vd: `--extension-opt-in yes|no|prompt`)
- Cài đai trích bảng phễu File (VD bằng: `--extension-answers answers.yaml`)

### Gói Đệm Mác Chóp Dải (Extension Metadata)

Từng nảy đánh trích cài đai bọc `extension-test-config.yaml` báo mỏ file:
- Cho mảng config test đi cái gán trích nào. 
- Mảng gài Opt-in trích ngỏ bọc cấu vào.
- Chỉ dấu chóp nhánh của branch rules (trích branch/tag/commit).
- Timestamp (Giờ mốc vạch chạy thử đai test).

## Định mốc Báo chóp Mai Sau (Future Enhancements)

Vùng nhả lên plan bục cải chạy rẽ extension testing:

1. **Khóa Tùy Mác Rã Files (Custom Configuration Files)**
   - Giải nảy trích rã nhóm phối hợp kết rẽ arbitrary extensions kết.
   - Cắm đai mỏ vạch báo định YAML-based YAML. 

2. **So đối mác Cắm điểm rẽ Extension (Extension-Specific Comparisons)**
   - Rã Test các chóp của các extension đơn vạch (individual).
   - Đánh đai móc Increment nhả của sức rẽ nảy bọc tác trích từ mỗi con extension. 

3. **Gán tự đai Test Báo Nảy Nhả (Regression Detection)**
   - Quăng nảy cờ gán Flag lúc bục lỗi nảy quality đi thoái dạt nhả bục degrade.
   - Định dò vết trích impact extension theo mốc time báo rẽ. 

4. **Trích rã Ma trận Vách Test Mã Máy Matrix**
   - Thử rẽ cài trích cựa nhả các gộp tổ bộ extensions N 
   - Đai cài sinh Report mạng nảy chéo ngỏ Matrices 

## Mốc File Trích Cấu Liên Tham (References)

- [Trích Nhánh rẽ Branch chứa luồn Rule Feature của Extension Hook](https://github.com/awslabs/aidlc-workflows/tree/feat/extension_hook_question_split)
- [Cấu mẫu gài ví Security Baseline Opt-in](https://github.com/awslabs/aidlc-workflows/blob/feat/extension_hook_question_split/aidlc-rules/aws-aidlc-rule-details/extensions/security/baseline/security-baseline.opt-in.md)
- [Ổ khóa chính của Kho AIDLC Workflows Repository](https://github.com/awslabs/aidlc-workflows)

## Gọi trích hỗ Cấu (Support)

Móc khi mắc cặn bọc hỏi hoặc sự cố (issues):

1. Check ở gán log của bộ Extension tại thư folder (output directory)
2. Bọc móc đai cái file cài `extension-test-config.yaml` coi bục đai config.
3. Rã đai ở xả file `extension-test-report.md` trút nhả so sánh. 
4. Gán trích thả phễu gọi issue mở mỏ thư mục aidlc-regression repository.
