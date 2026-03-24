# Bài kiểm thử (Test Case): all-stages

## Mục đích

Ra lệnh ép buộc luồng AI-DLC (AIDLC workflow) phải kích hoạt và thực thi **tất cả mọi giai đoạn có điều kiện (conditional stage)** thay vì được phép bỏ qua bất kỳ bước nào. Test case về máy tính khoa học (sci-calc) trước đó được cố tình thiết kế rất đơn giản (chỉ có một unit duy nhất, không dùng user stories, không có thiết kế NFR design, không thiết kế hạ tầng infrastructure). Trong khi đó, bộ bài kiểm thử `all-stages` này được thiết kế theo một cách để mô hình AI không thể nào kiếm cớ tìm cách bỏ qua (skip) một thứ gì.

## Các Giai Đoạn Kích Hoạt (Stages Triggered)

| Giai Đoạn (Stage) | Cơ Chế Kích Hoạt Mở (Trigger Mechanism) |
|---|---|
| Workspace Detection (Nhận diện Môi trường) | Luôn kích hoạt chạy |
| Reverse Engineering (Kỹ thuật Đảo ngược) | **Không kích hoạt** -- (Bắt buộc phải là dự án mã nguồn cũ - brownfield mới gọi; xem ghi chú bên dưới) |
| Requirements Analysis (Phân tích Yêu cầu) | Luôn chạy |
| User Stories (Câu chuyện Người dùng) | Dự án có 3 nhóm đặc tính tính cách người dùng (personas) dẫn đến luồng quy trình (workflows) khác nhau |
| Workflow Planning (Kế hoạch Luồng làm) | Luôn chạy |
| Application Design (Thiết kế Ứng dụng) | Do có các components mới, service mới, và các bộ luật business rules |
| Units Generation (Tạo Unit khối làm việc) | Gồm 2 phân hệ (modules) bắt buộc phải bóc tách (decomposition) |
| Functional Design (Thiết kế Chức năng - per unit) | Luật mô hình (business rules) khá phức tạp (VD: giới hạn cho mượn, phí phạt, trạng thái giữ sách) |
| NFR Requirements (Yêu cầu Phi chức năng - per unit) | Bắt tính hệ các độ trễ SLAs hiệu năng, tính bảo mật, tính năng scale (scalability) explicit rõ ràng |
| NFR Design (Thiết kế NFR - per unit) | Yêu cầu phải có các bản thiết kế pattern về tính bền chịu lỗi (Resilience), bộ đệm (caching), cắt hạn lưu lượng (rate limiting) |
| Infrastructure Design (Thiết kế Hạ tầng - per unit) | Có dính định mảng các dịch vụ AWS bị giới hạn cụ thể (như Lambda, DynamoDB, Cognito, SQS) |
| Code Generation (Sinh Mã) | Luôn chạy |
| Build and Test (Gói Build & Kiểm Thử) | Luôn chạy |

## Các Tệp Đầu Vào (Files)

| Tệp | Mô tả |
|---|---|
| `vision.md` | Tài liệu nền dự án (Vision) -- Nền tảng Thư viện API BookShelf (BookShelf community library API) |
| `tech-env.md` | Môi trường kỹ thuật (Tech env) -- Bằng Python/FastAPI, đẩy trên AWS serverless |
| `openapi.yaml` | Bản thiết kế cấu trúc API contract có gắn `x-test-cases` dành cho việc chạy post đánh Contract API |
| `golden-aidlc-docs/` | Tài liệu tham chiếu gốc (điểm vàng - Golden reference) (được sinh ra sau lần chạy thành công đầu tiên) |
| `golden.yaml` | Số liệu đo (baseline metrics) dùng làm mốc quy chiếu của bản golden run (được ghi nhận sau khi gán Promote) |

---

## Cài Chạy Bằng lệnh  `run_evaluation.py`

Lệnh `run_evaluation.py` sẽ cầm trịch chạy gọi thảy qua hết 6 bước (six-stage) của ống pipeline. Do thiết lập đường dẫn ngầm mặc định ban đầu đều trỏ về hướng của bài test `test_cases/sci-calc/`, bạn bắt buộc phải tùy chỉnh ghi đè khai báo các đường path mồi vào mới có thể cắm cài bài test này.

### Lần chạy khởi thủy (Lúc chưa trích bộ Điểm Vàng)

Ở kỳ chạy "chập chững" lần đầu, hiển nhiên sẽ bóc thiếu thư mục mốc vàng `golden-aidlc-docs/` để so đo định lượng ngữ nghĩa. Bạn cứ cài gán cho luồng execution mảng chạy mở bình thường, việc không chấm qualitative comparison bị bỏ qua cứ kệ, và xuất File nhả bằng tay manual coi.

```bash
# Nhặt một model có sẵn từ kho config/ (VD kiểu: opus.yaml, sonnet-4-5.yaml)
python run_evaluation.py \
  --config  config/opus.yaml \
  --vision  test_cases/all-stages/vision.md \
  --tech-env test_cases/all-stages/tech-env.md \
  --openapi test_cases/all-stages/openapi.yaml \
  --golden  test_cases/all-stages/golden-aidlc-docs
```

Đường dẫn cắm vào thông báo cành `--golden` chưa hiện diện đâu, vì thế cái mốc test Qualitative kia sẽ hỏng trượt (fail). Việc trượt đó nằm trong dự tính cả. Còn thư ổ kết qua Output của test thì vẫn bọc đính sinh các nhánh như:

```
runs/<timestamp>/
  aidlc-docs/          # Hệ chữ Documents chạy xả báo 
  workspace/           # Dải hệ source app đục mã sinh Code
  test-results.yaml    # Bọc nhả test unit sau hậu kỳ 
  run-meta.yaml        # Giấy nháp info cài rã ghi vết bọc 
```

### Thăng đai cho bản chạy ốp thành Mốc Vàng (Promoting a run to golden)

Một lần bọc kiểm ra đai nhả xem thông mà lọt trơn tru (outputs look correct), chóp cài đi qua bọc:

```bash
# Dán bản aidlc-docs từ dải thư chứa run nạp qua làm hòm Điểm Vàng 
cp -r runs/<timestamp>/aidlc-docs test_cases/all-stages/golden-aidlc-docs
```

Để nhả đâm một mốc đai YAML mác chuẩn là `golden.yaml` chứa cho hồi quy, chóp cài móc key metrics theo chạy đai (tham khảo: `test_cases/sci-calc/golden.yaml` nhả dải format mốc).

### Các lượt chạy đánh vòng sau (Khi đã vách nảy Điểm Baseline)

Bất kỳ khi nao mà túi thư `golden-aidlc-docs/` đó ra hình rồi, thì toàn luồng 6 mảng sẽ chạy luồn end-to-end tuốt:

```bash
python run_evaluation.py \
  --config   config/opus.yaml \
  --vision   test_cases/all-stages/vision.md \
  --tech-env test_cases/all-stages/tech-env.md \
  --openapi  test_cases/all-stages/openapi.yaml \
  --golden   test_cases/all-stages/golden-aidlc-docs
```

Biến lệnh `--baseline` ngầm tự luồn dò mỏ tìm chóp bản `golden.yaml` nằm trích bên trong túi có cái nhánh `--golden`, vì vậy lỡ rã có file `test_cases/all-stages/golden.yaml` hiện diện mộc, luồn máy test nó sẽ múa chạy móc luôn cho cái vách hồi quy comparison regression gán đai.

### Chấm Nhịp Evaluate lên 1 mỏ thư chạy cũ có sẵn (evaluate-only)

Nếu gõ cựa bọc để chấm rẽ (re-score) 1 đai báo file mà ko thiết vách đâm xả bằng AI agent đai: 

```bash
python run_evaluation.py \
  --evaluate-only runs/<timestamp>/aidlc-docs \
  --golden  test_cases/all-stages/golden-aidlc-docs \
  --openapi test_cases/all-stages/openapi.yaml
```

### Bảng Dải Lệnh Đai Các Mỏ Khóa CLI quan trích (Key CLI flags)

| Cờ Lệnh | Tác Dụng Mảng (Purpose)| Trích Mặc Định (Default) |
|---|---|---|
| `--config` | Xác mác file YAML nhả (lấy thông aws creds, executor model, cài vòng swarm settings) | `config/default.yaml` |
| `--vision` | Bản file trích rẽ chữ Vision | `test_cases/sci-calc/vision.md` |
| `--tech-env` | Cắm Tech-env document thư | `test_cases/sci-calc/tech-env.md` |
| `--openapi` | Trích cấu nạp API spec cho OpenAPI cài đai `x-test-cases` | `test_cases/sci-calc/openapi.yaml` |
| `--golden` | Tham chiếu Golden Baseline nhả thư cài test điểm chấm | `test_cases/sci-calc/golden-aidlc-docs` |
| `--baseline` | `golden.yaml` đo điểm vách chéo đai hồi quy regression | Rẽ nạp auto kiếm (Auto-discovered) móc trong `--golden` ổ chứa |
| `--executor-model` | Móc nạp thế gán chóp cựa mã ID từ bộ YAML | Lấy thông từ YAML |
| `--scorer-model` | Bedrock Model máy cho vạch đo trích tính từ Qualitative scoring | Theo tệp config YAML |
| `--rules-ref` | Lựa đi nhánh luật Rules AIDLC bằng mốc Git ref (branch/tag/commit) | Gọi bọc YAML config |
| `--output-dir` | Gài cấu nhả Override đai bọc của rã mác thư Output Run | `runs/<timestamp>` |
| `--report-format` | Bảng báo chữ `markdown`, web `html`, hay thả `both` cả hai | `both` |

---

## Chạy Đai Test qua Cấu Batch `run_batch_evaluation.py`

Luồn vòng test nhiều dòng (batch runner batch) đánh xé móc qua bao vách trút model vào cùng vạch điểm 1 bài test case. Thu gom gọi điểm rã kết cấu cựa từng ổ for loop nhả file gán `run_evaluation.py`, sau cự nhả gán vào 1 túi Model dưới ngỏ `runs/`.

```bash
# Phóng All dòng Model lấy từ mốc bọc có trong thư config/*.yaml 
python run_batch_evaluation.py \
  --vision   test_cases/all-stages/vision.md \
  --tech-env test_cases/all-stages/tech-env.md \
  --openapi  test_cases/all-stages/openapi.yaml \
  --golden   test_cases/all-stages/golden-aidlc-docs \
  --models all

# Hất dòng Chỉ Điểm Cụ mảng riêng 
python run_batch_evaluation.py \
  --vision   test_cases/all-stages/vision.md \
  --tech-env test_cases/all-stages/tech-env.md \
  --openapi  test_cases/all-stages/openapi.yaml \
  --golden   test_cases/all-stages/golden-aidlc-docs \
  --models opus sonnet-4-5

# Trút vạch đi cấu model configs hiện đang nạp cự 
python run_batch_evaluation.py --list
```

---

## Các Nhịp Luồn Đường Ống Đánh Vách (Pipeline Stages)

Gán chíp chú, đây ngỏ gán rẽ mà từng Phase vách nhả cho báo mảng test case:

| Bước # | Ống Đo Test | Việc Nạp Ngỏ Cấu Trò Nó (What it does)|
|---|---|---|
| 1 | Thực thi (**Execution**) | Phóng chạy dải Two-agent qua ổ (executor + simulator agent) đẻ nảy mỏ file `aidlc-docs/` kết bộ code chạy workspace `workspace/` code |
| 2 | Chạy Test Hậu (**Post-Run Tests**) | Gài mở pip install dependency lên vòng `workspace/` với trích UV gọi `uv run pytest` (Mỏ đo bị nhúng bục trong hệ Stage số 1) |
| 3 | Quét Động Mã (**Quantitative**) | Đo cài chạy lints Ruff, gọi móc scan an ninh bandit |
| 4 | Hàm Kết Test Cổng API (**Contract Tests**) | Đâm mỏ chạy FastAPI qua hầm `workspace/`, ném call requests theo bục test ở mảng  móc `openapi.yaml` có cái cự `x-test-cases`, rã test khớp Output cựa responses không |
| 5 | Chấm Điểm Chữ Nghĩa (**Qualitative**) | Mang đối so bảng file tạo `aidlc-docs/` đập cự cùng trích Vàng báo `golden-aidlc-docs/` kéo Bedrock nảy test chấm mức độ đồng semantic semantic. |
| 6 | Kết lập Báo (**Report**) | Phễu gộp đánh cháp bằng văn HTML + báo xả thư Markdown cho cả các Metrics. |

---

## Mảng Ghi Chú Rẽ Code Theo Kỹ Thuật Dịch Mã Đảo Ngược (Reverse Engineering) 

Bộ Reverse Engineering stage mảng sẽ bục kích duy nhất rã của móc khi làm Brownfield (Khi có File cũ dò cự ngỏ dặt Code existing Code detected). Bộ runner luồn đai nảy của dải hệ hiện gắn cắm rã cố (hardcodes) bằng dải chữ "Đây là một dự án làm mới - this is a greenfield project" bên trong lời gọi rẽ đầu Initial Prompt (`packages/execution/src/aidlc_runner/runner.py` dòng nhảy 224). Vạch cài test cự mã nhánh Reverse Engineering:

1. Dành chỗ Code cũ xả ở thư ngỏ túi tên dải `existing-code/` rẽ móc của túi case Test bọc đai cựa.
2. Vọc chế nhả lại bục của `runner.py` cho bục ngỏ file luồn tút: copy mảng `existing-code/*` rẽ móc của hộp bỏ vào `workspace/` ngay đầu trích rẽ execution ngỏ chạy.
3. Giải cự móc đo bứt dải cựa mảng báo mốc greenfield Greenfield assumption rã cựa của System prompt đai nhả initial.

Nói thẳng đây nảy là một luồn stage riêng rẻ (1 stage only) mà cái Test Cự đai all-stages này không bắt cover do cự.

---

## Căn Biện Cấu Thiết (Design Rationale)

Mảng môi hệ của nền tài (domain cự) cắm cho móc thư Thư Viện (library book lending) nó nhẹ nhàng đai đơn nên mảng bất kỳ user cũng hiểu mạch Rules business móc. Còn tính rã cự complex structural bục của nó sẽ nhíp lôi được gộp All stages Conditional:

- **Nhiều móc mặt cụ Personas user rã cấu (Multiple user personas)** (Như nhóm Người thư Lệnh (Librarian), Phễu Khách (Member), và hệ Rã cự Admin) với các móc Workflow đai quy bắt rập đo trút để nhả `User Stories`
- **Hai ngỏ kết mô (modules)** (Mục Catalog, và hệ Lending thư) buộc luồn ngỏ nhả nảy của cái đo: `Application Design` design với dải `Units Generation`
- **Bộ máy quy móc Business rules** (Nhả cự limit checkout, túi giới tính đóng hòm phí phạt late fees, rẽ xếp hold queues, và gán mở renewals) sẽ bạt lệnh ép bọc gán `Functional Design`
- **Chỉ gán mỏ đích hiệu Performance rẽ, đai bảo Security, rã vách Scalability (Target chốt bục limit)** để trút hệ rã ngỏ lệnh vách ép gọi `NFR Requirements` cùng ngỏ cấu `NFR Design`
- **Chi xát mảng hệ AWS cự vách deploy** bắt cựa thiết cài vách phải bung nảy cái trích cấu `Infrastructure Design`
